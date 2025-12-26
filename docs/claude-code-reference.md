# Claude Code リファレンス

このドキュメントでは、Claude Code の機能と設定方法を説明します。

効果的なプロンプティングの方法は [ベストプラクティス](../docs/best-practices.md) を参照してください。

## Claude Code とは

Claude Code は Anthropic が提供する CLI ツールで、AI を活用したコード生成、編集、デバッグを対話的に行うことができます。

## コマンド一覧

### 起動

```bash
claude
```

### 主要なコマンド

| コマンド | 説明 |
|----------|------|
| `/help` | ヘルプを表示 |
| `/clear` | 会話履歴をクリア |
| `/config` | 設定を確認・変更 |
| `/compact` | 会話を要約してコンテキストを圧縮 |
| `/context` | 現在のコンテキスト使用状況を視覚的に表示 |
| `/cost` | トークン使用量を表示（API ユーザー向け） |
| `/usage` | プラン使用量を表示（Max/Pro サブスクライバー向け） |
| `/doctor` | Claude Code の設定を診断 |
| `/init` | プロジェクトに CLAUDE.md を作成 |
| `/review` | コードレビューを依頼 |
| `/{command}` | カスタムコマンドを実行 |

## CLAUDE.md

プロジェクトルートに `CLAUDE.md` を配置すると、Claude Code が起動時に自動的に読み込みます。
（Cursorで言うところの/.cursor/rules配下のmdファイルに相当）

### 活用シーン

- プロジェクト固有のルールを伝える
- 使用する技術スタックを明示する
- 禁止事項や注意点を記載する
- 毎回同じ説明を繰り返したくないとき
- チームで共通の前提を共有したいとき

### 記載する内容

| セクション | 内容 |
|------------|------|
| プロジェクト概要 | 何のためのプロジェクトか |
| 技術スタック | 使用する言語、フレームワーク、ライブラリ |
| コーディング規約 | 命名規則、フォーマット、スタイル |
| 禁止事項 | 使用禁止のライブラリ、パターン |
| ディレクトリ構成 | 主要なディレクトリの役割 |

### 記載例

```markdown
# プロジェクト概要

〇〇のための△△アプリケーション

# 技術スタック

- 言語: TypeScript
- フレームワーク: React
- 状態管理: Zustand
- スタイリング: Tailwind CSS

# コーディング規約

- 変数名は camelCase
- コンポーネント名は PascalCase
- 定数は UPPER_SNAKE_CASE
- 1行は80文字以内

# 禁止事項

- console.log はデバッグ用のみ、本番コードには残さない
- any 型の使用禁止
- インラインスタイルの使用禁止

# ディレクトリ構成

- src/components/ - React コンポーネント
- src/hooks/ - カスタムフック
- src/utils/ - ユーティリティ関数
- src/types/ - 型定義
```

## 拡張機能の使い分け

Claude Code には、タスクの規模や用途に応じた3つの拡張機能があります。

| 機能 | 配置場所 | 用途 |
|------|----------|------|
| カスタムコマンド | `.claude/commands/*.md` | 軽量で頻繁に使うタスク（60行程度） |
| Skills | `.claude/skills/*/SKILL.md` | 中規模で繰り返し使う定型作業 |
| Subagent | `.claude/agents/*.md` | 独立したコンテキストで実行したいタスク |

### 使い分けの目安

- **カスタムコマンド**: テスト実行、ビルド、リントなど日常的な操作。`/command` で明示的に呼び出す
- **Skills**: Figma MCP でデザインから UI コンポーネントを生成するなど、複数ステップの定型作業。説明文に基づき自動で呼び出される
- **Subagent**: コードレビューなど、現在の作業コンテキストから離れて客観的に判断したい場合。新しいコンテキストで実行される

## カスタムコマンド

`.claude/commands/` にマークダウンファイルを配置すると、`/` で呼び出せるカスタムコマンドになります。

### コマンドの作成

```bash
mkdir -p .claude/commands
```

### コマンドの例

#### テストコマンド

