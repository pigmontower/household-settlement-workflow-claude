# セットアップガイド

このガイドでは、Claude Code Starter Template を使い始めるための環境構築手順を説明します。

## 前提条件

| 項目 | 要件 |
|------|------|
| Git | 最新版 |
| Anthropic アカウント | Claude Code の認証に必要 |

## 1. Claude Code のインストール

### ネイティブインストール（自己完結型の実行ファイル）※推奨

```bash
# macOS / Linux
curl -fsSL https://claude.ai/install.sh | sh

# Windows (PowerShell)
irm https://claude.ai/install.ps1 | iex
```

### Homebrew でインストール（macOS）

```bash
brew install claude-code
```

### npm でインストール

Node.js 18 以上が必要です。

```bash
npm install -g @anthropic-ai/claude-code
```

### インストール確認

```bash
claude --version
```

## 2. Claude Code の認証

```bash
claude auth login
```

ブラウザが開き、認証が完了すると Claude Code が使用可能になります。

## 3. テンプレートの取得

```bash
git clone https://github.com/your-org/claude-code-template.git my-project
cd my-project
```

### 既存プロジェクトに統合する場合

1. `docs/` ディレクトリをコピー
2. `.claude/` ディレクトリを作成
3. `CLAUDE.md` を作成

## 4. セキュリティ設定（推奨）

### Dependabot alerts の有効化

依存ライブラリの脆弱性を自動監視するため、Dependabot alerts の有効化を推奨します。

**手順:**

1. GitHub リポジトリページを開く
2. **Settings** タブをクリック
3. 左サイドバーの **Code security** をクリック
4. **Dependabot alerts** の **Enable** をクリック

または、GitHub CLI で有効化:

```bash
gh api -X PUT repos/<owner>/<repo>/vulnerability-alerts
```

> **注意:** この設定はテンプレートから引き継がれません。新しいリポジトリを作成するたびに手動で有効化してください。

## 5. 作業ディレクトリで起動

```bash
cd working-directory
claude
```

## 6. 初めての対話

Claude Code を起動したら、まず `CLAUDE.md` の作成を依頼することを推奨します。

```
> このプロジェクト用の CLAUDE.md を作成してください
```

その後、作りたいものを伝えます：

```
> iOS アプリを作りたい
```

```
> Web サービスのバックエンドを作りたい
```

`CLAUDE.md` やカスタムコマンドの詳細は [Claude Code リファレンス](claude-code-reference.md) を参照してください。

## トラブルシューティング

### 認証エラーが発生する場合

```bash
claude auth logout
claude auth login
```

### Claude Code が起動しない場合

再インストールを試してください。

```bash
# ネイティブインストールの場合
curl -fsSL https://claude.ai/install.sh | sh

# npm でインストールした場合
npm uninstall -g @anthropic-ai/claude-code
npm install -g @anthropic-ai/claude-code
```

### プロジェクトのコンテキストが認識されない場合

- `CLAUDE.md` が正しい場所にあるか確認
- Claude Code を再起動

## 次のステップ

- [ベストプラクティス](best-practices.md) - プロンプティング技術、セキュリティ、チーム活用
- [Claude Code リファレンス](claude-code-reference.md) - コマンド一覧、CLAUDE.md、カスタムコマンド
