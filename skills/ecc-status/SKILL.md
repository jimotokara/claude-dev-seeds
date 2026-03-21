# /ecc-status - ECC プロジェクト状態表示

現在のプロジェクトに配置されている ECC コンポーネントの一覧と、ECC リポジトリの状態を表示する。

## 実行手順

### 1. ECC リポジトリの状態

`S:\ECC\ecc-state.json` を読み、バージョン・最終更新日を表示。
30日以上経過していれば `/ecc-setup` の実行を推奨。

### 2. プロジェクト内コンポーネントの一覧

プロジェクトの `.claude/` を確認し、以下をテーブル表示:

```bash
# エージェント
ls .claude/agents/ecc-*.md 2>/dev/null

# スキル
ls -d .claude/skills/ecc-*/ 2>/dev/null

# ルール
ls -d .claude/rules/ecc-*/ 2>/dev/null
ls .claude/rules/ecc-*.md 2>/dev/null

# コンテキスト
ls .claude/contexts/ecc-*.md 2>/dev/null
```

何もなければ「ECC コンポーネントは配置されていません。`/ecc-code` 等で導入してください。」と表示。

### 3. フェーズ推定

配置されているコンポーネントの組み合わせから、現在のフェーズを推定して表示:

| 検出パターン | 推定フェーズ |
|---|---|
| planner, architect | plan |
| tdd-guide, build-error-resolver, coding-standards | code |
| e2e-runner, eval-harness | test |
| code-reviewer, security-reviewer | review |
| docs-lookup, doc-updater, codebase-onboarding | docs |
| deep-research, exa-search | research |
| continuous-learning, strategic-compact | learn |
| article-writing, content-engine | content |
| deployment-patterns, dmux-workflows | ops |

### 4. ECC 本体のモジュール対応表示

`S:\ECC\everything-claude-code\manifests\install-modules.json` を参照し、配置済みコンポーネントがどのモジュールに属するかを表示。
