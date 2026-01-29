# Ledger Query

Learning Ledger を検索し、パターンを見つける。

## トリガー

- 「誤分類を検索」「失敗パターンを見つけて」などの検索要求
- 負債監視で自動呼び出し
- 盲点カバー率計算で自動呼び出し

## 契約

```yaml
name: LedgerQuery
version: "0.1.0"

input:
  pattern: string  # 検索パターン（正規表現）
  filters:
    component?: string[]  # コンポーネントでフィルタ
    event_type?: string[]  # イベントタイプでフィルタ
    date_range?: { start: string, end: string }

output:
  entries: LedgerEntry[]
  count: number
  summary: string  # 検索結果の要約

guarantees:
  - パターンに一致するエントリを全て返す
  - フィルタは AND 条件で適用
  - 結果は timestamp 降順（新しい順）
```

## 実行手順

### Step 1: entries.yaml を読み込む

```yaml
path: "learnings/flow/entries.yaml"
```

### Step 2: パターンマッチング

```yaml
対象フィールド:
  - content
  - rationale
  
正規表現:
  - 大文字小文字を区別しない
  - 部分一致
```

### Step 3: フィルタ適用

```yaml
filters:
  component: entries.component IN filters.component
  event_type: entries.event_type IN filters.event_type
  date_range: filters.date_range.start <= entries.timestamp <= filters.date_range.end
```

### Step 4: 結果返却

```yaml
output:
  entries: マッチしたエントリ（新しい順）
  count: エントリ数
  summary: "X件のエントリが見つかりました。最新: {latest_content}"
```

## 例

### 例1: 誤分類を検索

**入力**:
```yaml
pattern: "誤分類"
filters: {}
```

**出力**:
```yaml
entries:
  - id: "LEDGER-2026-01-28T19:30:00Z"
    content: "VISION 違反を検出: 人格候補リスト = 編成表"
    rationale: "..."
count: 1
summary: "1件のエントリが見つかりました。"
```

### 例2: watcher の判断を検索

**入力**:
```yaml
pattern: ".*"  # 全件
filters:
  component: ["watcher"]
  event_type: ["decision"]
```

**出力**:
```yaml
entries:
  - id: "LEDGER-..."
    component: "watcher"
    event_type: "decision"
    content: "pass = true"
    rationale: "..."
  # ... more
count: N
summary: "N件のエントリが見つかりました。"
```

### 例3: pass=false を検索（負債監視用）

**入力**:
```yaml
pattern: "pass.*false"
filters:
  component: ["watcher"]
```

**出力**:
```yaml
entries: [...]
count: M
summary: "M件の fail が見つかりました。"
```

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| entries.yaml が読めない | 空配列を返す |
| パターンが無効 | エラーを返す |
| マッチなし | entries: [], count: 0 |

---

## query_stats（Phase 3.2 追加）

統計クエリを実行する。設計者: Kahneman + Taleb。

### 契約

```yaml
input:
  metric: "fail_rate" | "pass_rate" | "count"
  window: { type: "last_n", value: N } | { type: "all" }
  filters: { component?: [], event_type?: [] }

output:
  value: number  # 比率（0.0 - 1.0）
  sample_size: number
  confidence_interval: { lower, upper }  # Wilson スコア区間
  sufficient_data: boolean  # n >= 20
  raw_counts: { numerator, denominator }
```

### fail_rate の計算

```yaml
query:
  filters:
    component: ["watcher"]
    event_type: ["decision"]

numerator: content に "pass = false" を含むエントリ数
denominator: 全エントリ数

fail_rate = numerator / denominator
```

### Wilson スコア区間

```
# 95% 信頼区間（サンプルサイズが小さくても有効）
z = 1.96  # 95%
p = successes / trials

center = (p + z²/2n) / (1 + z²/n)
margin = z * sqrt(p(1-p)/n + z²/4n²) / (1 + z²/n)

lower = max(0, center - margin)
upper = min(1, center + margin)
```

### 例: fail 率の計算

**入力**:
```yaml
metric: "fail_rate"
window: { type: "last_n", value: 30 }
filters: { component: ["watcher"], event_type: ["decision"] }
```

**出力**（仮に 30件中8件 fail の場合）:
```yaml
value: 0.267  # 26.7%
sample_size: 30
confidence_interval:
  lower: 0.14  # 14%
  upper: 0.44  # 44%
sufficient_data: true  # 30 >= 20
raw_counts:
  numerator: 8
  denominator: 30
```

→ 信頼区間の下限（14%）が閾値（30%）を下回るため、DEBT-004 はトリガーしない

---

## 参照

- [Learning Ledger 契約](../ledger/learning-ledger.yaml)
- [debt-monitor.md](./debt-monitor.md)
- [debt-triggers.yaml](../monitors/debt-triggers.yaml)
- [coverage-check.md](./coverage-check.md)
