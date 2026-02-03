# Sprint Retro S23 - 2026-02-03
# Place: sprint_retro
# Sprint: S23

## 入力

- 前回 Retro: research/discussions/2026-05-20_sprint_retro_S22.md
- insight-contract-S22
- 本スプリントのプロセス:
  - S23 Goal: release-notes の習慣化を継続し、OSS 境界の symlink 検証を実施
  - symlink 検証: ✅ 完了（cursor-rules-investigation-S20.md, oss-boundary-design-S17.md に記録）
  - release-notes: ❌ 未実行（research/releases/S23-release-notes.md が存在しない）
  - S22 Retro で preserve: 「Sprint 終了日に release-notes を実行するルーティン」→ S23 で漏れた

## 分析（Step 1）

```yaml
analysis:
  focus: "release-notes が S23 で漏れた原因。S22 で preserve したルーティンがなぜ実行されなかったか"
  risk: "preserve が「書いただけ」で終わり、実行トリガーが弱い。場の開催を省略する習慣"
  context: "S22 で習慣化初回検証成功。S23 で継続を検証するはずが、release-notes が未実行。symlink 検証は完了"
  mental_model_trap: "preserve に書いたから大丈夫、という安心。ルーティンの実在性を検証していない"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  critical: "プロセスの盲点を探す批判的視点。習慣化の漏れ、構造的問題を指摘する傾向。実名で選出。"
  learning: "学びを抽出し、次のアクションに落とし込むことを重視する人物。実名で選出。"
  constraint: "LLM の内部知識から選出。speaker は実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Esther Derby**（Agile Retrospectives 共著者）
- **Norm Kerth**（Project Retrospectives 著者、Prime Directive 提唱者）

### 議論の過程

| speaker | statement | rationale |
|---------|-----------|-----------|
| Esther Derby | S23 の事実: symlink 検証は完了したが、release-notes が漏れた。S22 Retro で「Sprint 終了日に release-notes を実行するルーティン」を preserve した。しかし preserve は「やるべきことのリスト」であり、「誰がいつトリガーするか」が曖昧だった。 | 習慣化には明確なトリガーが必要。preserve に書くだけでは実行保証にならない |
| Norm Kerth | 批判的視点: workload-monitor や scrum-master の推奨に「Sprint 終了時は release-notes → Review」とある。しかし誰が workload-monitor を実行するか。セッション開始時の「scrum-master でワークロード確認」は推奨であり必須ではない。トリガーが複数あって、どれも弱い。 | トリガーの責任主体が分散していると、誰も実行しない |
| Esther Derby | stop_doing: 「preserve に書いたから完了」と考えること。start_doing: Sprint 終了日の「やること」を契約や DoD に明示する。sprint-goal-contract の verification に「release-notes 実行」が既にあるが、それは成果物の存在確認。実行トリガーは別。 | 成果物の検証と実行トリガーは分けて設計する |
| Norm Kerth | preserve: symlink 検証の成果は価値がある。OSS 境界の方式 A が実現可能と判明した。next_hypothesis: release-notes の実行を「Sprint Retro の入力準備」に紐づける。Retro を開催する前に release-notes を実行する、という依存関係にすれば、Retro がトリガーになる。 | Retro は場として必須。Retro の入力に release-notes を含めれば、Retro 開催がトリガーになる |

### 収束

- release-notes が漏れた原因: トリガーが弱い。preserve は「やるべきこと」の記録であり、実行保証ではない
- 改善案: Retro の入力に「release-notes」を必須にし、Retro 開催が release-notes 実行のトリガーにする
- symlink 検証の成果は preserve

## 統合（Step 4）

```yaml
integration:
  stop_doing:
    - "preserve に書いただけで実行保証と考えること"
  start_doing:
    - "Retro の入力準備として release-notes を必須にする（Retro 開催 = release-notes 実行のトリガー）"
  preserve:
    - "symlink 検証の成果（OSS 境界方式 A が実現可能）"
    - "Sprint 終了日のルーティンに release-notes を含める意図（トリガー設計を改善）"
  next_hypothesis: "Retro の入力に release-notes を必須にすれば、Retro 開催時に release-notes が実行される"
```

## 出力

- insight-contract-S23（Retro の学びを反映）
- sprint-retro 場の入力に「release-notes（該当 Sprint の成果物要約）」を追加するルール更新を検討
