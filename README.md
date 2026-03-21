# claude-dev-seeds

Claude Code の開発環境を「育てる」ためのスキル集。

[everything-claude-code (ECC)](https://github.com/affaan-m/everything-claude-code) のスキル・エージェント（116+28個）を、プロジェクトのライフサイクルに合わせて段階的に導入する仕組みです。

## スキルの役割

```
/ecc-setup        初回1度だけ。ECC クローン + グローバルリンク
/init-memory      プロジェクトごと。CLAUDE.md 等を生成 + ECC 自動利用ルール組み込み
/ecc-design       設計フェーズのコンポーネント導入
/ecc-spec         仕様確定フェーズのコンポーネント導入
/ecc-implement    実装フェーズのコンポーネント導入
/ecc-test         テストフェーズのコンポーネント導入
/ecc-review       レビュー・リリース準備フェーズのコンポーネント導入
/ecc-deploy       デプロイフェーズのコンポーネント導入
/ecc-clean        不要フェーズのコンポーネントを削除
```

## インストール

### 1. このリポジトリのスキルを配置

```bash
# クローン
git clone https://github.com/jimotokara/claude-dev-seeds.git

# グローバルスキルとしてシンボリックリンク（Windows）
# 管理者権限 or 開発者モードが必要
cd %USERPROFILE%\.claude\skills
mklink /D ecc-setup     "<clone先>\skills\ecc-setup"
mklink /D ecc-design    "<clone先>\skills\ecc-design"
mklink /D ecc-spec      "<clone先>\skills\ecc-spec"
mklink /D ecc-implement "<clone先>\skills\ecc-implement"
mklink /D ecc-test      "<clone先>\skills\ecc-test"
mklink /D ecc-review    "<clone先>\skills\ecc-review"
mklink /D ecc-deploy    "<clone先>\skills\ecc-deploy"
mklink /D ecc-clean     "<clone先>\skills\ecc-clean"
mklink /D init-memory   "<clone先>\skills\init-memory"
```

```bash
# macOS / Linux
for skill in ecc-setup ecc-design ecc-spec ecc-implement ecc-test ecc-review ecc-deploy ecc-clean init-memory; do
  ln -s <clone先>/skills/$skill ~/.claude/skills/$skill
done
```

### 2. ECC リポジトリのクローン先を決める

デフォルトでは `~/.ecc/everything-claude-code` を想定しています。
変更する場合は、各スキルファイル内のパスを書き換えてください。

### 3. セットアップ実行

Claude Code で以下を実行：

```
/ecc-setup
```

## 使い方

### 新プロジェクト開始時

```
/init-memory
```

対話的にプロジェクト情報を収集し、以下を自動生成します：
- AGENTS.md（プロジェクト規約）
- CLAUDE.md（Claude Code 固有設定 + ECC 自動利用ルール）
- .agent/（HANDOFF.md, MEMORY.md）
- .spec/（SPEC.md, TODO.md）
- .claude/agents/（サブエージェント定義）

### フェーズが進んだとき

```
/ecc-design       # 設計段階: planner, architect
/ecc-spec         # 仕様確定: database-reviewer, backend-patterns
/ecc-implement    # 実装: 言語別パターン, TDD
/ecc-test         # テスト: e2e-runner
/ecc-review       # レビュー: security-reviewer, code-reviewer
/ecc-deploy       # デプロイ: deployment-patterns
```

### フェーズが変わったとき

```bash
/ecc-clean
```

現在のフェーズを選択すると、そのフェーズのコンポーネントだけ残して他を削除します。
コンテキストが軽量に保たれ、AI の応答品質が維持されます。

## ECC 自動利用ルール

`/init-memory` が CLAUDE.md に以下のようなルールを自動生成します。
Claude Code はフェーズに関係なく、状況に応じて適切な ECC エージェントを自律的に起動します。

| 状況 | 使うエージェント |
|---|---|
| 新機能の設計・方針転換 | ecc-planner |
| アーキテクチャの判断 | ecc-architect |
| DB スキーマの変更 | ecc-database-reviewer |
| ビルドエラーが3回解消しない | ecc-build-error-resolver |
| マージ前のコードレビュー | ecc-code-reviewer |
| セキュリティに関わる変更 | ecc-security-reviewer |

## カスタマイズ

### ECC クローン先の変更

各スキルファイル内のパスを変更してください：
- `skills/ecc-setup/skill.md` 内の `$eccPath`
- 各フェーズスキル内の ECC パス参照
- `skills/init-memory/skill.md` 内の ECC 存在チェックパス

### バージョンチェックの閾値

デフォルトでは30日で警告、7日で情報表示です。
各フェーズスキルのステップ0で変更できます。

## 関連記事

- [Claude Codeのセッション記憶喪失問題を解決し続けたら、開発が劇的に変わった話](https://qiita.com/ProgrammingForEver/items/ee68fc621f7d1c53ffe1) by @ProgrammingForEver
- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) - ECC 本体リポジトリ

## ライセンス

MIT
