# /ecc-ops - ECC オーケストレーション・デプロイフェーズ

マルチエージェント運用・デプロイ・インフラ関連の ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC モジュール: `orchestration` + `devops-infra` + `agentic-patterns` 相当

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
鮮度チェック。

## AskUserQuestion で選択 (multiSelect: true)

**「導入するオペレーション関連コンポーネントを選んでください」**

### デプロイ・インフラ

| 選択肢 | 含まれるもの |
|--------|-------------|
| デプロイパターン | skill: `deployment-patterns/` |
| Docker パターン | skill: `docker-patterns/` |

### オーケストレーション

| 選択肢 | 含まれるもの |
|--------|-------------|
| dmux マルチエージェント | skill: `dmux-workflows/` |
| 自律ループ | skill: `autonomous-loops/` |
| 継続的エージェントループ | skill: `continuous-agent-loop/` |
| チーフオブスタッフ | agent: `chief-of-staff.md` |
| ループオペレーター | agent: `loop-operator.md` |
| ハーネスオプティマイザー | agent: `harness-optimizer.md` |

### エージェンティックパターン

| 選択肢 | 含まれるもの |
|--------|-------------|
| エージェントハーネス構築 | skill: `agent-harness-construction/` |
| エージェンティックエンジニアリング | skill: `agentic-engineering/` |
| AI ファーストエンジニアリング | skill: `ai-first-engineering/` |
| コスト最適化 LLM パイプライン | skill: `cost-aware-llm-pipeline/` |
| プロンプト最適化 | skill: `prompt-optimizer/` |
| チームビルダー | skill: `team-builder/` |
| Claude Devfleet | skill: `claude-devfleet/` |
| データスクレイパーエージェント | skill: `data-scraper-agent/` |

## 関連する ECC 本体コマンド

- `/pm2` — PM2 サービスライフサイクル管理
- `/multi-plan`, `/multi-execute` — マルチエージェントタスク分解・実行
- `/multi-backend`, `/multi-frontend`, `/multi-workflow` — マルチサービスオーケストレーション
- `/orchestrate` — ワークツリーベースの並列実行

## 配置・報告

スキル → ジャンクション / エージェント → コピー。既存はスキップ。
