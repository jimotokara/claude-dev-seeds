# claude-dev-seeds

Claude Code の開発環境を「育てる」ためのスキル集。

[everything-claude-code (ECC)](https://github.com/affaan-m/everything-claude-code) の全コンポーネント（スキル116+、エージェント28、コマンド59、ルール12言語）を、プロジェクトのフェーズに合わせて必要なものだけ導入・切替する仕組みです。

## 設計思想

- **グローバルにはフェーズ管理コマンドだけ**配置し、コンテキストを軽量に保つ
- 実際のスキル・エージェント・ルールは**プロジェクト単位で `.claude/` に配置**
- 各コマンド内で **AskUserQuestion による対話的な選択**（言語・サブカテゴリ）
- ECC 本体のプロファイル / モジュール / コンテキスト / フックレベルと整合
- 不要になったら `/ecc-clean` で削除 → コンテキストウィンドウを常に最適化

## コマンド一覧

```
メタ（3）
  /ecc-setup      ECC リポジトリ管理（クローン・更新）
  /ecc-status     プロジェクト内の ECC コンポーネント状態表示
  /ecc-clean      プロジェクト内の ECC コンポーネント削除

フェーズ（9）
  /ecc-plan       企画・設計（planner, architect, deep-research）
  /ecc-code       実装（TDD, 言語別パターン, コーディング規約）
  /ecc-test       テスト（e2e-runner, eval-harness）
  /ecc-review     レビュー・品質（security, code-reviewer）
  /ecc-docs       ドキュメント（docs-lookup, codebase-onboarding）
  /ecc-research   リサーチ（deep-research, exa-search）
  /ecc-learn      継続的学習（instinct, strategic-compact）
  /ecc-content    コンテンツ制作（article, media, crosspost）
  /ecc-ops        オーケストレーション・デプロイ（dmux, agentic patterns）

その他
  /init-memory    プロジェクト初期化（CLAUDE.md, AGENTS.md 等を生成）
```

## アーキテクチャ

```
S:\ECC\everything-claude-code/     ← ECC 本体リポジトリ（倉庫）
  manifests/install-modules.json      モジュール定義
  manifests/install-profiles.json     プロファイル定義（core/developer/security/research/full）
  contexts/*.md                       コンテキストモード（dev/review/research）
  agents/, skills/, commands/, rules/ 実コンポーネント

~/.claude/skills/                  ← フェーズ管理コマンドのみ（12個）
  ecc-setup/, ecc-status/, ecc-clean/
  ecc-plan/, ecc-code/, ecc-test/, ecc-review/
  ecc-docs/, ecc-research/, ecc-learn/, ecc-content/, ecc-ops/

プロジェクト/.claude/              ← 各フェーズコマンドが配置・削除
  agents/ecc-*.md                     (コピー)
  skills/ecc-*/                       (ジャンクション or コピー)
  rules/*.md                          (コピー)
  contexts/ecc-*.md                   (コピー)
```

### ECC 本体との対応

| コマンド | ECC プロファイル | コンテキスト | フックレベル |
|---|---|---|---|
| ecc-plan | core + research-apis | research.md | minimal |
| ecc-code | developer + 言語別 | dev.md | standard |
| ecc-test | workflow-quality | dev.md | standard |
| ecc-review | core + security | review.md | strict |
| ecc-docs | core | research.md | minimal |
| ecc-research | research | research.md | minimal |
| ecc-learn | workflow-quality(学習系) | — | standard |
| ecc-content | business-content + social + media | — | minimal |
| ecc-ops | orchestration + devops + agentic | dev.md | standard |

## インストール

### 1. このリポジトリのスキルを配置

```bash
# クローン
git clone https://github.com/jimotokara/claude-dev-seeds.git

# グローバルスキルとしてコピー
# Windows（PowerShell）
$src = "<clone先>\skills"
$dst = "$env:USERPROFILE\.claude\skills"
Get-ChildItem "$src\ecc-*", "$src\init-memory" -Directory | ForEach-Object {
  Copy-Item $_.FullName "$dst\$($_.Name)" -Recurse -Force
}

# macOS / Linux
for skill in ecc-setup ecc-status ecc-clean ecc-plan ecc-code ecc-test ecc-review ecc-docs ecc-research ecc-learn ecc-content ecc-ops init-memory; do
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

### フェーズに応じたコンポーネント導入

```bash
/ecc-plan       # 企画段階: planner, architect, deep-research
/ecc-code       # 実装段階: 言語自動検出 → 対話で選択
/ecc-test       # テスト段階: e2e-runner, eval-harness
/ecc-review     # レビュー: security-reviewer, code-reviewer
```

各コマンドは技術スタックを自動検出し、AskUserQuestion で追加コンポーネントを対話的に選択できます。

### フェーズの切り替え

```bash
/ecc-clean      # 不要コンポーネントを削除（全削除 / フェーズ指定 / 個別選択）
/ecc-code       # 新しいフェーズのコンポーネントを導入
```

### 状態確認

```bash
/ecc-status     # 現在のプロジェクト内 ECC コンポーネント一覧
```

## ECC 本体への貢献

このプロジェクトの「フェーズ別コンポーネント管理」は、ECC 本体の以下の Issue に関連しています：

- [#657](https://github.com/affaan-m/everything-claude-code/issues/657) — token optimization, lazy-load skills, context scoping
- [#421](https://github.com/affaan-m/everything-claude-code/issues/421) — install-state, uninstall/doctor/repair lifecycle
- [#423](https://github.com/affaan-m/everything-claude-code/issues/423) — manifest-driven selective install profiles

## カスタマイズ

### ECC クローン先の変更

各スキルファイル内の `S:\ECC\everything-claude-code` パスを変更してください。

### バージョンチェックの閾値

デフォルトでは 30 日で警告、7 日で情報表示です。
各フェーズスキルの鮮度チェック部分で変更できます。

## 関連記事

- [Claude Codeのセッション記憶喪失問題を解決し続けたら、開発が劇的に変わった話](https://qiita.com/ProgrammingForEver/items/ee68fc621f7d1c53ffe1) by @ProgrammingForEver
- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) - ECC 本体リポジトリ

## ライセンス

MIT
