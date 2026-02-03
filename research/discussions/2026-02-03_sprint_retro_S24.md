# Sprint Retro S24 - 2026-02-03
# Place: sprint_retro
# Sprint: S24

## 入力

- 前回 Retro: research/discussions/2026-02-03_sprint_retro_S23.md
- insight-contract-S23
- 本スプリントのプロセス: 進捗ベースで Sprint を進めた。release-notes を Review の前に実行。Retro の前に release-notes が揃った

## 分析（Step 1）

```yaml
analysis:
  focus: "進捗ベースの Sprint 運用、Retro 前の release-notes 流れ"
  risk: "日付固定の習慣が残る"
  context: "S24 で release-notes → Review → Retro の流れを進捗ベースで実行。S23 の仮説（Retro の入力に release-notes）を検証"
  mental_model_trap: "Sprint は週単位で区切るもの"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  critical: "プロセスの盲点を探す批判的視点。"
  learning: "学びを次のアクションに落とし込むことを重視する人物。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Esther Derby**（Agile Retrospectives 共著者）
- **Norm Kerth**（Project Retrospectives 著者）

### 議論の過程

| speaker | statement |
|---------|------------|
| Esther Derby | S24 のプロセス: 進捗ベースで Sprint を進めた。release-notes を Review の前に実行。Retro の前に release-notes が揃った。S23 で漏れた原因（トリガーが弱い）への対策が S24 で検証された。 |
| Norm Kerth | preserve: 進捗ベースの Sprint 運用。Retro の入力に release-notes を必須にすれば、Retro 開催がトリガーになる。S25 で sprint-retro の入力に追加する。start_doing: Sprint 終了日の「やること」を日付で固定しない。 |
| Esther Derby | 収束。S24 Retro の学び: 進捗ベース + Retro の入力に release-notes を追加する、で習慣化の確実性が上がる。 |

### 収束

S24 の検証成功。S25 で sprint-retro の入力に release-notes を必須追加する。

## 統合（Step 4）

```yaml
integration:
  stop_doing: []
  start_doing:
    - "S25 で sprint-retro の入力に release-notes を必須追加する"
  preserve:
    - "進捗ベースの Sprint 運用"
    - "Retro 開催前に release-notes を実行する流れ"
  next_hypothesis: "sprint-retro の入力に release-notes を必須にすれば、習慣化が確実になる"
```

## 出力

- insight-contract-S24（Retro の学びを反映）
- sprint-retro 場の入力に release-notes を追加（S25 で反映）
