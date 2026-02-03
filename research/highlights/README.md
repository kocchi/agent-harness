# Highlights: 安心・面白みの抽出

各イベント（Sprint Planning, Review, Retro）で出た専門家レビューから、「安心」と「面白み」を抽出して記録する。

## 抽出基準

### 安心（Good News ではない）

| 観点 | 抽出の目安 |
|------|------------|
| **自律的・建設的議論** | 専門家同士が批判・反論し合い、妥協で丸めずに収束している |
| **価値提供に迎えている** | 誰が何を得たか、次に何が効くかが明確に議論されている |
| **価値検証でスライス** | 検証可能な単位で切れている、必須/既知/未知が区別されている |
| **構造的対策の効き** | 発見した問題が仕組みで防がれている証拠 |

### 面白み

| 観点 | 抽出の目安 |
|------|------------|
| **意外な洞察** | 想定外の盲点指摘、発見的学習 |
| **人格の一貫性** | 実名の専門家が「その人らしく」議論している |
| **設計の本質** | トリガー構造、依存関係、境界の議論 |
| **学びの伝播** | 誰が言ったかがわかれば参照できる知見 |

## フォーマット

各 highlight は YAML フロントマター + 本文。

```yaml
---
event: sprint_review  # planning | review | retro
sprint: S7
date: 2026-01-30
type: reassurance | interest  # 安心 | 面白み
source: research/discussions/2026-01-30_sprint_review.md
personas: [Lisa Crispin, Don Reinertsen]
---
```

## 抽出済み一覧

| 日付 | イベント | Sprint | 種別 | ファイル |
|------|----------|--------|------|----------|
| 2026-01-30 | Review | S7 | 安心 | [2026-01-30_sprint_review_S7.md](2026-01-30_sprint_review_S7.md) |
| 2026-01-30 | Retro | S7 | 面白み | [2026-01-30_sprint_retro_2.md](2026-01-30_sprint_retro_2.md) |
| 2026-02-14 | Review | S8 | 安心 | [2026-02-14_sprint_review_S8.md](2026-02-14_sprint_review_S8.md) |
| 2026-02-14 | Retro | S8 | 安心 | [2026-02-14_sprint_retro_S8.md](2026-02-14_sprint_retro_S8.md) |
| 2026-04-04 | Planning | S16 | 安心 | [2026-04-04_sprint_planning_S16.md](2026-04-04_sprint_planning_S16.md) |
| 2026-04-10 | Planning | S17 | 面白み | [2026-04-10_sprint_planning_S17.md](2026-04-10_sprint_planning_S17.md) |

## 参照

- [what-good-managers-want.md](../references/what-good-managers-want.md) - 優れた上司が求める報告のリサーチ
