# Sprint Review 場

Increment を検証する場。persona-council サブエージェントに委譲する。

## 入力

- sprint-goal-contract
- slice-contract（関連）
- verification-report（あれば）
- 成果物（実装、契約の変更など）

## Step 1: 分析（ペルソナ選定の変数）

対象を分析し、以下の変数を得る:

```yaml
analysis:
  focus: "検証完了の確認 | 価値の届き方"
  risk: "見落とし、success_criteria の曖昧さ"
  context: "全 slice 完了 | 一部完了 | 方向転換"
  mental_model_trap: "検証報告があれば十分と早合点する"
```

## Step 2: 召喚条件

VISION の必要な構成: **QA + 価値重視**

```yaml
summon_criteria:
  qa: "検証可能性を厳格に問う人物。DoD や success_criteria の曖昧さを指摘する傾向。実名で選出。"
  value: "価値（何が届いたか、誰が得するか）を重視する人物。成果物の価値伝達を問う。実名で選出。"
  constraint: "LLM の内部知識から選出。speaker は実名で記録する。"
```

## Step 3: 議論（persona-council に委譲）

選定された複数人格（実名）が議論する。議論の過程を research/discussions/ に記録する。

## Step 4: 統合

```yaml
integration:
  stop_doing: []
  start_doing: []
  preserve: []
  next_hypothesis: "次に検証する仮説"
```

## 出力

- insight-contract（Review の成果を反映）
- discussion_log（議論の過程）
- sprint-goal-contract の retro_actions を done に更新（該当するもの）

## 参照

- [VISION.md - 場の一覧](../../docs/VISION.md)
- [insight スキル](../skills/insight/SKILL.md)
