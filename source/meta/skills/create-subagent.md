# Create Subagent Skill

Cursor から Claude Code 形式のサブエージェントを作成するスキル。

## Cursor と Claude Code の違い

| 項目 | Cursor | Claude Code |
|------|--------|-------------|
| サブエージェント指定 | Task ツールの `subagent_type` パラメータ | `.claude/agents/` にカスタム定義 |
| 組み込みタイプ | explore, generalPurpose, deep-research 等 | Explore, Plan, general-purpose |
| カスタム定義 | 組み込みタイプの選択のみ | 完全カスタム可能（frontmatter + プロンプト） |
| 自動ルーティング | なし（明示的に指定） | description に基づき自動判断 |

### Cursor の subagent_type 一覧

`subagent_type` は Cursor の Task ツール固有のパラメータ。Claude Code には存在しない。

| type | 用途 | 安定性 |
|------|------|--------|
| `generalPurpose` | 汎用、複雑なマルチステップタスク | 安定 |
| `deep-research` | 深いリサーチ、複数ソース収集 | 安定 |
| `explore` | コードベース探索（高速） | 不安定な場合あり |
| `explorer` | コードベース探索（読み取り専用） | 不安定な場合あり |
| `code-reviewer` | コードレビュー | 安定 |
| `debugger` | デバッグ | 安定 |
| `test-runner` | テスト実行 | 安定 |

### Claude Code の組み込みサブエージェント

| 名前 | モデル | ツール | 用途 |
|------|--------|--------|------|
| Explore | Haiku（高速） | 読み取り専用 | コードベース探索 |
| Plan | 継承 | 読み取り専用 | プランモード用リサーチ |
| general-purpose | 継承 | 全ツール | 複雑なマルチステップタスク |

### タスク種別ごとの推奨

| タスク | Cursor | Claude Code |
|--------|--------|-------------|
| リサーチ | `subagent_type: deep-research` | カスタム `deep-research` エージェント |
| コード探索 | `subagent_type: explore`（不安定な場合あり） | 組み込み `Explore`（自動選択） |
| 汎用タスク | `subagent_type: generalPurpose` | 組み込み `general-purpose`（自動選択） |

## 発火条件

- 「サブエージェントを作成」「エージェントを作って」
- 「新しいエージェントが必要」「専用エージェントを追加」

## プロセス

### Step 1: 要件ヒアリング

以下を確認:
1. エージェントの目的（何をするか）
2. 使用するツール（Read, Write, Edit, Bash, Grep, Glob など）
3. 制限事項（読み取り専用か、特定ツールのみか）

### Step 2: Claude Code 形式でテンプレート生成

```markdown
---
name: agent-name
description: When Claude should delegate to this subagent. Use proactively when...
tools: Read, Grep, Glob  # 許可するツール
model: inherit  # sonnet, opus, haiku, inherit
---
システムプロンプトをここに記述。

When invoked:
1. 最初のステップ
2. 次のステップ
3. ...

## チェックリスト
- 確認項目1
- 確認項目2

## 出力形式
期待される出力フォーマットを記述。
```

### Step 3: 配置

- source/ ベース: `source/meta/agents/{agent-name}.md`
- 直接配置: `.cursor/agents/{agent-name}.md`

source/ に配置した場合は `./scripts/generate.sh` を実行。

### Step 4: 動作検証（Cursor の場合）

作成後、必ず動作検証を行う:

```
Cursor の Task ツールで以下を実行:
- subagent_type: generalPurpose を使用（カスタムエージェントのテスト用）
- 最小のタスクでテスト
- 期待される出力形式が得られるか確認

注意: subagent_type は Cursor 固有。リサーチタスクには deep-research を使用。
```

### Step 4: 動作検証（Claude Code の場合）

Claude Code では `.claude/agents/` に配置後、自動的にルーティングされる:

```
- description に基づき Claude が自動で委譲
- 明示的に「[agent-name] エージェントを使って」と指示も可能
- /agents コマンドで管理
```

## ベストプラクティス（Claude Code 公式）

1. 1つのタスクに特化（focused）
2. 詳細な description を記述（delegation の判断に使われる）
3. 必要最小限のツールのみ許可
4. バージョン管理にチェックイン

## ツール一覧

| ツール | 用途 |
|---|---|
| Read | ファイル読み取り |
| Write | ファイル書き込み |
| Edit | ファイル編集 |
| Bash | コマンド実行 |
| Grep | パターン検索 |
| Glob | ファイル検索 |
| SemanticSearch | 意味検索 |
| WebSearch | Web 検索 |
| WebFetch | URL 取得 |

## 公式テンプレート例

### Code Reviewer（読み取り専用）

```markdown
---
name: code-reviewer
description: Expert code review specialist. Use proactively after writing or modifying code.
tools: Read, Grep, Glob, Bash
model: inherit
---
You are a senior code reviewer ensuring high standards.

When invoked:
1. Run git diff to see recent changes
2. Focus on modified files
3. Begin review immediately

Review checklist:
- Code is clear and readable
- No exposed secrets or API keys
- Proper error handling
- Good test coverage
```

### Debugger（修正可能）

```markdown
---
name: debugger
description: Debugging specialist for errors and test failures. Use proactively when encountering issues.
tools: Read, Edit, Bash, Grep, Glob
---
You are an expert debugger specializing in root cause analysis.

When invoked:
1. Capture error message and stack trace
2. Identify reproduction steps
3. Isolate the failure location
4. Implement minimal fix
5. Verify solution works
```

## 注意事項

### Cursor 固有

- `subagent_type` は Cursor の Task ツール固有のパラメータ
- `explore` / `explorer` は高速だが不安定な場合がある
- カスタムエージェントのテストには `generalPurpose` を使用
- リサーチタスクには `deep-research` を使用

### Claude Code 固有

- `.claude/agents/` にカスタムエージェントを定義
- description に基づき Claude が自動でルーティング
- `/agents` コマンドで管理・作成が可能

### 共通

- 作成後は必ず動作検証を行う
- 1つのタスクに特化させる

## 参考

- Claude Code 公式: https://docs.anthropic.com/en/docs/claude-code/sub-agents
- 調査結果: research/investigations/2026-01-26_subagent-creation-best-practices.md
