# /ecc-review - ECC レビュー・品質フェーズ

コード品質・セキュリティの確認に必要な ECC コンポーネントをプロジェクトの `.claude/` に配置する。
ECC モジュール: `security` 相当 / コンテキスト: `review.md` / フックレベル推奨: `strict`

## 前提

`S:\ECC\everything-claude-code` が存在すること。なければ `/ecc-setup` を案内。
鮮度チェック（30日超 → 警告、7日超 → 情報表示）。

## 自動配置

| 種別 | 名前 | 用途 |
|------|------|------|
| agent | `code-reviewer.md` | コード品質レビュー |
| agent | `security-reviewer.md` | セキュリティ脆弱性レビュー |
| skill | `security-review/` | セキュリティチェックリスト |
| context | `review.md` | レビューモード（品質・セキュリティ重視） |

## AskUserQuestion で選択 (multiSelect: true)

**「追加で導入するレビュー関連コンポーネントを選んでください」**

| 選択肢 | 含まれるもの |
|--------|-------------|
| セキュリティスキャン | skill: `security-scan/` |
| リファクタ・デッドコード除去 | agent: `refactor-cleaner.md` |
| ドキュメント同期 | agent: `doc-updater.md` |
| コーディング規約 | skill: `coding-standards/` |
| ドキュメント検索 | agent: `docs-lookup.md` |

技術スタックに応じて追加表示:
| 条件 | 選択肢 | 含まれるもの |
|------|--------|-------------|
| PHP/Laravel | Laravel セキュリティ | skill: `laravel-security/` |
| Django | Django セキュリティ | skill: `django-security/` |
| Spring Boot | Spring Boot セキュリティ | skill: `springboot-security/` |
| Perl | Perl セキュリティ | skill: `perl-security/` |

## 配置・報告

スキル → ジャンクション / エージェント・コンテキスト → コピー。既存はスキップ。
推奨フックレベル `strict` を案内。
