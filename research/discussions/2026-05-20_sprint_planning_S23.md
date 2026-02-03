# Sprint Planning S23 - 2026-05-20
# Place: sprint_planning
# 2026-05-20

## 入力

- product-goal-contract-Q2
- insight-contract-S22（release-notes 習慣化の初回検証成功）

## 分析（Step 1）

```yaml
analysis:
  focus: "release-notes の継続、Product Goal Q2 の他項目への着手"
  risk: "release-notes に集中しすぎて Product Goal が進まない"
  context: "S21-S22 でリリースノートの仕組みが動いた。S23 で習慣化を継続しつつ、余力で OSS 境界や H6 に着手"
  mental_model_trap: "習慣化の検証だけに終始する"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: "習慣化の継続と Product Goal のバランスを重視。"
  coach_b: "測定可能な成果を重視。曖昧なスコープを避ける傾向。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Mary Poppendieck**（Lean Software Development 著者）
- **Tom Gilb**（Software Engineering Management 著者）

### 議論の過程

| speaker | statement |
|---------|-----------|
| Mary Poppendieck | S23 の Goal: release-notes の習慣化を継続しつつ、Product Goal Q2 の「OSS の境界」に着手する。oss-boundary-design で symlink の検証が次とあった。S23 で symlink の動作を検証する slice を追加する。 |
| Tom Gilb | success_criteria を測定可能に。(1) S23 終了時に S23-release-notes.md が存在する（習慣化の継続）、(2) OSS 境界の symlink 検証結果が research に記録されている。 |
| Mary Poppendieck | 同意。S23 は「習慣化の継続」と「symlink 検証」の 2 本立て。 |

### 収束

S23 Goal: release-notes の習慣化を継続し、OSS 境界の symlink 検証を実施する。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "symlink が動けば方式 A（vendor 配置）の可否が判断できる"
```

## 出力

- sprint-goal-contract-S23
- slice-contract-S23-symlink
