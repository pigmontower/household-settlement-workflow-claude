# Claude Code Starter Template

Claude Code を活用して多様なアプリケーションを立ち上げるためのマルチスタック対応スターターテンプレートです。

## 概要

このテンプレートは、モバイル（iOS/Android）、Web フロントエンド、バックエンド、スクリプトなど、あらゆる技術スタックのプロジェクトを Claude Code と共に素早く立ち上げるための抽象的なベースを提供します。

| 特徴 | 説明 |
|------|------|
| マルチスタック対応 | Web、モバイル、バックエンド、スクリプトなど自由に構成可能 |
| Claude Code 最適化 | AI 駆動開発に適したディレクトリ構造とドキュメント |
| 拡張性 | 必要なスタックだけを選択して利用可能 |

## ディレクトリ構成

```
your-project/
├── README.md                     # プロジェクト概要（このファイル）
├── docs/                         # ドキュメント
│   ├── getting-started.md       # セットアップガイド
│   ├── best-practices.md        # ベストプラクティス
│   └── claude-code-reference.md # Claude Code 機能リファレンス
├── working-directory/            # 作業ディレクトリ
├── .claude/                      # Claude Code 設定
│   └── commands/                # カスタムコマンド
└── CLAUDE.md                     # Claude Code へのプロジェクト指示
```

> **Note**: `working-directory/` 内の構成は自由です。プロジェクトに応じて最適な構造を作成してください。

## クイックスタート

```bash
# 1. テンプレートを取得
git clone https://github.com/your-org/claude-code-template.git my-project
cd my-project

# 2. Claude Code をインストール・認証
curl -fsSL https://claude.ai/install.sh | sh  # ※npm の場合: npm install -g @anthropic-ai/claude-code
claude auth login

# 3. 作業ディレクトリで Claude Code を起動
cd working-directory
claude
```

詳細は [セットアップガイド](docs/getting-started.md) を参照してください。

## ドキュメント

| ドキュメント | 内容 |
|-------------|------|
| [セットアップガイド](docs/getting-started.md) | インストール、認証、初回起動、トラブルシューティング |
| [ベストプラクティス](docs/best-practices.md) | プロンプティング技術、セキュリティ、チーム活用 |
| [Claude Code リファレンス](docs/claude-code-reference.md) | コマンド、CLAUDE.md、カスタムコマンドの使い方 |

## ライセンス

MIT License

## 関連リンク

- [Claude Code 公式ドキュメント](https://docs.anthropic.com/en/docs/claude-code)
- [Anthropic Developer Portal](https://console.anthropic.com/)
