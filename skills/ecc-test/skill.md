# /ecc-test - ECC テストフェーズ

テスト品質確保に必要な ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC モジュール: `workflow-quality` 相当 / コンテキスト: `dev.md` / フックレベル推奨: `standard`

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
鮮度チェック（30日超 → 警告、7日超 → 情報表示）。

## 自動配置

| 種別 | 名前 | 用途 |
|------|------|------|
| agent | `e2e-runner.md` | Playwright E2E テスト実行 |
| agent | `tdd-guide.md` | TDD 実践ガイド |
| skill | `tdd-workflow/` | TDD ワークフロー |
| context | `dev.md` | 実装モード |

## AskUserQuestion で選択 (multiSelect: true)

**「追加で導入するテスト関連コンポーネントを選んでください」**

| 選択肢 | 含まれるもの |
|--------|-------------|
| E2E テストパターン | skill: `e2e-testing/` |
| 評価ハーネス | skill: `eval-harness/` |
| AI 回帰テスト | skill: `ai-regression-testing/` |
| 検証ループ | skill: `verification-loop/` |
| コーディング規約 | skill: `coding-standards/` |
| ドキュメント検索 | agent: `docs-lookup.md` |

## 配置・報告

スキル → ジャンクション / エージェント・コンテキスト → コピー。既存はスキップ。
