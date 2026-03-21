# /ecc-research - ECC リサーチフェーズ

調査・分析に必要な ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC プロファイル: `research` 相当 / コンテキスト: `research.md` / フックレベル推奨: `minimal`

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
鮮度チェック。

## 自動配置

| 種別 | 名前 | 用途 |
|------|------|------|
| skill | `deep-research/` | マルチソース深層リサーチ（firecrawl + exa MCP） |
| context | `research.md` | 調査モード（理解優先） |

## AskUserQuestion で選択 (multiSelect: true)

**「追加で導入するリサーチ関連コンポーネントを選んでください」**

| 選択肢 | 含まれるもの |
|--------|-------------|
| Exa ニューラル検索 | skill: `exa-search/` |
| 市場調査・競合分析 | skill: `market-research/` |
| Claude API パターン | skill: `claude-api/` |
| ドキュメント検索 | agent: `docs-lookup.md`, skill: `documentation-lookup/` |
| リサーチ優先ワークフロー | skill: `search-first/` |

## 配置・報告

スキル → ジャンクション / エージェント・コンテキスト → コピー。既存はスキップ。
