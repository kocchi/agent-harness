# Sprint Planning S24 - 2026-02-03
# Place: sprint_planning
# Sprint: S24

## 入力

- product-goal-contract-Q2
- insight-contract-S23（symlink 検証完了、release-notes トリガー改善の仮説）
- プロダクトゴール達成までの道筋の説明（本セッションで実施）

## 分析（Step 1）

```yaml
analysis:
  focus: "Product Goal の道筋の明確化、S24 の焦点"
  risk: "道筋が不明確なまま Sprint を回し続ける。優先順位が決まらない"
  context: "S23 で symlink 検証完了。方式 A が実現可能。release-notes は S23 で漏れた。プロダクトゴール達成までの道筋が不明確"
  mental_model_trap: "「やれること」を優先して「やるべきこと」が後回しになる"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  coach_a: "Product Goal と Sprint のバランスを重視。道筋の明確化を促す。"
  coach_b: "優先順位とブロッカー解消を重視。流れを止めない判断。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Mary Poppendieck**（Lean Software Development 著者）
- **Don Reinertsen**（The Principles of Product Development Flow 著者）

### 議論の過程

| speaker | statement |
|---------|------------|
| Mary Poppendieck | S24 の焦点: プロダクトゴールの道筋が不明確だった。まず「何をいつやるか」を決める。方式 A の具体化と 2 人目導入、どちらを先にするか。 |
| Don Reinertsen | 優先順位の基準: ブロッカーを解消するものを先に。方式 A は「コンフリクト多発時に検討」と oss-boundary-design にあった。現状、コンフリクトは観測されていない。2 人目導入は H6 の検証に直結。 |
| Mary Poppendieck | 同意。方式 A の実装は「必要になったら」でよい。S24 は release-notes のトリガー改善（S23 Retro の仮説）と、Product Goal の iteration 更新。道筋を契約に明文化する。 |
| Don Reinertsen | success_criteria: (1) S24-release-notes.md が存在する（習慣化）、(2) Retro 開催前に release-notes を実行する流れを検証（ルール変更は S25 で）、(3) product-goal-contract-Q2 の iteration を現状に更新。 |
| Mary Poppendieck | 収束。S24 Goal: release-notes のトリガー改善を検証し、Product Goal の道筋を契約に反映する。 |

### 収束

S24 Goal: release-notes のトリガー改善を検証し、Product Goal の道筋を契約に反映する。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "Retro 開催前に release-notes を実行する流れで、S24 で release-notes が漏れなければ、S25 で sprint-retro の入力に追加する"
```

## 出力

- sprint-goal-contract-S24
- slice-contract-S24（必要に応じて）
- product-goal-contract-Q2 の iteration 更新
