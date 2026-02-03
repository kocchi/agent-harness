# Rules Validator Agent

ルールの整合性を検査するエージェント。
セッション開始時（P0）、Plan フェーズ、コミット前に呼び出される。

## 入力

- timing: session_start | plan | pre_commit
- files: 変更対象ファイル（任意）

## 検査対象

```
.cursor/rules/
├── constitution.mdc      # 憲法（P0-P7）
├── core-rules.mdc        # コアルール
└── implementation-rules.mdc  # 実装ルール
```

## 検査プロトコル

### 1. ルールファイルの構文チェック

各 `.mdc` ファイルが:
- フロントマターが正しいか（任意）
- Markdown の構造が正しいか
- 必須セクションが存在するか

### 2. 参照の整合性チェック

エージェント/スキルが参照するパスが存在するか:

```yaml
check_paths:
  agents:
    - ".cursor/agents/*.md"
  skills:
    - ".cursor/skills/*/SKILL.md"
  rules:
    - ".cursor/rules/*.mdc"
  roles:
    - ".cursor/roles/*.md"
  personas:
    - ".cursor/personas/custom/"
  places:
    - ".cursor/places/*.md"
```

### 3. ルール間の矛盾チェック

- constitution.mdc の原則と他ルールが矛盾していないか
- 「必須」と「禁止」が衝突していないか

## timing 別の検査内容

| timing | 主な検査内容 |
|--------|-------------|
| session_start | 全ルールの整合性、参照チェック |
| plan | Plan が constitution に沿っているか |
| pre_commit | 変更ファイルがルールに違反していないか |

## 出力形式

```yaml
timing: session_start
status: OK | NG
summary:
  total_checks: 12
  passed: 11
  failed: 1

checks:
  - category: "syntax"
    file: ".cursor/rules/core-rules.mdc"
    status: OK
    
  - category: "reference"
    file: ".cursor/agents/rules-validator.md"
    status: OK
    detail: "全ての参照パスが存在"
    
  - category: "reference"
    file: ".cursor/agents/old-agent.md"
    status: NG
    detail: "source/meta/rules/*.yaml を参照（存在しないパス）"
    action: ".cursor/rules/*.mdc に更新"

recommendations:
  - "参照エラーを修正してください"
```

## 使用方法

### セッション開始時（P0）

```
セッション開始時のルール検査を実行してください。
timing: session_start
```

### Plan 作成後

```
計画がルールに沿っているか検査してください。
timing: plan
```

### コミット前

```
コミット前にルールを検査してください。
timing: pre_commit
```

## 他のエージェントとの連携

- ~~ssot-validator~~: 非推奨（直接編集運用のため）
- systematic-debugger: 問題発生時の要因分割

## 参照

- [constitution.mdc](../rules/constitution.mdc)
- [core-rules.mdc](../rules/core-rules.mdc)
- [implementation-rules.mdc](../rules/implementation-rules.mdc)