```markdown
# .claude/commands/test.md
テストを実行し、失敗したテストがあれば修正してください。
```

#### ビルドコマンド

```markdown
# .claude/commands/build.md
プロジェクトをビルドしてください。エラーがあれば修正してください。
```

#### リントコマンド

```markdown
# .claude/commands/lint.md
コードの静的解析を実行し、警告やエラーを修正してください。
```

#### コードレビューコマンド

```markdown
# .claude/commands/review.md
直近の変更をレビューしてください。以下の観点で確認してください：
- セキュリティ上の問題
- パフォーマンスの問題
- コーディング規約への準拠
```

### コマンドの使用

```bash
claude
> /test
> /build
> /lint
```

### チームでの共有

カスタムコマンドは `.claude/commands/` に配置して Git で管理すると、チーム全体で共有できます。

## Skills

Skills は中規模で繰り返し使う定型作業に適しています。MCP 連携など複雑な処理にも対応できます。

### ディレクトリ構成

```
.claude/skills/skill-name/
├── SKILL.md          # 必須: スキル定義
├── reference.md      # 任意: 参考ドキュメント
└── templates/        # 任意: テンプレートファイル
```

### SKILL.md の書き方

```markdown
---
name: skill-name
description: スキルの説明。どのような場面で使用するかを記載。
allowed-tools: Read, Write, Bash
---

# スキル名

## 手順

1. ステップ1
2. ステップ2
3. ステップ3
```

### 呼び出し方

Skills は description に基づいて自動的に呼び出されます。明示的なコマンドは不要です。

## Subagent

Subagent は新しいコンテキストで実行されるため、現在の作業から独立した視点で判断したい場合に使用します。

### コンテキストの仕組み

| 場面 | コンテキスト |
|------|-------------|
| Subagent実行時 | サブエージェント専用の新しいコンテキスト |
| メイン会話 | メインの会話コンテキスト |

**Subagentの特徴:**
- 独立したコンテキストで実行される
- メインの会話履歴は引き継がない
- 実行結果だけがメインに返される

これにより「客観的な視点」でのレビューが可能になります。Subagentは会話の流れ（自分が書いたコードなど）を知らない状態で実行されるため、バイアスがかかりにくくなります。

### ファイル配置

```
.claude/agents/agent-name.md
```

### 定義ファイルの書き方

```markdown
---
name: code-reviewer
description: コードレビューを行う。コード品質、セキュリティ、保守性を評価。
tools: Read, Grep, Glob, Bash
model: inherit
---

あなたはシニアコードレビュアーです。

## レビュー観点

- コードの可読性と明確さ
- 適切な命名
- 重複コードの有無
- エラーハンドリング
- セキュリティ上の問題
- テストカバレッジ

## 出力形式

- Critical（必須修正）
- Warning（推奨修正）
- Suggestion（検討事項）
```

### 設定オプション（フロントマター）

YAMLフロントマターはClaude Codeの公式推奨フォーマットです。

| フィールド | 必須 | 説明 |
|------------|------|------|
| `name` | ✓ | エージェントの一意な識別子（小文字とハイフン） |
| `description` | ✓ | エージェントの目的（Claude Codeが自動委任判断に使用） |
| `tools` | × | 使用可能なツール（省略時は全ツール継承） |
| `model` | × | 使用モデル |
| `permissionMode` | × | 権限モード |
| `skills` | × | 自動ロードするスキル名（カンマ区切り） |

### toolsに指定できる値

`tools`フィールドにはシェルコマンドではなく、**Claude Code内部のツール名**を指定します。

| ツール | 説明 |
|--------|------|
| `Read` | ファイル読み込み |
| `Edit` | ファイル編集 |
| `Write` | ファイル作成 |
| `Grep` | コンテンツ検索 |
| `Glob` | ファイルパターンマッチング |
| `Bash` | Bashコマンド実行 |
| `WebFetch` | Web コンテンツ取得 |
| `WebSearch` | Web 検索 |
| `Task` | サブエージェント起動 |

MCPサーバーが接続されている場合、そのツールも指定可能です。`/agents`コマンドで対話的に管理すると、利用可能なすべてのツールが表示されます。

