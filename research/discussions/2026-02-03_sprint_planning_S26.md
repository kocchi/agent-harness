# Sprint Planning S26 - 2026-02-03
# Place: sprint_planning
# Sprint: S26

## 入力

- product-goal-contract-Q2
- insight-contract-S25
- S25 の結果（方式 A 設計ドラフト完了、release-notes 定着、oss-boundary-design に具体手順追記）

## 分析（Step 1）

```yaml
analysis:
  focus: "Product Goal Q2 の次の焦点。方式 A 設計完了後の選択肢"
  risk: "設計で止まり実装に進まない / 2 人目がいないと H6 検証できない"
  context: "S25 で方式 A の設計ドラフト完了。コンフリクト多発時まで実装は保留。H6（オンボーディング）は 2 人目待ち"
  mental_model_trap: "設計が終わったら次の設計を探す（実装・検証を先送り）"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: |
    Product Goal と Sprint のバランスを重視し、
    mental_model_trap（設計で止まる）を書き換える視点を持つ人物。
    断定せず仮説として提示する傾向。
  coach_b: |
    risk（実装に進まない、検証できない）への対策を重視し、
    Coach A と補完的な視点を持つ人物。
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Mary Poppendieck**（Lean Software Development 著者。価値の流れ、完了の定義を重視）
- **Kent Beck**（TDD 創始者。小さく検証、実装の具体化を重視）

### 議論の過程

| speaker | statement | rationale |
|---------|-----------|-----------|
| Mary Poppendieck | S26 の選択肢を整理する。方式 A の設計は完了した。Product Goal の success_metrics を見ると、「OSS の境界」は「実装はコンフリクト多発時に検討」とある。今すぐ実装する必然性はない。H6（オンボーディング）は 2 人目待ち。では S26 で何をするか。 | 設計完了後の「次の一手」を明確にしないと、設計で止まる罠に陥る |
| Kent Beck | 実装を「コンフリクト多発時まで待つ」は妥当。ただし、その「多発」をどう検知するかが曖昧。follow-upstream を実行した回数・コンフリクト件数を記録していれば、閾値が決められる。S26 で「コンフリクト検知の基盤」を置くのはどうか。 | 測定できないものは改善できない。小さな検証を積む |
| Mary Poppendieck | それは価値がある。ただ、follow-upstream の実行頻度が月 1 回程度なら、1 Sprint で「多発」かどうかは判断しづらい。別の道として、方式 A の**ドッグフーディング**がある。agent-harness 自体を「チームリポジトリ」と見立て、vendor 配置を試す。実装ではなく、セットアップ手順の検証。 | 本番前に自分たちで試す。設計の実現可能性を検証 |
| Kent Beck | ドッグフーディングは良い。oss-boundary-design に手順は書いたが、実際にやってみないと抜け漏れがわからない。S26 の Goal を「方式 A のセットアップを agent-harness 上でドッグフーディングし、手順の抜け漏れを洗い出す」とする。 | 書いた手順を実行して検証。小さな実験 |
| Mary Poppendieck | 収束。ただし、agent-harness は既に .cursor/ を持っている。vendor に自分自身を submodule で入れるのは循環する。代替: 別リポジトリ（例: 歩活）でドッグフーディングするか、agent-harness を「Fork 先」と見立てて、upstream を別の OSS に変えるか。 | 循環参照を避ける。現実的な検証経路 |
| Kent Beck | agent-harness の Fork を作り、Fork 側で vendor に upstream（本家）を submodule で入れる。これなら循環しない。Fork が「チームリポジトリ」、本家が「OSS」の役割。S26 で Fork を作成し、方式 A の手順を 1 から実行して検証する。 | 実際のユースケースに近い。手順の検証になる |

### 収束

**S26 Goal**: 方式 A（vendor 配置）のセットアップ手順をドッグフーディングで検証する。agent-harness の Fork を作成し、Fork 側で vendor に upstream を submodule で配置、symlink で参照する構成を試す。手順の抜け漏れ・不明点を oss-boundary-design に追記する。

## 統合（Step 4）

```yaml
integration:
  stop_doing: "設計で止まり、実装・検証を先送りにする"
  start_doing: "設計した手順をドッグフーディングで検証する"
  preserve: "release-notes の流れ、進捗ベースの Sprint 運用"
  next_hypothesis: "方式 A の手順を Fork で実行すれば、抜け漏れが洗い出され、実装判断の材料になる"
```

## 出力

- sprint-goal-contract-S26
- slice-contract-S26（作成予定）
