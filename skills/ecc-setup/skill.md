# /ecc-setup - ECC リポジトリ管理

ECC リポジトリ (`S:\ECC\everything-claude-code`) のクローン・更新と、フェーズ管理コマンドのグローバル配置を行う。

## 実行手順

### 1. リポジトリのクローンまたは更新

```bash
if [ -d "S:/ECC/everything-claude-code" ]; then
  cd "S:/ECC/everything-claude-code" && git pull
else
  mkdir -p "S:/ECC" && git clone https://github.com/affaan-m/everything-claude-code "S:/ECC/everything-claude-code"
fi
```

### 2. バージョン情報の取得と報告

```bash
cd "S:/ECC/everything-claude-code"
git rev-parse --short HEAD
git log -1 --format="%ci"
git describe --tags --abbrev=0 2>/dev/null || echo "(no tag)"
```

`S:\ECC\ecc-state.json` を更新: `{"lastPull":"<ISO>","version":"<tag>","hash":"<hash>"}`

更新があれば `git log --oneline -10` で変更サマリーを表示。

### 3. フェーズ管理コマンドの確認

`~/.claude/skills/` に以下の 12 コマンドが存在することを確認。不足があれば作成を案内。

メタ: `ecc-setup`, `ecc-status`, `ecc-clean`
フェーズ: `ecc-plan`, `ecc-code`, `ecc-test`, `ecc-review`, `ecc-docs`, `ecc-research`, `ecc-learn`, `ecc-content`, `ecc-ops`

### 4. グローバルの不要コンポーネントを整理

`~/.claude/agents/ecc-*` や `~/.claude/skills/ecc-*`（上記 12 コマンド以外）が残っている場合、ユーザーに確認の上削除する。

### 5. 報告

```
ECC v{tag} ({hash}) - 更新済み
グローバル: フェーズ管理コマンド 12 個のみ
実コンポーネントは各プロジェクトで /ecc-code 等を実行して配置
```

## アーキテクチャ

```
S:\ECC\everything-claude-code/          ← 倉庫（全コンポーネント）
  manifests/install-modules.json           モジュール定義
  manifests/install-profiles.json          プロファイル定義
  contexts/*.md                            コンテキストモード
  agents/, skills/, commands/, rules/      実コンポーネント

~/.claude/skills/ecc-*/                 ← フェーズ管理コマンドのみ

プロジェクト/.claude/                   ← 各フェーズコマンドが配置・削除
  agents/ecc-*.md                          (コピー)
  skills/ecc-*/                            (ジャンクション)
  rules/ecc-*/                             (コピー)
  contexts/ecc-*.md                        (コピー)
```

## 配置方式（Windows）

- スキル（ディレクトリ）: `powershell -Command "cmd /c 'mklink /J \"<link>\" \"<target>\"'"`
- エージェント/ルール/コンテキスト（ファイル）: `cp` でコピー
