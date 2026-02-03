# スキル棚卸し

agent-harness の構成一覧。

## エージェント（7）

| エージェント | 目的 | 呼び出し元 |
|-------------|------|-----------|
| `rules-validator` | ルール整合性検査 | P0（セッション開始時）、コミット前 |
| `scrum-master` | ワークロード監視、障害検出、プロセス健全性 | セッション開始時（推奨）、Daily Scrum 相当、進捗確認時 |
| `deep-research` | 深い調査 | 調査タスク時 |
| `systematic-debugger` | 体系的デバッグ | 問題発生時 |
| `guardian-generator` | リポジトリ固有 Agent 生成 | リポジトリ追加時（add-repository） |
| `persona-vessel` | 人格の器（単一憑依） | 非同期・単一視点が必要なとき |
| `persona-council` | 複数人格の議論 | 場（Sprint Planning, Review, Retro） |

## Role（2）

| Role | 責務 | 使用場面 |
|------|------|----------|
| `pdm` | Why/What を定義 | Discovery, Sprint Planning |
| `tech-lead` | How を定義 | Sprint Planning, Slice 設計 |

## Personas

- **文脈から導出**: persona-vessel が context から選出（事前定義リストなし）
- **カスタム**: `.cursor/personas/custom/` にユーザーが定義

## 契約系スキル（6）

| スキル | 目的 | 出力 |
|-------|------|------|
| `discovery` | 価値発見、入力→契約変換 | （プロセス） |
| `sprint-goal` | スプリント目標設定 | sprint-goal-contract |
| `slice` | 価値分類（必須/既知/未知） | slice-contract |
| `increment` | 成果物の定義、DoD | increment-contract |
| `insight` | 学びの記録と還元 | insight-contract |
| `propose-change` | 変更提案フロー | （中間生成物での合意） |

## プロセス監視（1）

| スキル | 目的 | 出力 |
|-------|------|------|
| `workload-monitor` | ワークロード監視、障害検出、プロセス健全性 | workload-report |

## ユーティリティ（3）

| スキル | 目的 |
|-------|------|
| `add-repository` | 開発リポジトリを追加 |
| `contribute-upstream` | OSS 貢献ガイド |
| `follow-upstream` | upstream の変更を取り込む |

## メトリクス（1）

| スキル | 目的 |
|-------|------|
| `metrics` | 契約活用率・手戻り率・自律度の集計 |
