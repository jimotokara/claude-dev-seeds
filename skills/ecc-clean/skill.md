# /ecc-clean - ECC コンポーネントクリーンアップ

不要な ECC コンポーネントを削除し、コンテキストを軽量に保つ。

## 実行手順

### ステップ1: 現在の ECC コンポーネントを一覧取得

グローバル（`~/.claude/agents/ecc-*`, `~/.claude/skills/ecc-*`）とプロジェクト固有（`.claude/agents/ecc-*`, `.claude/skills/ecc-*`）の両方を確認し、現在導入済みのコンポーネントを一覧する。

### ステップ2: フェーズ選択

ユーザーに AskUserQuestion で質問する:

**「現在のフェーズを選択してください（選択したフェーズのコンポーネントだけ残し、他は削除します）」**

選択肢:
- `design` - 設計・企画
- `spec` - 仕様確定
- `implement` - 実装
- `test` - テスト
- `review` - レビュー・リリース準備
- `deploy` - デプロイ
- Other → ステップ2b へ

選択されたフェーズに属するコンポーネント + 常駐コンポーネントだけを残し、それ以外の ECC コンポーネントを全て削除する。

### ステップ2b: カスタム削除（Other 選択時）

AskUserQuestion（multiSelect: true）で質問する:

**「削除するフェーズを選択してください」**

選択肢:
- `design` のコンポーネント
- `spec` のコンポーネント
- `implement` のコンポーネント
- `test` のコンポーネント
- `review` のコンポーネント
- `deploy` のコンポーネント

選択されたフェーズのコンポーネントのみ削除する。

### ステップ3: 削除対象の特定

フェーズ別コンポーネントマッピング:

**design:**
- agent: `planner.md`, `architect.md`
- skill: `market-research/`, `deep-research/`

**spec:**
- agent: `database-reviewer.md`
- skill: `postgres-patterns/`, `database-migrations/`, `backend-patterns/`

**implement:**
- skill: `frontend-patterns/`, `docker-patterns/`, `tdd-workflow/`, `verification-loop/`, `backend-patterns/`
- agent: `tdd-guide.md`, `build-error-resolver.md`
- 言語別スキル・エージェント（`typescript-reviewer`, `rust-reviewer`, `rust-build-resolver`, `rust-patterns/`, `rust-testing/`, `golang-*`, `python-*`, `springboot-*`, `kotlin-*`, `django-*`, `laravel-*`, `cpp-*`, `flutter-*` 等）

**test:**
- agent: `e2e-runner.md`
- skill: `e2e-testing/`, `eval-harness/`, `ai-regression-testing/`

**review:**
- agent: `code-reviewer.md`, `security-reviewer.md`, `refactor-cleaner.md`, `doc-updater.md`
- skill: `security-review/`, `security-scan/`

**deploy:**
- skill: `deployment-patterns/`, `docker-patterns/`

**常駐（常に残す、どのフェーズでも削除しない）:**
- skill: `coding-standards/`, `continuous-learning-v2/`
- agent: `docs-lookup.md`

注意: 複数フェーズに属するコンポーネント（例: `backend-patterns/` は spec と implement 両方、`docker-patterns/` は implement と deploy 両方）は、選択されたフェーズに含まれていれば残す。

### ステップ4: 確認と実行

削除対象のコンポーネント一覧をテーブルで表示し、ユーザーの確認を取ってから削除する。

削除はグローバルとプロジェクト固有の両方で行う（存在する方を削除）。

### ステップ5: 結果報告

削除したコンポーネントと、残っているコンポーネントの一覧を表示する。
