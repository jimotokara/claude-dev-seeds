# /tools - スマートツールディスパッチャー

プロジェクトの現在の状況を読み取り、次に必要なスキル・エージェントだけを選択導入する。
不要になったコンポーネントの削除も行う。

## ツールソース

| ソース | パス | 内容 |
|---|---|---|
| ECC | `S:\ECC\everything-claude-code` | 116 スキル、28 エージェント、コンテキスト、ルール |

将来的にソースが増えた場合はこのテーブルに追加する。

## 前提

- 少なくとも1つのツールソースが存在すること（なければ `/ecc-setup` 等を案内）
- プロジェクトの `.claude/` ディレクトリが存在すること

## 実行フロー

### Step 1: 現状把握

以下を読み取り、プロジェクトの現在の状況を把握する:

- `.agent/handoff/HANDOFF.md` — 前回セッションの引き継ぎ
- `.agent/memory/MEMORY.md` — 蓄積された知見
- 会話の文脈 — 今のチャットで何を話しているか
- 現在のTODOリスト — 進行中のタスク
- `.spec/` — 進行中の仕様があれば

既にプロジェクトに配置されているコンポーネントも確認:

```bash
ls .claude/agents/ecc-*.md 2>/dev/null
ls -d .claude/skills/ecc-*/ 2>/dev/null
ls -d .claude/rules/ecc-*/ 2>/dev/null
ls .claude/rules/ecc-*.md 2>/dev/null
ls .claude/contexts/ecc-*.md 2>/dev/null
```

### Step 2: フェーズ提案

状況から判断して、次にやるべきフェーズを 2-3 個 AskUserQuestion で提案する。

フェーズの例（これに限定しない、状況に応じて自由に提案）:

| フェーズ | 典型的なシナリオ |
|---|---|
| 設計・企画 | 新機能の設計、アーキテクチャ判断 |
| 実装 | コーディング、バグ修正 |
| テスト | テスト作成、TDD |
| レビュー | コードレビュー、セキュリティレビュー |
| ドキュメント | ドキュメント整備、オンボーディング |
| リサーチ | 技術調査、ライブラリ評価 |
| デプロイ・運用 | デプロイ準備、CI/CD |
| リファクタ | コード整理、品質改善 |

### Step 3: ツールカタログ確認

選択されたフェーズに基づき、全ツールソースの中から関連するコンポーネントを探す。

**ECC リポジトリの構造:**
```
S:\ECC\everything-claude-code\
  skills/     — スキル（ディレクトリ）
  agents/     — エージェント（.md ファイル）
  contexts/   — コンテキストモード（.md ファイル）
  rules/      — ルール（ディレクトリ or .md ファイル）
```

各コンポーネントの skill.md や .md ファイルの冒頭を読み、フェーズに合うものを特定する。

### Step 4: コンポーネント提案

AskUserQuestion (multiSelect: true) で、導入候補のコンポーネントを提案する。

- 各選択肢に「何のために使うか」を description で説明する
- 既にプロジェクトに配置済みのものは「(配置済み)」と明記してスキップ対象にする
- 多すぎる場合は最も有用な 4 つに絞る
- どのソースから来るかを明記する（例: `[ECC] coding-standards`）

### Step 5: 配置実行

選択されたコンポーネントを配置する。

**スキル（ディレクトリ）→ ジャンクション:**
```bash
powershell -Command "cmd /c 'mklink /J \".claude\skills\ecc-<name>\" \"S:\ECC\everything-claude-code\skills\<name>\"'"
```

**エージェント（ファイル）→ コピー:**
```bash
mkdir -p .claude/agents
cp "S:/ECC/everything-claude-code/agents/<name>.md" ".claude/agents/ecc-<name>.md"
```

**コンテキスト（ファイル）→ コピー:**
```bash
mkdir -p .claude/contexts
cp "S:/ECC/everything-claude-code/contexts/<name>.md" ".claude/contexts/ecc-<name>.md"
```

**ルール → コピー:**
```bash
mkdir -p .claude/rules
# ディレクトリの場合
cp -r "S:/ECC/everything-claude-code/rules/<name>" ".claude/rules/ecc-<name>"
# ファイルの場合
cp "S:/ECC/everything-claude-code/rules/<name>.md" ".claude/rules/ecc-<name>.md"
```

### Step 6: 不要コンポーネントの検出・削除

配置済みのコンポーネント（Step 1 で確認したもの）の中で、今回選択したフェーズに不要なものを検出する。

- 今回新たに配置したものは残す
- 明らかに今のフェーズに不要なものを AskUserQuestion で削除提案する
- ユーザーが選択したものだけ削除する

削除方法:
```bash
# ジャンクション（スキル）の削除
powershell -Command "cmd /c 'rmdir \".claude\skills\ecc-<name>\"'"

# コピー（エージェント/コンテキスト/ルール）の削除
rm ".claude/agents/ecc-<name>.md"
rm ".claude/contexts/ecc-<name>.md"
rm -rf ".claude/rules/ecc-<name>"
```

不要なものがなければこのステップはスキップ。

### Step 7: 報告

導入・削除したコンポーネントの一覧をテーブル表示する:

```
【ツール更新】
フェーズ: <選択したフェーズ>

追加:
  + [ECC] skill: coding-standards (コーディング規約)
  + [ECC] agent: typescript-reviewer.md (TSレビュー)

削除:
  - [ECC] agent: planner.md (前フェーズ用)

現在の配置:
  agents: ecc-typescript-reviewer.md
  skills: ecc-coding-standards/
```
