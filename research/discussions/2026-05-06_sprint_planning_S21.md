# Sprint Planning S21 - 2026-05-06
# Place: sprint_planning
# 2026-05-06

## 入力

- product-goal-contract-Q2
- insight-contract-S20（Retro でリリースノートを start_doing に追加）
- Sprint Retro S20 の議論

## 分析（Step 1）

```yaml
analysis:
  focus: "リリースノートの導入、Review の入力の質向上"
  risk: "リリースノートが形骸化する"
  context: "S20 Retro で「リリースノートがないと Review が厳しい」と指摘。S21 に含める"
  mental_model_trap: "リリースノートを書くことが目的化する"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: "価値の可視化を重視。誰に何が届いたかを明確にする視点。"
  coach_b: "実装可能性と検証方法を重視。曖昧な成果物を避ける傾向。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Mary Poppendieck**（Lean Software Development 著者）: 価値の可視化、無駄の削除
- **Tom Gilb**（Software Engineering Management 著者）: 測定可能な基準、検証方法を重視

### 議論の過程

| speaker | statement |
|---------|-----------|
| Mary Poppendieck | S21 の Goal: リリースノートを Sprint 終了時に生成する仕組みを導入し、sprint-review の入力に含める。形式は research/releases/S{NN}-release-notes.md。届けた価値、変更点、検証結果を1ページにまとめる。 |
| Tom Gilb | success_criteria: (1) research/releases/ に S21 のリリースノートが存在する、(2) sprint-review 場の入力に release_notes が追加されている、(3) 初回 Review（S21 終了時）でリリースノートが materials に含まれる。 |
| Mary Poppendieck | リリースノートは「書くこと」が目的ではなく、Review の質を高める手段。形式はシンプルに: 届けた価値、変更ファイル、検証結果。 |

### 収束

S21 Goal: リリースノートを Sprint 終了時に生成する仕組みを導入し、sprint-review の入力に含める。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "リリースノートがあると、persona-council のレビュー品質が上がる"
```

## 出力

- sprint-goal-contract-S21
- slice-contract-S21-release-notes
