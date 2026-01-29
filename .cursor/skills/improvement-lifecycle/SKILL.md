# Improvement Lifecycle Skill

改善提案を段階的に昇格させ、ルール/スキルに反映するスキル。

## トリガー

- レトロスペクティブで改善提案が出たとき
- 改善提案の状態を遷移させるとき
- 棚卸しを行うとき

## 契約

```yaml
name: ImprovementLifecycle
version: "1.0.0"
spec: "repos/specs/improvement-lifecycle.yaml"

# 状態
states: [problem, try, keep, rule, stopped]

# 状態遷移
transitions:
  problem → try: try_proposed
  try → keep: try_evaluated (effective)
  try → stopped: try_evaluated (ineffective)
  keep → rule: keep_promoted
  keep → stopped: stopped (housekeeping)
  rule → stopped: stopped (housekeeping)
```

## 使い方

### 1. 改善提案を登録する（Problem → Try）

レトロスペクティブで出た改善提案を Try として登録：

```yaml
# Learning Ledger に記録
- id: "LEDGER-2026-01-28T23:00:00Z"
  timestamp: "2026-01-28T23:00:00Z"
  component: "ImprovementLifecycle"
  event_type: "try_proposed"
  content: "session-init を実際に動かす検証"
  rationale: "セッション開始時の自動チェックが機能するか確認が必要"
  improvement_id: "IMP-2026-01-28T23:00:00Z"  # 新規 ID
  acceptance_criteria: "DEBT/Coverage/学びが表示される"
  target: null  # プロセス変更なのでファイル変更なし
```

### 2. トライを評価する（Try → Keep or Stop）

実験を行い、効果を評価：

**効果あり（Keep へ）**:
```yaml
- id: "LEDGER-2026-01-29T10:00:00Z"
  component: "ImprovementLifecycle"
  event_type: "try_evaluated"
  content: "session-init の検証完了"
  rationale: "3回実行し、毎回 DEBT/Coverage/学びが正しく表示された"
  ref: "IMP-2026-01-28T23:00:00Z"
  result: "effective"
```

**効果なし（Stop へ）**:
```yaml
- id: "LEDGER-2026-01-29T10:00:00Z"
  component: "ImprovementLifecycle"
  event_type: "try_evaluated"
  content: "session-init の検証完了"
  rationale: "3回実行したが、パフォーマンスに問題があり実用的でない"
  ref: "IMP-2026-01-28T23:00:00Z"
  result: "ineffective"
```

### 3. ルール化する（Keep → Rule）

安定して効果がある改善をルール/スキルに反映：

```yaml
- id: "LEDGER-2026-02-01T10:00:00Z"
  component: "ImprovementLifecycle"
  event_type: "keep_promoted"
  content: "イテレーション省略禁止をルール化"
  rationale: "30日間運用し、問題なく機能している"
  ref: "IMP-2026-01-28T23:01:00Z"
  target: "rules/implementation-rules.yaml"
  change_description: |
    追加したルール:
    - 「技術的に明らか」でもセッション定義のイテレーション回数を守る
    - watcher がイテレーション回数をチェックする
```

### 4. 棚卸しで廃止する（Keep/Rule → Stop）

不要になった改善を廃止：

```yaml
- id: "LEDGER-2026-03-01T10:00:00Z"
  component: "ImprovementLifecycle"
  event_type: "stopped"
  content: "○○ルールを廃止"
  rationale: "別のアプローチに置き換わったため不要に"
  ref: "IMP-2026-01-28T23:02:00Z"
  reason: "superseded"
```

## クエリ

### list_by_state(state)

特定状態のアイテムを取得：

```yaml
input:
  state: "try"  # problem, try, keep, rule, stopped

output:
  items:
    - improvement_id: "IMP-2026-01-28T23:00:00Z"
      content: "session-init を実際に動かす検証"
      current_state: "try"
      last_updated: "2026-01-28T23:00:00Z"
```

**実装方法**:
1. Learning Ledger から `component: "ImprovementLifecycle"` のエントリを取得
2. `improvement_id` または `ref` でグループ化
3. 各グループの最新イベントから現在状態を計算
4. 指定状態でフィルタ

### list_stale(state, days)

長期間停滞しているアイテムを取得：

```yaml
input:
  state: "try"
  days: 30

output:
  items:
    - improvement_id: "IMP-2026-01-01T10:00:00Z"
      content: "古いトライ"
      current_state: "try"
      last_updated: "2026-01-01T10:00:00Z"
      stale_days: 28
```

**実装方法**:
1. `list_by_state(state)` を実行
2. `last_updated` が `days` 日以上前のものをフィルタ

### get_history(improvement_id)

アイテムの状態遷移履歴を取得：

```yaml
input:
  improvement_id: "IMP-2026-01-28T23:00:00Z"

output:
  events:
    - timestamp: "2026-01-28T23:00:00Z"
      event_type: "try_proposed"
      state_after: "try"
    - timestamp: "2026-01-29T10:00:00Z"
      event_type: "try_evaluated"
      result: "effective"
      state_after: "keep"
```

## セッション開始時の表示

`session-init` で自動実行される内容：

```
## 改善提案の状況

### Try 状態（実験中）
- [IMP-001] session-init を実際に動かす検証
  → 受け入れ基準: DEBT/Coverage/学びが表示される
  → 残り実験回数: 2回

### 停滞中（30日以上）
- [IMP-002] ○○の改善（45日）
  → 推奨: 評価して Keep か Stop を決定
```

## 棚卸しの推奨

レトロスペクティブ時に自動表示：

```
## 棚卸し推奨

### Keep 状態（30日以上）
- [IMP-003] △△の改善
  → 推奨: 安定していれば Rule に昇格、不要なら Stop

### Rule 状態（最終更新から90日以上）
- [IMP-004] □□ルール
  → 推奨: まだ有効か確認、不要なら Stop
```

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| 無効な状態遷移 | reject、有効な遷移を提示 |
| ref が見つからない | reject、improvement_id を確認 |
| acceptance_criteria なし | reject、基準の定義を要求 |

## 参照

- [Spec](../../../specs/improvement-lifecycle.yaml)
- [Learning Ledger](../ledger/learning-ledger.yaml)
- [Retrospective Session](../sessions/retrospective.yaml)
- [session-init](./session-init.md)
