# Debt Monitor

認識済み技術的負債を監視し、トリガー条件を満たしたらアラートを発行する。

## トリガー

- watcher の判断後に自動実行
- コミット前に自動実行
- 「負債を確認して」などの手動要求

## 契約

```yaml
name: DebtMonitor
version: "0.1.0"

input:
  debt_ids?: string[]  # 特定の負債のみチェック（省略時は全て）

output:
  triggered_debts: TriggeredDebt[]
  alerts: Alert[]
  recommendations: string[]
  summary: string

guarantees:
  - 全ての監視対象負債をチェック
  - トリガー発火時は必ずアラート
  - 推奨アクションを必ず提示
```

## 実行手順

### Step 1: 負債定義を読み込む

```yaml
path: "source/meta/monitors/debt-triggers.yaml"
```

### Step 2: 各負債のトリガー条件をチェック

```yaml
# DEBT-003 の場合（パターンマッチ）
check:
  1. ledger-query を呼び出す
     pattern: "誤分類"
     filters:
       component: ["watcher"]
  
  2. count >= threshold (3) か判定
  
  3. 条件を満たしたら triggered_debts に追加

# DEBT-004 の場合（統計 + サーキットブレーカー）
check:
  # サーキットブレーカー（Taleb 設計）: 先にチェック
  1. 直近のエントリで連続 fail をカウント
     - 5連続以上 → 即座にトリガー（統計計算をバイパス）
  
  # 統計的判定（Kahneman 設計）
  2. サンプルサイズ確認
     - n < 20 → insufficient_data → トリガーしない
  
  3. query_stats を呼び出す
     metric: "fail_rate"
     window: { type: "last_n", value: 30 }
  
  4. Wilson 区間の下限が閾値（30%）を超えるか判定
     - ci_lower > 0.30 → 有意
  
  5. 連続2回有意ならトリガー
```

### Step 3: アラート生成

```yaml
for each triggered_debt:
  alert:
    id: debt.id
    title: debt.title
    severity: debt.severity
    message: debt.action.message
    recommendations: debt.recommendations
```

### Step 4: Learning Ledger に記録

```yaml
component: "DebtMonitor"
event_type: "debt_recognized"
content: "DEBT-003 がトリガーされました"
rationale: "誤分類が {count} 回検出。閾値 {threshold} を超過。"
```

### Step 5: 結果返却

```yaml
output:
  triggered_debts: [...]
  alerts: [...]
  recommendations: [...]
  summary: "N件の負債がトリガーされました。"
```

## 例

### 例1: DEBT-003 がトリガーされる場合

**条件**: 誤分類が 3 回以上記録されている

**出力**:
```yaml
triggered_debts:
  - id: DEBT-003
    title: "Task Classifier の判断基準が暗黙的"
    trigger_count: 3
    threshold: 3

alerts:
  - id: DEBT-003
    severity: high
    message: |
      DEBT-003 がトリガーされました。
      Task Classifier の判断基準が曖昧である可能性があります。

recommendations:
  - "誤分類パターンを ledger-query で検索: pattern='誤分類'"
  - "task-classifier.md の判定ルールを見直す"

summary: "1件の負債がトリガーされました: DEBT-003"
```

### 例2: トリガーなし

**条件**: 誤分類が 2 回以下

**出力**:
```yaml
triggered_debts: []
alerts: []
recommendations: []
summary: "トリガーされた負債はありません。"
```

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| debt-triggers.yaml が読めない | 空結果を返す |
| ledger-query が失敗 | その負債をスキップ |
| Learning Ledger 記録失敗 | stderr にフォールバック |

## 参照

- [debt-triggers.yaml](../monitors/debt-triggers.yaml)
- [ledger-query.md](./ledger-query.md)
- [Learning Ledger](../ledger/learning-ledger.yaml)
