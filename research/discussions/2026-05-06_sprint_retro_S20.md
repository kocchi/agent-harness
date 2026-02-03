# Sprint Retro S20 - 2026-05-06
# Place: sprint_retro
# Sprint: S20

## 入力

- insight-contract-S19
- 本スプリント: S20 で Cursor 仕様調査を実施。sprint-review の入力にリリースノートがないことが指摘された
- **追加トピック**: リリースノートがないと Review が厳しい。次のスプリントに含める

## 分析（Step 1）

```yaml
analysis:
  focus: "Review の入力の質、プロセス改善"
  risk: "契約と成果物の羅列だけでは persona-council が何をレビューしているか曖昧"
  context: "sprint-review の入力は sprint-goal, slice, verification-report, 成果物。リリースノート相当の「何を届けたか」まとめがない"
  mental_model_trap: "契約があれば十分と早合点する"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  critical: "プロセスの盲点を探す批判的視点。習慣化の漏れ、構造的問題を指摘する傾向。"
  learning: "学びを抽出し、次のアクションに落とし込むことを重視する人物。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Esther Derby**（Agile Retrospectives 共著者）: プロセスの盲点を探す批判的視点
- **Diana Larsen**（Agile Retrospectives 著者）: 学びを次のアクションに落とし込むことを重視

### 議論の過程

| speaker | statement |
|---------|-----------|
| Esther Derby | S20 の Retro で追加トピック: Review の入力にリリースノートがない。persona-council は契約と成果物のパスから「何を届けたか」を自分で組み立てている。レビュー対象が曖昧だと、検証の質が落ちる。 |
| Diana Larsen | リリースノートは「この Sprint で何を届けたか」「誰に何の価値が届いたか」「検証結果」を1ページにまとめる。Review の入力に含めれば、persona-council が success_criteria との照合をしやすくなる。 |
| Esther Derby | 次のスプリント（S21）に含める。slice として「Sprint 終了時にリリースノートを生成する仕組み」を追加。sprint-review の入力に release_notes を追加する。 |
| Diana Larsen | start_doing: Sprint 終了時にリリースノートを生成し、Review の入力に含める。形式は research/releases/S{NN}-release-notes.md。preserve: 契約ベースの検証フローは維持。 |

### 収束

S21 にリリースノート生成を slice として含める。sprint-review 場の入力に release_notes を追加する。

## 統合（Step 4）

```yaml
integration:
  stop_doing: []
  start_doing:
    - "Sprint 終了時にリリースノートを生成し、Review の入力に含める"
  preserve:
    - "契約ベースの検証フロー"
  next_hypothesis: "リリースノートがあると、persona-council のレビュー品質が上がる"
```

## 出力

- insight-contract-S20（Retro の学びを反映）
- S21 Planning でリリースノートをスコープに含める
