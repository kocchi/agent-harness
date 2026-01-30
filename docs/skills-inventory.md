# スキル棚卸し

agent-harness のスキル一覧。

## エージェント（5）

| エージェント | 目的 | 呼び出し元 |
|-------------|------|-----------|
| `rules-validator` | ルール整合性検査 | P0（セッション開始時）、コミット前 |
| `deep-research` | 深い調査 | 調査タスク時 |
| `systematic-debugger` | 体系的デバッグ | 問題発生時 |
| `guardian-generator` | Guardian 生成 | リポジトリ追加時 |
| `weekly-research` | 週次リサーチ | 週次 |

## 契約系スキル（6）

| スキル | 目的 | 出力 |
|-------|------|------|
| `discovery` | 価値発見、入力→契約変換 | （プロセス） |
| `sprint-goal` | スプリント目標設定 | sprint-goal-contract |
| `slice` | 価値分類（必須/既知/未知） | slice-contract |
| `increment` | 成果物の定義、DoD | increment-contract |
| `insight` | 学びの記録と還元 | insight-contract |
| `propose-change` | 変更提案フロー | （中間生成物での合意） |

## 外部フレームワーク連携（3）

| スキル | 目的 |
|-------|------|
| `superpowers-brainstorming` | 創造的作業の前に実行 |
| `superpowers-writing-plans` | 計画作成 |
| `superpowers-systematic-debugging` | デバッグ |

## ユーティリティ（3）

| スキル | 目的 |
|-------|------|
| `add-repository` | 開発リポジトリを追加 |
| `contribute-upstream` | OSS 貢献ガイド |
| `create-subagent` | サブエージェント作成ガイド |
