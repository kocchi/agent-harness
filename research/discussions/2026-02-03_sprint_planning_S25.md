# Sprint Planning S25 - 2026-02-03
# Place: sprint_planning
# Sprint: S25

## 入力

- product-goal-contract-Q2
- insight-contract-S24
- S24 の結果（進捗ベース運用確立、Retro 入力に release-notes 追加）

## 分析（Step 1）

```yaml
analysis:
  focus: "Product Goal Q2 の次の焦点"
  risk: "習慣化に終始して Product Goal が進まない"
  context: "S24 でプロセス改善が完了。方式 A の具体化、2 人目導入の準備が候補"
  mental_model_trap: "プロセス改善が終わったら休む"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: "Product Goal と Sprint のバランスを重視。"
  coach_b: "測定可能な成果を重視。曖昧なスコープを避ける。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Mary Poppendieck**（Lean Software Development 著者）
- **Tom Gilb**（Software Engineering Management 著者）

### 議論の過程

| speaker | statement |
|---------|------------|
| Mary Poppendieck | S25 の焦点: S24 でプロセス改善が完了。Product Goal Q2 の「チーム適用」「OSS の境界」に戻る。方式 A の具体化か、2 人目導入の準備か。 |
| Tom Gilb | 測定可能に。方式 A は「コンフリクト多発時」に検討と oss-boundary-design にあった。2 人目導入は H6 の検証。ブロッカーがなければ、どちらも着手可能。S25 は「Product Goal の道筋を具体化する」slice を追加。 |
| Mary Poppendieck | 収束。S25 Goal: Product Goal Q2 の次の一歩を具体化する。方式 A の設計ドラフト作成が現実的（2 人目は人がいれば検証）。 |

### 収束

S25 Goal: 方式 A（vendor 配置）の設計ドラフトを作成し、oss-boundary-design に具体手順を追記する。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "方式 A の設計が具体化されれば、コンフリクト多発時に実装判断が速くなる"
```

## 出力

- sprint-goal-contract-S25
- slice-contract-S25
