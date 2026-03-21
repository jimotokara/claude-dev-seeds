# /ecc-content - ECC コンテンツ制作フェーズ

記事執筆・コンテンツ配信・メディア生成のための ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC モジュール: `business-content` + `social-distribution` + `media-generation` 相当

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
鮮度チェック。

## AskUserQuestion で選択 (multiSelect: true)

**「導入するコンテンツ関連コンポーネントを選んでください」**

| 選択肢 | 含まれるもの |
|--------|-------------|
| 記事執筆 | skill: `article-writing/` |
| コンテンツエンジン（マルチプラットフォーム） | skill: `content-engine/` |
| 市場調査・競合分析 | skill: `market-research/` |
| 投資家向け資料 | skill: `investor-materials/` |
| 投資家アウトリーチ | skill: `investor-outreach/` |
| X/Twitter API 連携 | skill: `x-api/` |
| クロスポスト配信 | skill: `crosspost/` |
| プレゼンテーション | skill: `frontend-slides/` |
| AI メディア生成 (fal.ai) | skill: `fal-ai-media/` |
| 動画編集 | skill: `video-editing/` |
| VideoDB | skill: `videodb/` |

## 配置・報告

スキル → ジャンクション。既存はスキップ。
