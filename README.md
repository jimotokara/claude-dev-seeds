# claude-dev-seeds

Claude Code の開発環境を「育てる」ための3つのスキル。

[everything-claude-code (ECC)](https://github.com/affaan-m/everything-claude-code) のスキル・エージェント（116+28個）を、プロジェクトのライフサイクルに合わせて段階的に導入する仕組みです。

## 3つのスキルの役割

```
/ecc-setup        初回1度だけ。ECC クローン + グローバルリンク
/init-memory      プロジェクトごと。CLAUDE.md 等を生成 + ECC 自動利用ルール組み込み
/ecc-phase        随時。フェーズに応じたスキル・エージェントの追加導入
```

## インストール

### 1. このリポジトリのスキルを配置

```bash
# クローン
git clone https://github.com/jimotokara/claude-dev-seeds.git

# グローバルスキルとしてシンボリックリンク（Windows）
# 管理者権限 or 開発者モードが必要
cd %USERPROFILE%\.claude\skills
mklink /D ecc-setup   "<clone先>\skills\ecc-setup"
mklink /D ecc-phase   "<clone先>\skills\ecc-phase"
mklink /D init-memory "<clone先>\skills\init-memory"
```

```bash
# macOS / Linux
ln -s <clone先>/skills/ecc-setup   ~/.claude/skills/ecc-setup
ln -s <clone先>/skills/ecc-phase   ~/.claude/skills/ecc-phase
ln -s <clone先>/skills/init-memory ~/.claude/skills/init-memory
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
/ecc-phase design      # 設計段階: planner, architect
/ecc-phase spec        # 仕様確定: database-reviewer, backend-patterns
/ecc-phase implement   # 実装: 言語別パターン, TDD
/ecc-phase test        # テスト: e2e-runner
/ecc-phase review      # レビュー: security-reviewer, code-reviewer
/ecc-phase deploy      # デプロイ: deployment-patterns
```

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
- `skills/ecc-phase/skill.md` 内の ECC パス参照
- `skills/init-memory/skill.md` 内の ECC 存在チェックパス

### バージョンチェックの閾値

デフォルトでは30日で警告、7日で情報表示です。
`skills/ecc-phase/skill.md` のステップ0で変更できます。

## 関連記事

- [Claude Codeのセッション記憶喪失問題を解決し続けたら、開発が劇的に変わった話](https://qiita.com/ProgrammingForEver/items/ee68fc621f7d1c53ffe1) by @ProgrammingForEver
- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) - ECC 本体リポジトリ

## ライセンス

MIT
