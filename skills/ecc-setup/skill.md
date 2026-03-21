# /ecc-setup - ECC グローバルセットアップ

everything-claude-code (ECC) リポジトリをクローンし、頻用コンポーネントをグローバルにシンボリックリンクする。
初回に1度だけ実行すれば、以降は全プロジェクトから ECC のスキル・エージェントが利用可能になる。

## 前提

- Git がインストール済み
- Windows の場合、管理者権限または開発者モードが有効（シンボリックリンク作成に必要）

## 実行手順

### ステップ1: ECC リポジトリのクローンまたは更新

```powershell
$eccPath = "~/.ecc/everything-claude-code"
if (-not (Test-Path $eccPath)) {
  New-Item -ItemType Directory -Path (Split-Path $eccPath -Parent) -Force | Out-Null
  git clone https://github.com/affaan-m/everything-claude-code $eccPath
} else {
  Set-Location $eccPath
  git pull
}
```

### ステップ1.5: バージョンチェック

クローンまたは更新後、以下の情報を取得してユーザーに報告する。

```powershell
Set-Location "~/.ecc/everything-claude-code"

# 現在のコミットハッシュとタグ
$currentHash = git rev-parse --short HEAD
$currentDate = git log -1 --format="%ci"
$latestTag = git describe --tags --abbrev=0 2>$null
if (-not $latestTag) { $latestTag = "(タグなし)" }

# リモートとの差分（fetch済みなので比較可能）
$behind = git rev-list --count "HEAD..origin/main" 2>$null
if (-not $behind) { $behind = 0 }

# 前回の pull からの経過日数
$lastPullDate = [datetime](git log -1 --format="%ci")
$daysSincePull = ((Get-Date) - $lastPullDate).Days

# 最終 pull 日時を記録（他スキルからの経過日数チェック用）
$stateDir = Split-Path $eccPath -Parent
@{ lastPull = (Get-Date).ToString("o"); version = $latestTag; hash = $currentHash } |
  ConvertTo-Json | Set-Content "$stateDir\ecc-state.json" -Encoding UTF8
```

以下のフォーマットで報告する:

```
ECC バージョン情報:
  バージョン:     {$latestTag}
  コミット:       {$currentHash}
  コミット日時:   {$currentDate}
  最終更新からの経過: {$daysSincePull} 日
```

更新があった場合（`git pull` で変更を取得した場合）は、変更サマリーも表示する:

```powershell
# 直近の変更サマリー（更新があった場合のみ）
git log --oneline -10
```

新しいスキルやエージェントが追加されていた場合は、ユーザーに通知する:

```powershell
# 新規追加されたスキル/エージェントの検出（直近の更新分）
git diff --name-status HEAD@{1}..HEAD -- skills/ agents/ 2>$null |
  Where-Object { $_ -match "^A" }
```

### ステップ2: グローバル agents ディレクトリの作成

```powershell
$globalAgents = "$env:USERPROFILE\.claude\agents"
if (-not (Test-Path $globalAgents)) {
  New-Item -ItemType Directory -Path $globalAgents -Force
}
```

### ステップ3: コアエージェントのシンボリックリンク作成

以下のエージェントは言語非依存で汎用性が高いため、常にグローバルに配置する。
既にリンクまたはファイルが存在する場合はスキップする。

```powershell
$coreAgents = @(
  "planner",
  "architect",
  "code-reviewer",
  "security-reviewer",
  "database-reviewer",
  "build-error-resolver",
  "doc-updater",
  "refactor-cleaner",
  "e2e-runner",
  "docs-lookup",
  "tdd-guide"
)

foreach ($agent in $coreAgents) {
  $target = "~/.ecc/everything-claude-code\agents\$agent.md"
  $link = "$globalAgents\ecc-$agent.md"
  if ((Test-Path $target) -and -not (Test-Path $link)) {
    New-Item -ItemType SymbolicLink -Path $link -Target $target
    Write-Host "Linked: $agent"
  }
}
```

注意: エージェントファイル名に `ecc-` プレフィックスを付けることで、プロジェクト固有のエージェントとの名前衝突を防ぐ。

### ステップ4: 言語別エージェントの選択的リンク

ユーザーに「よく使う言語」を聞き、該当するレビュアーとビルドリゾルバーをリンクする。

| 言語 | レビュアー | ビルドリゾルバー |
|------|-----------|----------------|
| TypeScript | typescript-reviewer.md | — |
| Python | python-reviewer.md | pytorch-build-resolver.md |
| Go | go-reviewer.md | go-build-resolver.md |
| Rust | rust-reviewer.md | rust-build-resolver.md |
| Java | java-reviewer.md | java-build-resolver.md |
| Kotlin | kotlin-reviewer.md | kotlin-build-resolver.md |
| C++ | cpp-reviewer.md | cpp-build-resolver.md |
| Flutter | flutter-reviewer.md | — |

### ステップ5: コアスキルのシンボリックリンク作成

以下の汎用スキルをグローバルに配置する:

```powershell
$coreSkills = @(
  "security-review",
  "security-scan",
  "tdd-workflow",
  "verification-loop",
  "deep-research",
  "coding-standards"
)

$globalSkills = "$env:USERPROFILE\.claude\skills"
foreach ($skill in $coreSkills) {
  $target = "~/.ecc/everything-claude-code\skills\$skill"
  $link = "$globalSkills\ecc-$skill"
  if ((Test-Path $target) -and -not (Test-Path $link)) {
    New-Item -ItemType SymbolicLink -Path $link -Target $target
    Write-Host "Linked skill: $skill"
  }
}
```

### ステップ6: 言語別スキルの選択的リンク

ステップ4で選択した言語に応じて、対応するスキルもリンクする。
（マッピングは /ecc-phase の Phase 3: implement セクションを参照）

### ステップ7: 確認と報告

リンクされたコンポーネントの一覧を表示する:

```powershell
Write-Host "`n=== Linked Agents ===" -ForegroundColor Green
Get-ChildItem "$globalAgents\ecc-*" | ForEach-Object { Write-Host "  $($_.Name)" }

Write-Host "`n=== Linked Skills ===" -ForegroundColor Green
Get-ChildItem "$globalSkills\ecc-*" | ForEach-Object { Write-Host "  $($_.Name)" }
```

## 更新方法

ECC リポジトリを更新するだけで、シンボリックリンク経由で全プロジェクトに反映される:

```powershell
Set-Location "~/.ecc/everything-claude-code"
git pull
```

## アンインストール

```powershell
# エージェントのリンク削除
Get-ChildItem "$env:USERPROFILE\.claude\agents\ecc-*" | Remove-Item
# スキルのリンク削除
Get-ChildItem "$env:USERPROFILE\.claude\skills\ecc-*" | Remove-Item -Recurse
# リポジトリ削除（任意）
Remove-Item -Recurse (Split-Path $eccPath -Parent)
```
