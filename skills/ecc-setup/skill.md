# /ecc-setup - ECC リポジトリ管理

ECC リポジトリ (`S:\ECC\everything-claude-code`) のクローン・更新を行う。

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

### 3. 報告

```
ECC v{tag} ({hash}) - 更新済み
コンポーネントの導入は /tools で行ってください
```
