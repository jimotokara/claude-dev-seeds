# /ecc-phase - プロジェクトフェーズ別 ECC コンポーネント導入ガイド

everything-claude-code (ECC) のスキル・エージェントを、プロジェクトのライフサイクルフェーズに応じて段階的に導入する。

## 使い方

```
/ecc-phase <phase>
```

phase: `design` | `spec` | `implement` | `test` | `review` | `deploy` | `all`

引数なしで呼ぶとフェーズ一覧を表示する。

## 前提条件

- `/ecc-setup` が実行済みであること（ECC リポジトリのクローン + グローバルリンク済み）
- クローン先パスは環境変数 `ECC_REPO_PATH` またはデフォルト `~/.ecc/everything-claude-code` を使用
- 未セットアップの場合は `/ecc-setup` の実行を促す

## 導入方式

コンポーネントの導入は **シンボリックリンク** を使う。コピーではなくリンクすることで:
- ECC リポジトリの `git pull` だけで全プロジェクトに更新が反映される
- ディスク容量を節約できる
- 一元管理できる

グローバル（全プロジェクト共通）に入れるか、プロジェクト固有にするかはユーザーに確認する。

| 配置先 | 用途 | コマンド例 |
|---|---|---|
| `~/.claude/agents/ecc-*.md` | 全プロジェクトで使う | グローバル |
| `~/.claude/skills/ecc-*/` | 全プロジェクトで使う | グローバル |
| `.claude/agents/ecc-*.md` | このプロジェクトだけ | プロジェクト固有 |
| `.claude/skills/ecc-*/` | このプロジェクトだけ | プロジェクト固有 |

## 実行手順

### ステップ0: ECC 鮮度チェック

`~/.ecc/ecc-state.json`（ECC クローン先の親ディレクトリ） を読み、最終 pull からの経過日数を確認する。

- **30日以上**: 警告を表示し `/ecc-setup` の実行を推奨する
- **7日以上**: 情報として経過日数を表示する
- **7日未満**: 何も表示しない

`ecc-state.json` が存在しない場合は `/ecc-setup` が未実行なので、先に実行するよう促す。

### ステップ1: フェーズの特定

引数で渡されたフェーズ（または引数なしなら一覧表示）に基づき、下記のマッピング表から必要なコンポーネントを特定する。

### ステップ2: 技術スタックの確認

プロジェクトの技術スタックを確認する（package.json, *.csproj, go.mod, requirements.txt 等）。
スタックに無関係な言語固有コンポーネントは除外する。

### ステップ3: 既存コンポーネントの確認

`.claude/skills/` と `.claude/agents/` を確認し、既に導入済みのものはスキップする。

### ステップ4: 提案と確認

導入候補をテーブル形式でユーザーに提示し、確認を取る。

### ステップ5: 導入実行

ユーザーが承認したコンポーネントを ECC リポジトリからコピーする。

---

## フェーズ別コンポーネントマッピング

### Phase 1: `design` （設計・企画段階）

目的: アーキテクチャ決定、技術選定、要件整理

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| agent | `planner.md` | 機能実装の計画策定 |
| agent | `architect.md` | システム設計・アーキテクチャ判断 |
| skill | `market-research/` | 技術選定のための市場調査（必要に応じて） |
| skill | `deep-research/` | 深い技術調査 |

### Phase 2: `spec` （仕様確定段階）

目的: DB設計、API契約、状態遷移の確定

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| agent | `database-reviewer.md` | DB スキーマ・クエリ設計レビュー |
| skill | `postgres-patterns/` | PostgreSQL パターン（PG使用時） |
| skill | `database-migrations/` | マイグレーション戦略 |
| skill | `backend-patterns/` | API 設計パターン |
| skill | `coding-standards/` | 言語別コーディング規約の確立 |

### Phase 3: `implement` （実装段階）

目的: コード実装、パターン適用

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| skill | `frontend-patterns/` | React/Next.js 実装パターン |
| skill | `backend-patterns/` | API・DB・キャッシュパターン |
| skill | `docker-patterns/` | コンテナ構成（Docker使用時） |
| skill | `tdd-workflow/` | テスト駆動開発ワークフロー |
| agent | `tdd-guide.md` | TDD の実践ガイド |
| agent | `build-error-resolver.md` | ビルドエラーの解決 |
| skill | `verification-loop/` | 実装の検証ループ |

