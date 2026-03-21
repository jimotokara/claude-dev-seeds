# /ecc-docs - ECC ドキュメントフェーズ

ドキュメント整備に必要な ECC コンポーネントをプロジェクトの `.claude/` に配置する。
コンテキスト: `research.md` / フックレベル推奨: `minimal`

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
鮮度チェック。

## 自動配置

| 種別 | 名前 | 用途 |
|------|------|------|
| agent | `docs-lookup.md` | ドキュメント検索（Context7 MCP） |
| agent | `doc-updater.md` | ドキュメント・コードマップ同期 |
| context | `research.md` | 調査モード |

## AskUserQuestion で選択 (multiSelect: true)

**「追加で導入するドキュメント関連コンポーネントを選んでください」**

| 選択肢 | 含まれるもの |
|--------|-------------|
| コードベースオンボーディング | skill: `codebase-onboarding/` |
| ドキュメント検索 | skill: `documentation-lookup/` |
| プロジェクトガイドライン例 | skill: `project-guidelines-example/` |
| コーディング規約 | skill: `coding-standards/` |

## 配置・報告

スキル → ジャンクション / エージェント・コンテキスト → コピー。既存はスキップ。
