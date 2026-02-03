# Sprint Planning 議論ログ
# Place: sprint_planning
# Sprint: S9
# 2026-02-15

## 入力

- product-goal-contract, insight-contract-S8
- S8 の next_hypothesis: 場の委譲が継続して機能するか

## 分析

```yaml
analysis:
  focus: "H3 検証（スキル/ルールで自律度が上がる）、契約活用率の測定基盤"
  risk: "測定しないと改善できない"
  context: "S8 で場の仕組みが確立。Product Goal の success_metrics は current: null のまま"
  mental_model_trap: "仕組みができたので測定は後回し"
```

## 人格選定

- **Don Reinertsen**: 価値の測定、フローを重視
- **Tom Gilb**: 測定可能な目標を重視（Evolutionary Project Management）

## 議論の過程

| speaker | statement |
|---------|-----------|
| Don Reinertsen | Product Goal の契約活用率 80%、手戻り率 10% は目標だけ。current が null では改善の方向がわからない。まず「契約活用率」の定義と測定方法を決める。 |
| Tom Gilb | 契約活用率 = スプリントで参照された契約数 / 関連契約数。slice-contract の parent 参照、insight の discussion_ref などで追跡可能。まずは簡易版で始める。 |
| Don Reinertsen | S9 の Goal: 契約活用率の測定基盤を作る。metrics スキルを追加し、contracts/ を走査して活用状況を集計する。 |
| Tom Gilb | スコープを小さく。metrics スキルで sprint-goal-contract、slice-contract、insight-contract の参照状況を集計。手戻り率は S11 で。 |

## 統合

```yaml
integration:
  next_hypothesis: "契約活用率を測定すると、どの契約が使われていないか可視化できる"
```
