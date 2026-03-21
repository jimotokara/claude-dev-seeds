# /ecc-implement - ECC 実装フェーズ導入

コード実装・パターン適用段階で必要な ECC コンポーネントをプロジェクトに導入する。

## 前提条件

- `/ecc-setup` が実行済みであること
- 未セットアップの場合は `/ecc-setup` の実行を促す

## 実行手順

### ステップ0: ECC 鮮度チェック

ECC クローン先の親ディレクトリ（デフォルト `~/.ecc/`）にある `ecc-state.json` を読み、最終 pull からの経過日数を確認する。

- **30日以上**: 警告を表示し `/ecc-setup` の実行を推奨する
- **7日以上**: 情報として経過日数を表示する
- **7日未満**: 何も表示しない
- `ecc-state.json` が存在しない場合は `/ecc-setup` が未実行なので、先に実行するよう促して中断する

### ステップ1: 技術スタックの確認

プロジェクトの技術スタックを確認する（package.json, *.csproj, go.mod, requirements.txt 等）。
スタックに無関係な言語固有コンポーネントは除外する。

### ステップ2: 既存コンポーネントの確認

`~/.claude/skills/`, `~/.claude/agents/`, `.claude/skills/`, `.claude/agents/` を確認し、既に導入済みのものはスキップする。

### ステップ3: 導入候補の提示

以下の implement フェーズ用コンポーネントから、未導入のものを提示する:

**共通:**

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| skill | `frontend-patterns/` | React/Next.js 実装パターン |
| skill | `backend-patterns/` | API・DB・キャッシュパターン |
| skill | `docker-patterns/` | コンテナ構成（Docker使用時） |
| skill | `tdd-workflow/` | テスト駆動開発ワークフロー |
| agent | `tdd-guide.md` | TDD の実践ガイド |
| agent | `build-error-resolver.md` | ビルドエラーの解決 |
| skill | `verification-loop/` | 実装の検証ループ |

**言語別（技術スタックに応じて選択）:**

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
| Flutter | -- | `flutter-reviewer.md` |

常駐コンポーネント（未導入なら追加提案）:

| 種別 | コンポーネント | 用途 |
|------|---------------|------|
| skill | `coding-standards/` | コーディング規約 |
| agent | `docs-lookup.md` | ドキュメント検索 |

ユーザーに導入先（グローバル / プロジェクト固有）を確認する。

### ステップ4: 導入実行

ユーザーが承認したコンポーネントを ECC リポジトリからコピーする。

- グローバル: `~/.claude/agents/ecc-*.md`, `~/.claude/skills/ecc-*/`
- プロジェクト固有: `.claude/agents/ecc-*.md`, `.claude/skills/ecc-*/`
- `ecc-` プレフィックスを付けて名前衝突を防ぐ

### ステップ5: 確認と報告

導入したコンポーネントの一覧を表示する。

## 注意事項

- プロジェクト固有のルール（禁止事項等）が ECC のデフォルトと矛盾する場合はプロジェクト側を優先
- 不要になったコンポーネントは `/ecc-clean` で削除してコンテキストを軽量に保つ
