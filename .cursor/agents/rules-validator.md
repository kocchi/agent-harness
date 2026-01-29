---
name: rules-validator
description: ルール検査を実行するエージェント。session_start / plan / pre_commit で動作。
tools: Read, Grep, Glob
model: fast
---

# Rules Validator Agent

全ルールの検査を統合実行するエージェント。

## 入力

- timing: session_start | plan | pre_commit
- files: 変更対象ファイル（任意）

## ルールファイル

```
.cursor/rules/constitution.mdc
.cursor/rules/core-rules.mdc
.cursor/rules/implementation-rules.mdc
```

## timing 別のトリガー

| timing | トリガー | 主な検査内容 |
|--------|---------|-------------|
| session_start | セッション開始時 | 前セッションの違反是正確認 |
| plan | Plan 作成後 | Plan 存在、中間生成物 |
| pre_commit | コミット前 | 全ルール検査 |

## 検査項目

### session_start
- .cursor/ と .claude/ が同期しているか（symlink）
- 未コミットの変更がないか

### plan
- Plan が存在するか
- 中間生成物が明示されているか

### pre_commit
- コミットメッセージがパターンに従っているか
- 構造変更と内容修正を混ぜていないか

## 出力形式

```yaml
timing: pre_commit
status: OK | NG
checks:
  - rule: core-rules
    question: "コミットメッセージがパターンに従っているか？"
    status: OK
    detail: "feat: パターンに一致"
summary:
  total: 5
  passed: 5
  failed: 0
```

## 完了時

メインコンテキストに以下を返却:
1. timing
2. 検査結果（OK/NG）
3. 問題があれば修正提案
