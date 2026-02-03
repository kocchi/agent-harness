---
name: metrics
description: |
  Product Goal の success_metrics を集計するスキル。契約活用率、手戻り率、自律度を可視化する。
  使用タイミング: 四半期レビュー、Sprint Review 前、「メトリクス確認」と言われたとき。
---

# Metrics Skill

契約とプロセスのメトリクスを集計する。

## トリガー

- 四半期レビュー前
- Sprint Review 前（Sprint Goal 達成の補足）
- 「メトリクス確認」「契約活用率は？」と言われたとき

## 前提条件

- `contracts/` に product-goal-contract が存在する

## 実行手順

### 1. 契約活用率の集計

```yaml
定義: "スプリントで参照された契約数 / 関連契約数"
集計対象:
  - sprint-goal-contract: slice-contract の parent 参照、insight の discussion_ref
  - slice-contract: sprint-goal からの参照
  - insight-contract: discussion_ref で research/discussions を参照
算出: (参照された契約数) / (総契約数) * 100
```

### 2. 手戻り率（S11 で実装）

```yaml
定義: "修正が必要だった成果物数 / 成果物総数"
集計: insight の next_action（type: rule|skill）、retro_actions の修正指示から推定
算出: (修正指示があった成果物数) / (成果物総数) * 100
```

### 3. 自律度の集計（S13 で実装）

```yaml
定義: "介入頻度 = 人間が明示的に判断を求めた回数（セッション単位）"
集計: ユーザーが「どれにする？」「確認して」等で判断を求めた回数
目標: 6ヶ月で 50% 減
```

### 4. レポート生成

```yaml
metrics_report:
  generated_at: "YYYY-MM-DD"
  contract_usage_rate: "0%"
  rework_rate: "0%"
  autonomy: "介入頻度 N 回/セッション"
```

## 出力

- ユーザーへの Markdown レポート
- product-goal-contract の success_metrics.current を更新（オプション）

## 参照

- [product-goal-contract](../../../contracts/product-goal-contract.yaml)
- [VISION.md - 成功指標](../../../docs/VISION.md)