### modelに指定できる値

| 値 | 説明 |
|-----|------|
| `inherit` | 親スレッド（メイン会話）と同じモデルを使用 |
| `sonnet` | Claude Sonnet |
| `opus` | Claude Opus（最高性能） |
| `haiku` | Claude Haiku（高速・軽量） |
| 省略時 | デフォルトモデル（通常sonnet） |

`inherit`は親のコンテキストからモデル設定を継承するため、一貫性を保つベストプラクティスです。

### permissionModeに指定できる値

サブエージェントの権限レベルを制御します。指定できる値（`default`, `acceptEdits`, `bypassPermissions`, `plan`）の詳細は[権限管理](#権限管理)セクションを参照してください。

**グローバル設定との違い:**
- `permissionMode`: サブエージェント単位の設定（`.claude/agents/*.md`で指定）
- `defaultMode`: 全体設定（`settings.json`で指定）

**CLIオプション `--dangerously-skip-permissions`:**
CLI起動時に使用するフラグで、すべてのパーミッション確認をスキップします。サブエージェントの`permissionMode`とは異なり、Claude Code全体に適用されます。CI/CD環境など自動化用途向けです。

### 管理コマンド

```bash
/agents  # Subagent の一覧表示・作成・編集・削除
```

## 権限管理

Claude Codeでは、セキュリティ上の理由から**会話中に権限を変更することはできません**。権限は事前に設定ファイルで定義する必要があります。

### 権限管理の方法

| 方法 | 可否 | 詳細 |
|------|------|------|
| 会話中に「確認なしで」と指示 | ✗ | 不可 |
| `/trust`コマンド | ✗ | 存在しない |
| `settings.json`で事前設定 | ✓ | 公式推奨 |
| `/add-dir`でディレクトリ追加 | ✓ | セッション中に可能 |
| `/permissions`で権限確認 | ✓ | 確認のみ（変更不可） |

### settings.jsonでの許可設定

`settings.json`の`permissions.allow`で、確認なしで実行を許可するコマンドを指定できます。

```json
{
  "permissions": {
    "allow": [
      "Bash(gh pr create:*)",
      "Bash(git:*)",
      "Bash(npm run build)"
    ]
  }
}
```

### 許可パターンの書き方

**Bashコマンド（プレフィックスマッチ）:**
```json
"Bash(npm run build)"          // 完全一致
"Bash(npm run test:*)"         // プレフィックスマッチ
"Bash(git:*)"                  // git全体を許可
```

**ファイル操作（gitignore形式）:**
```json
"Read(~/Documents/*.pdf)"      // ホームディレクトリから相対
"Edit(/src/**/*.ts)"           // 設定ファイルから相対
"Read(//Users/alice/secrets/**)" // 絶対パス（//プレフィックス）
```

**WebFetch:**
```json
"WebFetch(domain:github.com)"
```

**MCPツール:**
```json
"mcp__puppeteer"               // サーバー全体
"mcp__puppeteer__*"            // ワイルドカード
"mcp__puppeteer__puppeteer_navigate" // 特定ツール
```

### 権限モード

`settings.json`でセッション全体のデフォルト権限モードを設定できます。Subagentの`permissionMode`でも同じ値を使用します。

```json
{
  "permissions": {
    "defaultMode": "acceptEdits"
  }
}
```

| モード | 説明 |
|--------|------|
| `default` | 初回使用時にプロンプト表示（デフォルト） |
| `acceptEdits` | ファイル編集を自動承認 |
| `plan` | 読み取り専用モード。ファイル分析は可能だが修正やコマンド実行は不可 |
| `bypassPermissions` | すべての権限プロンプトをスキップ（安全な環境が必須） |

## 関連ドキュメント

- [セットアップガイド](../docs/getting-started.md) - インストール、認証、初回起動
- [ベストプラクティス](../docs/best-practices.md) - プロンプティング技術、セキュリティ
- [Claude Code 公式ドキュメント](https://docs.anthropic.com/en/docs/claude-code)
