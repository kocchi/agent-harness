# Coverage Check

盲点カバー率（axis_coverage）を計算し、多様性を監視する。

## トリガー

- 「カバー率を確認して」などの手動要求
- 定期的な健全性チェック
- コミット前の自動実行

## 契約

```yaml
name: CoverageCheck
version: "0.1.0"

input:
  window: number  # 直近N件を対象（デフォルト: 10）

output:
  axis_coverage: number  # 使用された軸の割合（0.0〜1.0）
  used_axes: string[]  # 使用された軸
  unused_axes: string[]  # 使用されていない軸
  trend: "improving" | "declining" | "stable"
  recommendations: string[]
  summary: string

guarantees:
  - 計算結果は一貫性がある
  - 未使用軸を必ず報告
  - 改善推奨を提示
```

## 歪み軸の定義

```yaml
# persona-strategy.md から参照
axes:
  - name: "削減 vs 追加"
    values: ["削減", "追加"]
  - name: "短期 vs 長期"
    values: ["短期", "長期"]
  - name: "顧客 vs システム"
    values: ["顧客", "システム"]
  - name: "理想 vs 現実"
    values: ["理想", "現実"]
  - name: "確率論 vs 決定論"
    values: ["確率論", "決定論"]
  - name: "分析 vs 直感"
    values: ["分析", "直感"]

total_axes: 6
```

## 実行手順

### Step 1: Learning Ledger から人格選択を取得

```yaml
query:
  pattern: "axis"
  filters:
    component: ["PersonaStrategy"]
    event_type: ["decision"]
  limit: window
```

### Step 2: 使用された軸を抽出

```yaml
# 各エントリの content/rationale から軸を抽出
extract:
  - "executor = Kent Beck（削減軸）" → "削減"
  - "watcher = Martin Fowler（追加軸）" → "追加"
```

### Step 3: カバー率を計算

```yaml
axis_coverage = unique(used_axes).length / total_axes

# 例: 4軸使用 / 6軸 = 0.67 (67%)
```

### Step 4: トレンドを判定

```yaml
# 前回のカバー率と比較
if current > previous + 0.05:
  trend: "improving"
elif current < previous - 0.05:
  trend: "declining"
else:
  trend: "stable"
```

### Step 5: 推奨を生成

```yaml
if axis_coverage < 0.8:
  recommendations:
    - "未使用軸があります: {unused_axes}"
    - "次のタスクでは {unused_axes[0]} 軸の人格を検討してください"
```

### Step 6: 結果返却

```yaml
output:
  axis_coverage: 0.67
  used_axes: ["削減", "追加", "長期", "顧客"]
  unused_axes: ["短期", "システム", "理想", "現実", "確率論", "決定論", "分析", "直感"]
  trend: "stable"
  recommendations: ["未使用軸があります: 短期, システム, ..."]
  summary: "軸カバー率: 67%（目標: 80%）"
```

## 例

### 例1: カバー率が目標未達

**入力**:
```yaml
window: 10
```

**出力**:
```yaml
axis_coverage: 0.5
used_axes: ["削減", "追加", "長期"]
unused_axes: ["短期", "顧客", "システム", "理想", "現実", "確率論", "決定論", "分析", "直感"]
trend: "stable"
recommendations:
  - "軸カバー率が目標（80%）を下回っています: 50%"
  - "未使用軸があります: 短期, 顧客, システム..."
  - "次のタスクでは「確率論」軸の人格（例: Nassim Taleb）を検討してください"
summary: "軸カバー率: 50%（目標: 80%）⚠️"
```

### 例2: カバー率が目標達成

**入力**:
```yaml
window: 10
```

**出力**:
```yaml
axis_coverage: 0.83
used_axes: ["削減", "追加", "長期", "顧客", "確率論"]
unused_axes: ["短期", "システム", "理想", "現実", "決定論", "分析", "直感"]
trend: "improving"
recommendations: []
summary: "軸カバー率: 83%（目標: 80%）✅"
```

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| Learning Ledger にデータがない | axis_coverage: 0, used_axes: [] |
| 軸の抽出失敗 | その エントリをスキップ |
| 計算エラー | エラーを返す |

## 目標値

```yaml
targets:
  axis_coverage: ">= 0.8"  # 80%以上の軸を使用
  
alerts:
  warning: "< 0.6"  # 60%未満で警告
  critical: "< 0.4"  # 40%未満でクリティカル
```

## 参照

- [persona-strategy.md](../agents/persona-strategy.md)
- [ledger-query.md](./ledger-query.md)
- [Learning Ledger](../ledger/learning-ledger.yaml)