言語別（技術スタックに応じて選択）:

| 条件 | スキル | エージェント |
|------|--------|-------------|
| TypeScript | `coding-standards/` | `typescript-reviewer.md` |
| C# / .NET | `coding-standards/` | `code-reviewer.md` |
| Go | `golang-patterns/`, `golang-testing/` | `go-reviewer.md`, `go-build-resolver.md` |
| Python | `python-patterns/`, `python-testing/` | `python-reviewer.md` |
| Rust | `rust-patterns/`, `rust-testing/` | `rust-reviewer.md`, `rust-build-resolver.md` |
| Java | `springboot-patterns/` | `java-reviewer.md`, `java-build-resolver.md` |
| Kotlin | `kotlin-patterns/`, `kotlin-testing/` | `kotlin-reviewer.md`, `kotlin-build-resolver.md` |
| Django | `django-patterns/`, `django-tdd/` | `python-reviewer.md` |
| Laravel | `laravel-patterns/`, `laravel-tdd/` | `code-reviewer.md` |
| C++ | `cpp-coding-standards/`, `cpp-testing/` | `cpp-reviewer.md`, `cpp-build-resolver.md` |
| Flutter | — | `flutter-reviewer.md` |

### Phase 4: `test` （テスト段階）

目的: テスト品質の確保、E2E テスト

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| agent | `e2e-runner.md` | Playwright E2E テスト実行 |
| skill | `e2e-testing/` | E2E テストパターン |
| skill | `eval-harness/` | 評価ハーネス構築 |
| skill | `ai-regression-testing/` | AI 回帰テスト（AI機能がある場合） |

### Phase 5: `review` （レビュー・リリース準備段階）

目的: コード品質・セキュリティの最終確認

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| agent | `code-reviewer.md` | コード品質レビュー |
| agent | `security-reviewer.md` | セキュリティ脆弱性レビュー |
| agent | `refactor-cleaner.md` | デッドコード除去・リファクタ |
| agent | `doc-updater.md` | ドキュメント同期 |
| skill | `security-review/` | セキュリティチェックリスト |
| skill | `security-scan/` | 脆弱性スキャン |

### Phase 6: `deploy` （デプロイ段階）

目的: デプロイ準備、インフラ確認

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| skill | `deployment-patterns/` | デプロイ戦略パターン |
| skill | `docker-patterns/` | コンテナ最適化 |

### 常駐コンポーネント（全フェーズ共通）

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| skill | `coding-standards/` | コーディング規約（初期から） |
| agent | `docs-lookup.md` | ドキュメント検索 |
| skill | `continuous-learning-v2/` | セッション学習（任意） |

---

## 導入コマンド例

シンボリックリンクで ECC リポジトリを参照する:

```powershell
$eccPath = "~/.ecc/everything-claude-code"

# グローバルに配置（全プロジェクト共通）
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.claude\agents\ecc-<agent-name>.md" -Target "$eccPath\agents\<agent-name>.md"
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.claude\skills\ecc-<skill-name>" -Target "$eccPath\skills\<skill-name>"

# プロジェクト固有に配置
New-Item -ItemType SymbolicLink -Path ".claude\agents\ecc-<agent-name>.md" -Target "$eccPath\agents\<agent-name>.md"
New-Item -ItemType SymbolicLink -Path ".claude\skills\ecc-<skill-name>" -Target "$eccPath\skills\<skill-name>"
```

```bash
ECC_PATH="$HOME/.ecc/everything-claude-code"

# グローバルに配置
ln -s "$ECC_PATH/agents/<agent-name>.md" "$HOME/.claude/agents/ecc-<agent-name>.md"
ln -s "$ECC_PATH/skills/<skill-name>" "$HOME/.claude/skills/ecc-<skill-name>"

# プロジェクト固有に配置
ln -s "$ECC_PATH/agents/<agent-name>.md" ".claude/agents/ecc-<agent-name>.md"
ln -s "$ECC_PATH/skills/<skill-name>" ".claude/skills/ecc-<skill-name>"
```

---

## 注意事項

- 導入前に既存の CLAUDE.md / AGENTS.md との競合がないか確認する
- プロジェクト固有のルール（禁止事項等）が ECC のデフォルトと矛盾する場合はプロジェクト側を優先
- 不要になったコンポーネントは積極的に削除してコンテキストを軽量に保つ
- ECC のバージョンアップ時は差分を確認してからマージする
