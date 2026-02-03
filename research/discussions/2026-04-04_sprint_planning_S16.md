# Sprint Planning S16
# Place: sprint_planning
# 2026-04-04

## 入力

- product-goal-contract-Q2
- Q1 の insight-contract S7-S15

## 分析

```yaml
focus: "断捨離の棚卸し、OSS 境界の設計検討"
risk: "削りすぎて必要なものがなくなる"
context: "Q2 開始。Product Goal に断捨離と OSS 境界が含まれる"
mental_model_trap: "全部必要だと思い込む"
```

## 人格選定（persona-council）

- **Mary Poppendieck**: シンプルさ、無駄の削除を重視
- **Tom Gilb**: 測定可能な基準で判断

## 議論の過程

| speaker | statement |
|---------|-----------|
| Mary Poppendieck | 断捨離は「使われているか」で判断。core-rules、implementation-rules、契約フローで参照されているものが必須。参照されていないものは候補。 |
| Tom Gilb | 棚卸しを先に。ルール・スキル・エージェントの参照元を一覧化。参照ゼロなら削除候補。参照ありでも、その参照元が不要なら連鎖して検討。 |
| Mary Poppendieck | S16 の Goal: 棚卸しを実施し、削除候補を特定。削除は代替経路の確認後に実行（P3）。 |

## 統合

```yaml
integration:
  next_hypothesis: "棚卸しで可視化すると、断捨離の判断が明確になる"
```

## 出力

- sprint-goal-contract-S16
- slice-contract-S16-danshari
