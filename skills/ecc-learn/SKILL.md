# /ecc-learn - ECC 継続的学習・コンテキスト管理

継続的学習とコンテキスト最適化のための ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC モジュール: `workflow-quality` の学習系 / フックレベル推奨: `standard`

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
鮮度チェック。

## 自動配置

| 種別 | 名前 | 用途 |
|------|------|------|
| skill | `continuous-learning-v2/` | instinct ベースの自動学習（信頼度スコア付き） |
| skill | `strategic-compact/` | 戦略的コンテキスト圧縮提案 |

## AskUserQuestion で選択 (multiSelect: true)

**「追加で導入する学習・最適化コンポーネントを選んでください」**

| 選択肢 | 含まれるもの |
|--------|-------------|
| 継続的学習 v1（レガシー） | skill: `continuous-learning/` |
| コンテキスト予算管理 | skill: `context-budget/` |
| 反復的コンテキスト精製 | skill: `iterative-retrieval/` |
| 評価ハーネス | skill: `eval-harness/` |
| スキル棚卸し | skill: `skill-stocktake/` |

## 関連する ECC コマンド

このフェーズのコンポーネントは以下の ECC 本体コマンドと連携:
- `/learn` — セッション中のパターン抽出
- `/instinct-status` — 学習した instinct を表示
- `/instinct-import`, `/instinct-export` — instinct の共有
- `/evolve` — instinct をスキルにクラスタリング
- `/checkpoint`, `/verify` — 検証ループ

これらのコマンドは ECC 本体の `commands/` にあり、本体をプラグインインストールするか、
手動で `~/.claude/commands/` にコピーすることで利用可能。

## 配置・報告

スキル → ジャンクション。既存はスキップ。
