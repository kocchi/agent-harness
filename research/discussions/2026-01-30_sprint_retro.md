# Sprint Retro 議論ログ
# Place: sprint_retro
# Sprint: S7
# 2026-01-30

## 入力

- materials: 上記 Sprint Review の議論、contracts/insight-contract-S7.yaml（初版）、workload-monitor の推奨
- 背景: S7 で Review/Retro を「場」として開催せず、メインエージェントが直接 insight を作成した。ユーザー指摘でやり直し。

## 分析（Step 1）

```yaml
analysis:
  focus: "プロセス改善、習慣化の漏れ"
  risk: "場の開催を省略する習慣がつく、ルールと実践の乖離"
  context: "persona-council が存在するが、Review/Retro で呼び出されなかった"
  mental_model_trap: "効率を優先して場をスキップする"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: "プロセスの盲点を探す批判的視点。習慣化の漏れ、構造的問題を指摘する傾向。"
  coach_b: "学びを抽出し、次のアクションに落とし込むことを重視する人物。"
```

## 議論（Step 3）

### 人格選定

召喚条件に基づき、LLM の内部知識から以下を選出:

- **Esther Derby**（Agile Retrospectives 共著者）: プロセスの盲点を探す批判的視点。習慣化の漏れ、構造的問題を指摘する傾向。
- **Norm Kerth**（Project Retrospectives 著者、Retro の父）: 学びを抽出し、次のアクションに落とし込むことを重視。

### 議論の過程

| speaker | statement | rationale | response_to |
|---------|-----------|-----------|-------------|
| Esther Derby | 今回、Review/Retro で persona-council を呼び出さなかった。core-rules には「場では persona-council を呼び出す」とある。ルールがあるのに守られない。なぜか。 | ルールと実践の乖離。構造的問題の指摘。 | — |
| Norm Kerth | 推測すると、(1) persona-council の呼び出し方法が明示されていない、(2) sprint-review / sprint-retro の場のテンプレートが未定義（sprint-planning のみ存在）、(3) 「推奨アクションを実施」という依頼で、場の開催がトリガーにならなかった。 | 原因分析。テンプレート不足、トリガー不足。 | Esther Derby |
| Esther Derby | テンプレートがなくても、persona-council の入力形式（place, materials, summon_criteria）は定義されている。メインエージェントが「場を開催する」と判断し、persona-council に委譲する責務がある。テンプレートがないことを、委譲しない言い訳にしてはいけない。 | 責務の明確化。テンプレートは便利だが、なくても委譲は可能。 | Norm Kerth |
| Norm Kerth | 同意。学びとして抽出すべきは: 「場の開催」を明示的にトリガーにする必要がある。例えば、insight スキルの実行手順に「Review/Retro は persona-council に委譲してから insight を作成する」を追加する。あるいは、sprint-review / sprint-retro の場テンプレートを sprint-planning と同様に作成する。 | 学びの具体化。次のアクションへの落とし込み。 | Esther Derby |
| Esther Derby | もう一つの盲点: retro_actions の status 更新。workload-monitor が指摘するまで気づかなかった。Review/Retro の出力に「sprint-goal-contract の retro_actions を done に更新」が含まれていない。これはプロセスの穴。 | 構造的盲点。出力定義の不足。 | — |
| Norm Kerth | その穴を埋めるには、insight スキルの実行手順に「Review/Retro 完了時に sprint-goal-contract の retro_actions を done に更新する」を追加する。workload-monitor の推奨にも「retro_actions の status を Review 時に更新」を入れると、二重のチェックになる。 | 対策の具体化。スキル更新 + workload-monitor の推奨追加。 | Esther Derby |
| Esther Derby | scrum-master / workload-monitor は今回の「やり直し」の契機になった。retro_actions の更新漏れを検出した。プロセス監視の価値は実証された。 | ポジティブな学び。workload-monitor の効果。 | Norm Kerth |
| Norm Kerth | まとめると、Retro の学びは三つ: (1) 場の開催をトリガー化する（insight スキル or 場テンプレート）、(2) retro_actions 更新をプロセスに組み込む、(3) workload-monitor の価値を継続検証する。 | 学びの統合。 | Esther Derby |

### 収束

三つの学びで合意。次の Sprint で (1)(2) を実装する。

## 統合（Step 4）

```yaml
integration:
  stop_doing:
    - "Review/Retro を場として開催せずに insight を直接作成する"
  start_doing:
    - "Review/Retro 実行時に persona-council に委譲する"
    - "insight スキルに「retro_actions を done に更新」を手順として追加する"
    - "sprint-review / sprint-retro の場テンプレートを作成する（sprint-planning を参考に）"
  preserve:
    - "workload-monitor による retro_actions 照合"
    - "ユーザーが「場は開催されたか」を問う習慣"
  next_hypothesis: "場テンプレートがあると、persona-council の委譲が確実になる"
```

## 出力

- Retro 結論: 場の開催漏れ、retro_actions 更新のプロセス穴を特定。対策を次のアクションに落とし込んだ。
- insight-contract への反映: 上記の学びを構造化
