# /ecc-plan - ECC 企画・設計フェーズ

企画・設計に必要な ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC プロファイル: `core` + `research-apis` 相当 / コンテキスト: `research.md`

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
`S:\ECC\ecc-state.json` で鮮度チェック（30日超 → 警告、7日超 → 情報表示）。

## コンポーネント

### 自動配置（確認のみ）

| 種別 | 名前 | ECC ソース | 用途 |
|------|------|-----------|------|
| agent | `planner.md` | `agents/planner.md` | 実装計画策定 |
| agent | `architect.md` | `agents/architect.md` | アーキテクチャ判断 |
| context | `research.md` | `contexts/research.md` | 調査モード（理解優先） |

### AskUserQuestion で選択

**「追加で導入するコンポーネントを選んでください」** (multiSelect: true)

| 選択肢 | 含まれるもの |
|--------|-------------|
| リサーチツール | skills: `deep-research/`, `exa-search/` |
| 市場調査 | skills: `market-research/` |
| ドキュメント検索 | agent: `docs-lookup.md`, skill: `documentation-lookup/` |
| アーキテクチャ記録 | skill: `architecture-decision-records/` |
| コーディング規約 | skill: `coding-standards/` |

## 配置実行

既にプロジェクトに存在するものはスキップ。

スキル（ディレクトリ）→ ジャンクション:
```bash
powershell -Command "cmd /c 'mklink /J \".claude\skills\ecc-<name>\" \"S:\ECC\everything-claude-code\skills\<name>\"'"
```

エージェント/コンテキスト（ファイル）→ コピー:
```bash
mkdir -p .claude/agents .claude/contexts
cp "S:/ECC/everything-claude-code/agents/<name>.md" ".claude/agents/ecc-<name>.md"
cp "S:/ECC/everything-claude-code/contexts/<name>.md" ".claude/contexts/ecc-<name>.md"
```

## 報告

導入したコンポーネントの一覧を表示。`/ecc-status` で確認可能と案内。
