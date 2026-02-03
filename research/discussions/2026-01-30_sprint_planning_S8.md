# Sprint Planning 議論ログ
# Place: sprint_planning
# Sprint: S8
# 2026-01-30

## 入力

- materials: contracts/product-goal-contract.yaml, contracts/sprint-goal-contract-S8.yaml（ドラフト）, contracts/insight-contract-S7.yaml
- Product Goal: 最終判断以外を AI チームに委譲できる
- ドラフト Sprint Goal: S7 Retro の学びをスキル・場テンプレートに反映し、Review/Retro のプロセスを確実にする

## 分析（Step 1）

```yaml
analysis:
  focus: "プロセス改善の実装"
  risk: "スコープ creep、場をスキップする習慣の再発"
  context: "S7 完了、insight-contract-S7 に next_action が5件"
  mental_model_trap: "効率を優先して場をスキップする"
```

## 召喚条件（Step 2）

VISION の Sprint Planning チェックリストに基づく:

```yaml
summon_criteria:
  pdm: "価値（何が得られるか）を重視する人物。検証する価値が明確か問う。"
  tech_lead: "実現可能性を問う人物。スコープが現実的か、技術的に可能か判断する。"
  qa: "検証方法が測れるかを問う人物。success_criteria の曖昧さを指摘する。"
```

## 議論（Step 3）

### 人格選定

召喚条件に基づき、LLM の内部知識から以下を選出:

- **Marty Cagan**（Inspired 著者）: 価値重視。何が得られるか、なぜやるかを問う。PdM 視点。
- **Kent Beck**（TDD 創始者）: 実現可能性。シンプルに届ける、小さく始める。TechLead 視点。
- **Lisa Crispin**（Agile Testing 著者）: 検証可能性。測れるか、成功基準は明確か。QA 視点。

### 議論の過程

| speaker | statement | rationale | response_to |
|---------|-----------|-----------|-------------|
| Marty Cagan | ドラフトの Sprint Goal は「S7 Retro の学びを反映」だ。価値としては、次回の Review/Retro で場が確実に開催され、retro_actions の更新漏れが防げる。Product Goal「最終判断以外を委譲」に貢献するか。委譲の質が上がる。場をスキップする習慣を断つことは、AI チームの自律性を高める。 | 価値の明確化。Product Goal への貢献。 | — |
| Kent Beck | 5つの slice は全て small か medium。insight スキルへの2行追加、場テンプレート2つの作成、workload-monitor への1行追加。技術的に難しくない。1週間で届く。ただし、S5「Sprint Planning でチェックリスト参照の検証」は、今まさにこの場で検証している。メタだ。 | 実現可能性。スコープの現実性。 | Marty Cagan |
| Lisa Crispin | S5 について。この Sprint Planning 自体が、VISION のチェックリスト（PdM/TechLead/QA の3問い）を参照しているか検証する、という意味か。今、私たちが議論していることが、その検証になる。成功基準は「チェックリストが参照されたか」。この議論ログに召喚条件としてチェックリストの観点が含まれていれば、検証完了。 | 検証可能性。S5 の success_criteria の明確化。 | Kent Beck |
| Marty Cagan | スコープに「他エージェント・スキルの必須事項棚卸し」が out_of_scope になっている。S7-retro-4 の学びだ。今回は含めない判断でよいか。含めるとスコープが膨らむ。まずは insight と場テンプレートを確実にし、棚卸しは次 Sprint で検討する。 | スコープの優先順位。 | — |
| Kent Beck | 同意。YAGNI。必須事項の棚卸しは、今回の変更で問題が起きたらやる。今は insight と workload-monitor の修正に集中する。 | シンプルに届ける。 | Marty Cagan |
| Lisa Crispin | success_criteria を確認する。「insight スキルに〇〇が含まれる」は、ファイルを grep すれば検証できる。「.cursor/places/sprint-review.md が存在する」も確認可能。「Sprint Planning でチェックリスト参照が行われるか」は、この議論ログの召喚条件に PdM/TechLead/QA の観点が含まれているかで検証できる。全て測れる。 | 検証可能性の確認。 | — |
| Marty Cagan | ドラフトの Sprint Goal、スコープ、success_criteria で合意してよいか。 | 合意の確認。 | Kent Beck, Lisa Crispin |
| Kent Beck | 合意する。 | — | Marty Cagan |
| Lisa Crispin | 合意する。 | — | Marty Cagan |

### 収束

Sprint Goal、スコープ、success_criteria で合意。ドラフトを承認。S5 の検証は「この Sprint Planning の議論でチェックリスト（PdM/TechLead/QA）が召喚条件に含まれていること」で満たす。

## 統合（Step 4）

```yaml
integration:
  stop_doing: []
  start_doing:
    - "S8 の slice を順に実装する"
  preserve:
    - "S7 の学びをスコープに含める方針"
    - "VISION のチェックリストを Sprint Planning の召喚条件に反映する"
  next_hypothesis: "場テンプレート（sprint-review, sprint-retro）があると、次回の Review/Retro で persona-council の委譲が確実になる"
```

## 出力

- Sprint Goal: 承認（変更なし）
- スコープ: 承認（変更なし）
- success_criteria: 承認。S5 の検証方法を明確化（この議論でチェックリストが参照されたこと）
- discussion_log: 本ファイル
