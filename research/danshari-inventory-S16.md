# 断捨離棚卸し S16
# 2026-04-04

## 参照元の定義

- **core-rules**: 必須。サブエージェントルーティングで明示
- **implementation-rules**: 場の開催、Plan フェーズ
- **契約フロー**: sprint-goal, slice, insight, workload-monitor
- **Q2 スコープ**: add-repository, contribute-upstream

## エージェント棚卸し

| エージェント | core-rules | その他参照 | 判断 |
|-------------|------------|-----------|------|
| rules-validator | P0, コミット前 | implementation-rules | **必須** |
| scrum-master | ワークロード確認時 | workload-monitor | **必須** |
| deep-research | 調査タスク時 | - | **必須** |
| systematic-debugger | 問題発生時 | - | **必須** |
| persona-council | 場 | implementation-rules, 場テンプレート | **必須** |
| persona-vessel | - | roles (pdm, tech-lead), philosophy | **保留**（設計上の概念） |
| guardian-generator | - | add-repository | **保留**（Q2 で add-repository とセット） |
| weekly-research | - | skills-inventory のみ | **削除**（呼び出し元なし） |

## スキル棚卸し

| スキル | 参照元 | 判断 |
|--------|--------|------|
| sprint-goal | 場、契約フロー | **必須** |
| slice | 契約フロー | **必須** |
| insight | 場、契約フロー | **必須** |
| workload-monitor | scrum-master, core-rules | **必須** |
| metrics | Product Goal, Q1 | **必須** |
| discovery | sprint-goal 前提、slice 前提 | **必須** |
| increment | 契約フロー、propose-change | **必須** |
| propose-change | 変更提案 | **必須** |
| add-repository | Q2 スコープ | **必須** |
| contribute-upstream | Q2 スコープ | **必須** |
| create-subagent | skills-inventory のみ | **削除**（主要フローにない） |
| superpowers-brainstorming | 自己参照、存在しない superpowers: 参照 | **削除**（参照壊れ） |
| superpowers-writing-plans | 存在しない executing-plans 参照 | **削除**（参照壊れ） |
| superpowers-systematic-debugging | 存在しない test-driven-development 参照 | **削除**（参照壊れ） |

## ルール棚卸し

| ルール | 判断 |
|--------|------|
| constitution | **必須** |
| core-rules | **必須** |
| implementation-rules | **必須** |

## 削除実行

P3（代替経路が証明されてから削除）に従い、以下を削除:
- weekly-research: 呼び出し元なし。deep-research が調査を担当
- create-subagent: 主要フローにない。必要時は contribute-upstream で OSS 貢献
- superpowers-*: 参照が壊れている。契約系（slice, increment, propose-change）で代替

**superpowers ノウハウの保持**: 外部 OSS 由来の洗練されたノウハウのため、`research/references/superpowers-knowhow.md` に抽出して保持
