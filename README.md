# claude-dev-seeds

Claude Code の開発環境を「育てる」ためのスキル集。

[everything-claude-code (ECC)](https://github.com/affaan-m/everything-claude-code) の全コンポーネント（スキル116+、エージェント28、コマンド59、ルール12言語）を、必要なときに必要なものだけ導入・切替する仕組みです。

## 設計思想

- **コマンドは最小限**に。フェーズ別コマンドは持たない
- `/tools` が状況を読み取り、必要なコンポーネントを対話的に提案・配置・削除
- 実際のスキル・エージェント・ルールは**プロジェクト単位で `.claude/` に配置**
- ECC に限定しない設計 — 将来的に別のツールソースも統合管理可能
- 不要になったコンポーネントは `/tools` 実行時に自動検出・削除提案

## コマンド一覧

```
/tools          スマートディスパッチャー（メインコマンド）
                状況読み取り → フェーズ提案 → コンポーネント選択 → 配置 → 不要分削除
/ecc-setup      ECC リポジトリのクローン・更新
/init-memory    プロジェクト初期化（CLAUDE.md, AGENTS.md 等を生成）
```

## アーキテクチャ

```
S:\ECC\everything-claude-code/     ← ECC 本体リポジトリ（倉庫）
  skills/                             116 スキル
  agents/                             28 エージェント
  contexts/                           コンテキストモード
  rules/                              ルール（12言語）

~/.claude/skills/                  ← グローバルスキル（3個のみ）
  tools/                              スマートディスパッチャー
  ecc-setup/                          リポジトリ管理
  init-memory/                        プロジェクト初期化

プロジェクト/.claude/              ← /tools が配置・削除
  agents/ecc-*.md                     (コピー)
  skills/ecc-*/                       (ジャンクション)
  rules/ecc-*                         (コピー)
  contexts/ecc-*.md                   (コピー)
```

## インストール

### 1. このリポジトリのスキルを配置

```bash
# クローン
git clone https://github.com/jimotokara/claude-dev-seeds.git

# グローバルスキルとしてコピー
# Windows（PowerShell）
$src = "<clone先>\skills"
$dst = "$env:USERPROFILE\.claude\skills"
Get-ChildItem "$src\tools", "$src\ecc-setup", "$src\init-memory" -Directory | ForEach-Object {
  Copy-Item $_.FullName "$dst\$($_.Name)" -Recurse -Force
}

# macOS / Linux
for skill in tools ecc-setup init-memory; do
  cp -r <clone先>/skills/$skill ~/.claude/skills/$skill
done
```

### 2. ECC リポジトリのセットアップ

Claude Code で以下を実行：

```
/ecc-setup
```

デフォルトで `S:\ECC\everything-claude-code` にクローンされます。
パスを変更する場合は、各スキルファイル内のパスを書き換えてください。

## 使い方

### 新プロジェクト開始時

```
/init-memory
```

対話的にプロジェクト情報を収集し、AGENTS.md, CLAUDE.md, .agent/, .spec/ 等を自動生成します。

### ツールの導入・切替

```
/tools
```

1. HANDOFF / MEMORY / TODO / チャット文脈から現状を把握
2. 次にやるべきフェーズを 2-3 個提案（AskUserQuestion）
3. 選んだフェーズに応じて ECC カタログから候補を特定
4. 導入するコンポーネントを提案（AskUserQuestion, multiSelect）
5. 選んだものを配置（スキル→ジャンクション、エージェント等→コピー）
6. 以前のフェーズで入れた不要なコンポーネントを検出・削除提案

### ECC リポジトリの更新

```
/ecc-setup
```

## カスタマイズ

### ECC クローン先の変更

各スキルファイル内の `S:\ECC\everything-claude-code` パスを変更してください。

### ツールソースの追加

`/tools` の skill.md 内「ツールソース」テーブルに新しいソースを追加することで、ECC 以外のスキルリポジトリも統合管理できます。

## 関連記事

- [Claude Codeのセッション記憶喪失問題を解決し続けたら、開発が劇的に変わった話](https://qiita.com/ProgrammingForEver/items/ee68fc621f7d1c53ffe1) by @ProgrammingForEver
- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) - ECC 本体リポジトリ

## ライセンス

MIT
