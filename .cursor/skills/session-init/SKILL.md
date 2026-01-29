# Session Init Skill

セッション開始時に自動実行される初期化スキル。
debt-monitor、coverage-check、ledger-query を呼び出し、
セッション開始前に必要な情報を収集・報告する。

## トリガー

- セッションが選択されたとき
- Task Orchestrator の session_initializing 状態

## 契約

```yaml
name: SessionInit
version: "1.0.0"

input:
  session_type: string  # 選択されたセッション
  context: string       # タスクのコンテキスト

output:
  debt_alerts: list     # DEBT アラート
  coverage_report: object  # 軸カバレッジ
  related_learnings: list  # 関連する過去の学び
  improvement_status: object  # 改善提案の状況
  housekeeping: object  # 棚卸し推奨（レトロ時のみ）
  recommendations: list # 推奨事項
  ready: boolean        # セッション開始可能か
```

## 実行手順

### Step 1: DEBT チェック

**呼び出すスキル**: debt-monitor

```yaml
purpose: "技術的負債の蓄積を検出"
action:
  - Learning Ledger から失敗パターンを検索
  - DEBT-003（誤分類）をチェック
  - DEBT-004（fail 率）をチェック
output:
  - 検出された DEBT アラートのリスト
  - 各アラートの severity と推奨対応
```

**報告フォーマット**:
```
## DEBT アラート

- DEBT-003: Task Classifier 誤分類 (3回検出)
  → 推奨: Task Classifier のルールを見直す
  
- DEBT-004: fail 率 35% (閾値 30% 超過)
  → 推奨: 失敗パターンを分析し、根本原因を特定
```

### Step 2: Coverage チェック

**呼び出すスキル**: coverage-check

```yaml
purpose: "人格選択の多様性を確保"
action:
  - PersonaStrategy の記録から使用軸を抽出
  - 全6軸に対するカバレッジを計算
output:
  - axis_coverage: 使用された軸の割合
  - unused_axes: 未使用の軸リスト
  - recommendation: 推奨する軸
```

**報告フォーマット**:
```
## Coverage レポート

- 軸カバレッジ: 4/6 (67%)
- 未使用軸: ["分析 vs 直感", "リスク vs 機会"]
- 推奨: 次のセッションで「分析 vs 直感」軸を使用
```

### Step 3: 関連する学びの検索

**呼び出すスキル**: ledger-query

```yaml
purpose: "過去の学びを活用"
action:
  - コンテキストからキーワードを抽出
  - Learning Ledger から関連エントリを検索
  - learning タイプのエントリを優先
output:
  - 関連するエントリのリスト
  - 各エントリの概要
```

**報告フォーマット**:
```
## 関連する学び

1. [LEDGER-2026-01-28T19:32:00Z] 統合の早さと中間生成物の提示
   - 統合が早すぎた → executor に feedback を返すべき
   - 関連度: 高

2. [LEDGER-2026-01-28T19:30:00Z] VISION 違反を検出
   - 人格候補リストは編成表 → VISION 違反
   - 関連度: 中
```

### Step 4: 推奨事項の生成

各チェック結果を統合し、推奨事項を生成：

```yaml
recommendations:
  - type: "warning"
    message: "DEBT-003 アラートが発生しています"
    action: "Task Classifier のルールを見直してください"
  
  - type: "suggestion"
    message: "「分析 vs 直感」軸が未使用です"
    action: "このセッションで使用を検討してください"
  
  - type: "learning"
    message: "過去に「統合が早すぎた」という学びがあります"
    action: "複数イテレーションを意識してください"
```

### Step 5: セッション開始可否の判定

```yaml
ready_criteria:
  - 重大な DEBT アラートがないこと
  - または、アラートを認識したうえでの継続
  
blocking_conditions:
  - DEBT-003 が 5 回以上検出
  - DEBT-004 の fail 率が 50% 以上
  
action_on_block:
  - ユーザーに報告
  - retrospective セッションを推奨
```

### Step 4b: 改善提案の状況確認

**呼び出すスキル**: improvement-lifecycle（list_by_state）

```yaml
purpose: "実験中の改善提案を表示"
action:
  - list_by_state('try') で Try 状態のアイテムを取得
  - 進捗確認を促す
output:
  - Try 状態の改善提案リスト
  - 各提案の acceptance_criteria
```

**報告フォーマット**:
```
## 改善提案の状況

### Try 状態（実験中）
- [IMP-001] session-init を実際に動かす検証
  → 受け入れ基準: DEBT/Coverage/学びが表示される
  → 推奨: 実験を継続し、効果を評価
```

### Step 4c: 棚卸し推奨（レトロ時のみ）

**条件**: session_type が "retrospective" の場合のみ実行

**呼び出すスキル**: improvement-lifecycle（list_stale）

```yaml
purpose: "長期間停滞している改善提案を表示"
action:
  - list_stale('try', 30) で 30日以上停滞中の Try を取得
  - list_stale('keep', 30) で 30日以上停滞中の Keep を取得
  - 棚卸し判断を促す
output:
  - 停滞中の改善提案リスト
  - 推奨アクション
```

**報告フォーマット**:
```
## 棚卸し推奨

### Try 状態（30日以上停滞）
- [IMP-002] ○○の改善（45日）
  → 推奨: 評価して Keep か Stop を決定

### Keep 状態（30日以上）
- [IMP-003] △△の改善
  → 推奨: 安定していれば Rule に昇格、不要なら Stop
```

## 出力例

```yaml
debt_alerts:
  - id: "DEBT-003"
    count: 2
    severity: "low"
    message: "Task Classifier 誤分類が 2 回検出（閾値: 3）"

coverage_report:
  axis_coverage: 0.67
  total_axes: 6
  used_axes: ["追加 vs 削減", "短期 vs 長期", "構造 vs 柔軟", "局所 vs 全体"]
  unused_axes: ["分析 vs 直感", "リスク vs 機会"]
  recommendation: "次のセッションで「分析 vs 直感」軸を使用"

related_learnings:
  - id: "LEDGER-2026-01-28T19:32:00Z"
    content: "学び: 統合の早さと中間生成物の提示"
    relevance: "high"

recommendations:
  - type: "suggestion"
    message: "「分析 vs 直感」軸が未使用です"
    action: "このセッションで使用を検討してください"
  - type: "learning"
    message: "過去に「統合が早すぎた」という学びがあります"
    action: "複数イテレーションを意識してください"

ready: true
```

## エラーハンドリング

| エラー | 対応 |
|--------|------|
| debt-monitor 失敗 | 警告を出して継続 |
| coverage-check 失敗 | 警告を出して継続 |
| ledger-query 失敗 | 警告を出して継続 |
| 全て失敗 | ready = true で継続（初期化はオプショナル） |

## 参照

- [debt-monitor.md](./debt-monitor.md)
- [coverage-check.md](./coverage-check.md)
- [ledger-query.md](./ledger-query.md)
- [improvement-lifecycle.md](./improvement-lifecycle.md)
- [sessions/session-schema.yaml](../sessions/session-schema.yaml)
- [Improvement Lifecycle Spec](../../../specs/improvement-lifecycle.yaml)
