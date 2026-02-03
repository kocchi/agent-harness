# Sprint Planning S20
# Place: sprint_planning
# 2026-04-30

## 入力

- product-goal-contract-Q2
- insight-contract-S19
- research/oss-boundary-design-S17.md

## 分析（Step 1）

```yaml
analysis:
  focus: "OSS 境界の Phase 2 具体化、H4 検証の準備"
  risk: "Cursor 仕様の調査が不十分で設計が空中楼閣になる"
  context: "S17 で follow-upstream 確立。S18-S19 でオンボーディング整備。OSS 境界は research に設計案あり。Phase 2 で Cursor 仕様を確認してから具体化する"
  mental_model_trap: "設計を先に決めたくなる"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: "依存関係を先に、設計は後から。測定可能な前提で判断する視点。"
  coach_b: "実装可能性と検証方法を重視。曖昧な設計を避ける傾向。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Mary Poppendieck**（Lean Software Development 著者）: 依存関係を先に、設計は後から
- **Tom Gilb**（Software Engineering Management 著者）: 測定可能な基準、検証方法を重視

### 議論の過程

| speaker | statement |
|---------|-----------|
| Mary Poppendieck | OSS 境界の設計は Cursor の読み込み仕様に依存する。oss-boundary-design-S17 で「方式 A は vendor 内の rules をどう読ませるか、Cursor の include サポート要確認」とある。S20 の Goal: Cursor の .cursor/rules 読み込み仕様を調査し、境界設計の前提を明確にする。 |
| Tom Gilb | 調査の成果は測定可能に。例えば「.cursor/rules/ の include パスは〇〇」「vendor 内の .mdc は読まれるか yes/no」のように、設計判断に使える事実を列挙する。 |
| Mary Poppendieck | 調査結果を research に記録し、oss-boundary-design を更新する。Phase 2 の判断材料が揃う。 |
| Tom Gilb | success_criteria: (1) Cursor の rules 読み込み仕様が research に記録されている、(2) 方式 A/B/C のいずれが現実的か、根拠付きで判断できる状態になっている。 |

### 収束

S20 Goal: Cursor の .cursor/rules 読み込み仕様を調査し、OSS 境界の設計前提を明確にする。調査結果を research に記録し、oss-boundary-design を更新する。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "Cursor 仕様が明確になれば、方式 A（vendor 配置）の可否が判断できる"
```

## 出力

- sprint-goal-contract-S20
- slice-contract-S20-cursor-investigation
