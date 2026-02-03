# Sprint Retro 場

プロセスを振り返る場。persona-council サブエージェントに委譲する。

## 入力

- 前回 Retro の議論（あれば）
- insight-contract（初版または前スプリント）
- workload-monitor の推奨（あれば）
- 本スプリントのプロセス（場の開催状況、障害など）

## Step 1: 分析（ペルソナ選定の変数）

対象を分析し、以下の変数を得る:

```yaml
analysis:
  focus: "プロセス改善、習慣化の漏れ"
  risk: "場の開催を省略する習慣、ルールと実践の乖離"
  context: "persona-council の呼び出し状況、retro_actions の更新状況"
  mental_model_trap: "効率を優先して場をスキップする"
```

## Step 2: 召喚条件

VISION の必要な構成: **批判的 + 学び重視**

```yaml
summon_criteria:
  critical: "プロセスの盲点を探す批判的視点。習慣化の漏れ、構造的問題を指摘する傾向。実名で選出。"
  learning: "学びを抽出し、次のアクションに落とし込むことを重視する人物。実名で選出。"
  constraint: "LLM の内部知識から選出。speaker は実名で記録する。"
```

## Step 3: 議論（persona-council に委譲）

選定された複数人格（実名）が議論する。議論の過程を research/discussions/ に記録する。

## Step 4: 統合

```yaml
integration:
  stop_doing: "棄却する習慣"
  start_doing: "獲得する習慣"
  preserve: "守るべき資産"
  next_hypothesis: "次に検証する仮説"
```

## 出力

- insight-contract（Retro の学びを反映）
- discussion_log（議論の過程）
- sprint-goal-contract の retro_actions を done に更新（該当するもの）

## 参照

- [VISION.md - 場の一覧](../../docs/VISION.md)
- [insight スキル](../skills/insight/SKILL.md)
