# Sprint Planning 場

スプリント目標を決める場。persona-council サブエージェントに委譲する。

## 入力

- product-goal-contract
- sprint-goal-contract（ドラフト）
- 前スプリントの insight-contract（あれば）

## Step 1: 分析（ペルソナ選定の変数）

対象を分析し、以下の変数を得る:

```yaml
analysis:
  focus: "価値検証 | 実装 | 技術負債 | 探索 | ..."
  risk: "スコープ creep | 依存 | 未知の技術 | ..."
  context: "初回 | 継続 | 方向転換後 | ..."
  mental_model_trap: "チームが陥りがちな思考パターン（1行）"
```

## Step 2: 召喚条件

分析変数に基づき、**条件のみ**を記載。具体名は挙げない。LLM の内部知識から選出させる。

```yaml
summon_criteria:
  coach_a: |
    {{focus}} に厳格で、{{mental_model_trap}} を書き換える視点を持つ人物。
    断定せず仮説として提示する傾向。
  coach_b: |
    {{risk}} への対策を重視し、Coach A と補完的な視点を持つ人物。
  constraint: "LLM の内部知識から選出。有名かどうかは問わず、条件への適合度で選ぶ。"
```

## Step 3: 議論（persona-council に委譲）

選定された複数人格が議論する。議論の過程を記録する。

## Step 4: 統合

```yaml
integration:
  stop_doing: "棄却する習慣"
  start_doing: "獲得する習慣"
  preserve: "守るべき資産"
  next_hypothesis: "次に検証する仮説"
```

## 出力

- sprint-goal-contract（更新版）
- discussion_log（議論の過程）
