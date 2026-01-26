# Rules Validator Agent

全ルールの検査を統合実行するエージェント。
__動的検査プロトコル__: ルール定義から checks を自動抽出して検査を実行。

## 入力

- timing: session_start | plan | implement | pre_commit
- files: 変更対象ファイル（任意）

## 動的検査プロトコル

### 1. ルールファイルを読み込む

```bash
# 全ルールファイル
source/meta/rules/core-rules.yaml
source/meta/rules/implementation-rules.yaml
source/meta/rules/research-rules.yaml
```

### 2. timing に応じた checks を抽出

各ルールの `checks.[timing]` を抽出する。

```yaml
# 例: core-rules.yaml の ssot ルール
ssot:
  checks:
    session_start:
      - question: "前セッションで source/ と generated/ の不整合はなかったか？"
        method: "git status で確認"
    pre_commit:
      - question: "generate.sh を実行したか？"
        method: "git diff で確認"
```

### 3. 抽出した checks を実行

```yaml
# timing: pre_commit の場合、以下を実行:
# - ssot.checks.pre_commit
# - commit.checks.pre_commit
# - documentation.checks.pre_commit
# - vertical_slice.checks.pre_commit
# - ...
```

## timing 別のトリガー

| timing | トリガー | 主な検査内容 |
|--------|---------|-------------|
| session_start | セッション開始時 | 前セッションの違反是正確認 |
| plan | Plan 作成後 | Plan 存在、中間生成物、マルチツール考慮 |
| implement | 実装中 | バーティカルスライス進行 |
| pre_commit | コミット前 | 全ルール検査 |

## 出力形式

```yaml
timing: pre_commit
status: OK | NG
rules_checked: 14
checks:
  - rule: ssot
    timing: pre_commit
    question: "generate.sh を実行したか？"
    status: OK
    detail: "差分なし"
  - rule: commit
    timing: pre_commit
    question: "構造変更と内容修正を混ぜていないか？"
    status: OK
    detail: "単一種別の変更"
  - rule: vertical_slice
    timing: pre_commit
    question: "Plan は存在するか？"
    status: NG
    detail: "Plan なしに実装開始"
    action: "Plan を作成してから実装を再開"
summary:
  total: 8
  passed: 7
  failed: 1
```

## 使用方法

### セッション開始時

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
コミット前に全ルールを検査してください。
timing: pre_commit
```

## 検査の実行手順

1. 指定された timing を確認
2. 全ルールファイル（core, implementation, research）を読み込む
3. 各ルールから `checks.[timing]` を抽出
4. 抽出した checks を順に実行
5. 結果を構造化フォーマットで出力

## 他のバリデーターとの連携

- ssot-validator: SSoT 整合性の詳細検査
- markdown-reviewer: ai_markdown ルールの詳細検査
- systematic-debugger: 問題発生時の要因分割
