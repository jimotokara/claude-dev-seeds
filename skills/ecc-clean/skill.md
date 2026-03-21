# /ecc-clean - ECC コンポーネントクリーンアップ

**プロジェクトの `.claude/`** から ECC コンポーネントを削除し、コンテキストを軽量に保つ。
グローバルのフェーズ管理コマンドには触れない。

## 実行手順

### 1. 現在のコンポーネントを一覧取得

`.claude/agents/ecc-*`, `.claude/skills/ecc-*`, `.claude/rules/ecc-*`, `.claude/contexts/ecc-*` を確認。
何もなければ「ECC コンポーネントはありません」と報告して終了。

### 2. 削除方法の選択

AskUserQuestion:

**「どのように削除しますか？」**
- `全削除` — プロジェクト内の ecc-* を全て削除
- `フェーズ指定` — 残すフェーズを選んでそれ以外を削除
- `個別選択` — 削除するものを個別に選択

### 3. フェーズ指定の場合

AskUserQuestion (multiSelect: true):

**「残すフェーズを選んでください」**
- plan, code, test, review, docs, research, learn, content, ops

### 4. フェーズ別コンポーネントマッピング

**plan:** agents: planner, architect / skills: deep-research, market-research
**code:** agents: tdd-guide, build-error-resolver, docs-lookup / skills: coding-standards, tdd-workflow, verification-loop, frontend-patterns, backend-patterns, docker-patterns + 言語別全て / rules: common/*, 言語別 / contexts: dev.md
**test:** agents: e2e-runner / skills: e2e-testing, eval-harness, ai-regression-testing
**review:** agents: code-reviewer, security-reviewer, refactor-cleaner, doc-updater / skills: security-review, security-scan / contexts: review.md
**docs:** agents: docs-lookup, doc-updater / skills: codebase-onboarding, documentation-lookup
**research:** skills: deep-research, exa-search, market-research / contexts: research.md
**learn:** skills: continuous-learning, continuous-learning-v2, strategic-compact, context-budget, iterative-retrieval
**content:** skills: article-writing, content-engine, crosspost, fal-ai-media, video-editing, frontend-slides, x-api, investor-materials, investor-outreach
**ops:** agents: chief-of-staff, loop-operator, harness-optimizer / skills: deployment-patterns, docker-patterns, dmux-workflows, autonomous-loops

複数フェーズに属するコンポーネントは、選択されたフェーズに含まれていれば残す。

### 5. 確認と実行

削除対象をテーブル表示し、ユーザー確認後に削除:
```bash
rm -rf ".claude/skills/ecc-<name>"
rm -f ".claude/agents/ecc-<name>.md"
rm -f ".claude/rules/ecc-<name>.md"
rm -rf ".claude/rules/ecc-<name>"
rm -f ".claude/contexts/ecc-<name>.md"
```

### 6. 結果報告

削除したコンポーネントと残っているコンポーネントの一覧を表示。
