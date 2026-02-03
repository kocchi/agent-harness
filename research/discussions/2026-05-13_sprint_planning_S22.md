# Sprint Planning S22 - 2026-05-13
# Place: sprint_planning
# 2026-05-13

## 入力

- product-goal-contract-Q2
- insight-contract-S21（release-notes の習慣化を S22 で検証）

## 分析（Step 1）

```yaml
analysis:
  focus: "release-notes の習慣化検証、Product Goal Q2 の継続"
  risk: "S22 で release-notes が実行されないと形骸化する"
  context: "S21 でリリースノートの仕組みを導入。S22 で実際に Sprint 終了時に release-notes が実行されるか検証"
  mental_model_trap: "検証を忘れて次の機能に飛ぶ"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: "習慣化と検証を重視。仕組みが形骸化しないかを問う視点。"
  coach_b: "Product Goal への貢献を忘れない視点。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Diana Larsen**（Agile Retrospectives 著者）: 習慣化、学びの還元を重視
- **Mary Poppendieck**（Lean Software Development 著者）: 価値の可視化、無駄の削除

### 議論の過程

| speaker | statement |
|---------|-----------|
| Diana Larsen | S22 の Goal: release-notes の習慣化を検証する。Sprint 終了時に release-notes を実行し、S22 Review で materials に含まれることを確認する。これができれば、リリースノートの仕組みが定着したと言える。 |
| Mary Poppendieck | Product Goal Q2 の継続も忘れない。OSS 境界の symlink 検証、H6（2人目導入）の準備など、並行して進められるものがあれば slice に含める。S22 は「release-notes 習慣化の検証」を主目標にしつつ、余力で他を進める。 |
| Diana Larsen | success_criteria: (1) S22 終了時に research/releases/S22-release-notes.md が存在する、(2) S22 Review の materials に S22-release-notes.md が含まれている。 |

### 収束

S22 Goal: release-notes の習慣化を検証する。Sprint 終了時に release-notes を実行し、Review で materials に含まれることを確認する。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "release-notes が習慣化されると、Review の質が持続的に上がる"
```

## 出力

- sprint-goal-contract-S22
- slice-contract-S22-release-notes-habit
