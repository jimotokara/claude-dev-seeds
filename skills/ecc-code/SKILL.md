# /ecc-code - ECC 実装フェーズ

コード実装に必要な ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC プロファイル: `developer` 相当 / コンテキスト: `dev.md` / フックレベル推奨: `standard`

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
`S:\ECC\ecc-state.json` で鮮度チェック（30日超 → 警告、7日超 → 情報表示）。

## ステップ1: 技術スタック自動検出

プロジェクト内のファイルを確認し、技術スタックを推定:
- `*.php`, `composer.json` → PHP
- `package.json`, `*.ts`, `*.tsx` → TypeScript
- `*.py`, `requirements.txt`, `pyproject.toml` → Python
- `go.mod` → Go
- `Cargo.toml` → Rust
- `*.java`, `pom.xml`, `build.gradle` → Java
- `*.kt`, `build.gradle.kts` → Kotlin
- `*.cpp`, `CMakeLists.txt` → C++
- `*.swift`, `Package.swift` → Swift
- `pubspec.yaml` → Flutter/Dart
- `Dockerfile`, `docker-compose.yml` → Docker

## ステップ2: 共通コンポーネント

以下を自動配置（確認のみ）:

| 種別 | 名前 | 用途 |
|------|------|------|
| agent | `tdd-guide.md` | TDD 実践ガイド |
| agent | `build-error-resolver.md` | ビルドエラー解決 |
| agent | `docs-lookup.md` | ドキュメント検索 |
| skill | `coding-standards/` | コーディング規約 |
| skill | `tdd-workflow/` | テスト駆動開発 |
| skill | `verification-loop/` | 検証ループ |
| context | `dev.md` | 実装モード |

## ステップ3: AskUserQuestion で追加選択

検出した技術スタックに基づいて選択肢を動的に構成する。

**「追加で導入するコンポーネントを選んでください」** (multiSelect: true)

常に表示:
| 選択肢 | 含まれるもの |
|--------|-------------|
| バックエンドパターン | skill: `backend-patterns/` |
| フロントエンドパターン | skill: `frontend-patterns/` |
| Docker パターン | skill: `docker-patterns/` |

技術スタック検出時のみ表示:

| 条件 | 選択肢 | 含まれるもの |
|------|--------|-------------|
| PHP | Laravel パターン+TDD+セキュリティ | skills: `laravel-patterns/`, `laravel-tdd/`, `laravel-security/`, `laravel-verification/` / rules: `php/*` |
| TypeScript | TS レビュアー+ルール | agent: `typescript-reviewer.md` / rules: `typescript/*` |
| Python | Python パターン+テスト | agent: `python-reviewer.md` / skills: `python-patterns/`, `python-testing/` / rules: `python/*` |
| Python+Django | Django パターン+TDD+セキュリティ | skills: `django-patterns/`, `django-tdd/`, `django-security/`, `django-verification/` |
| Go | Go パターン+テスト | agents: `go-reviewer.md`, `go-build-resolver.md` / skills: `golang-patterns/`, `golang-testing/` / rules: `golang/*` |
| Rust | Rust パターン+テスト | agents: `rust-reviewer.md`, `rust-build-resolver.md` / skills: `rust-patterns/`, `rust-testing/` / rules: `rust/*` |
| Java | Spring Boot パターン+TDD | agents: `java-reviewer.md`, `java-build-resolver.md` / skills: `springboot-patterns/`, `springboot-tdd/`, `springboot-security/`, `springboot-verification/` / rules: `java/*` |
| Kotlin | Kotlin パターン+テスト | agents: `kotlin-reviewer.md`, `kotlin-build-resolver.md` / skills: `kotlin-patterns/`, `kotlin-testing/`, `kotlin-coroutines-flows/`, `kotlin-ktor-patterns/` / rules: `kotlin/*` |
| C++ | C++ コーディング規約+テスト | agents: `cpp-reviewer.md`, `cpp-build-resolver.md` / skills: `cpp-coding-standards/`, `cpp-testing/` / rules: `cpp/*` |
| Swift | Swift パターン | skills: `swift-concurrency-6-2/`, `swiftui-patterns/`, `swift-protocol-di-testing/` / rules: `swift/*` |
| Flutter | Flutter レビュー | agent: `flutter-reviewer.md` / skill: `flutter-dart-code-review/` |

## ステップ4: ルールの配置

ルールは `.claude/rules/` にフラットコピー（ディレクトリ構造なし）:
```bash
mkdir -p .claude/rules
cp "S:/ECC/everything-claude-code/rules/common/"*.md .claude/rules/
cp "S:/ECC/everything-claude-code/rules/<lang>/"*.md .claude/rules/  # 選択した言語
```

コピー先のファイル名に `ecc-` プレフィックスは付けない（Claude Code のルール規約に従う）。
ただし、既存のプロジェクトルールとの競合を確認し、競合があれば報告。

## ステップ5: 配置実行

スキル → ジャンクション / エージェント・コンテキスト → コピー / ルール → コピー
既にプロジェクトに存在するものはスキップ。

## 報告

導入コンポーネントの一覧と、推奨フックレベル `standard` を表示。
