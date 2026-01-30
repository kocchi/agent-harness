# スキル棚卸し

agent-harness のスキル一覧と整理状況。

## 契約系スキル（新規作成）

| スキル | 目的 | 出力 | 状態 |
|-------|------|------|------|
| `discovery` | 価値発見、入力→契約変換 | discovery-contract | ✅ S1 で作成 |
| `sprint-goal` | スプリント目標設定 | sprint-goal-contract | ✅ S1 で作成 |
| `insight` | 学びの記録と還元 | insight-contract | ✅ S1 で作成、S2 で改善 |
| `increment` | 成果物の定義、DoD | increment-contract | ✅ S2 で作成 |
| `slice` | 価値分類（必須/既知/未知） | slice-contract | ✅ S2 で作成 |

## 外部フレームワーク連携（superpowers）

| スキル | 目的 | 整合性 |
|-------|------|--------|
| `superpowers-brainstorming` | 創造的作業の前に実行 | △ discovery と重複可能性 |
| `superpowers-writing-plans` | 計画作成 | △ sprint-goal と重複可能性 |
| `superpowers-systematic-debugging` | デバッグ | ✅ 独立 |

## 既存スキル（整理が必要）

| スキル | 目的 | 整合性 | 対応案 |
|-------|------|--------|--------|
| `task-classifier` | タスク分類、人格注入判定 | ✅ VISION と整合 | 維持 |
| `task-orchestrator` | タスクのルーティング | △ 契約体系と統合検討 | 要検討 |
| `session-init` | セッション開始時の初期化 | ✅ P0 と整合 | 維持 |
| `session-selector` | セッション種別の選択 | △ 契約体系と統合検討 | 要検討 |
| `propose-change` | 変更提案フロー | △ increment と重複可能性 | 統合検討 |
| ~~`log-learning`~~ | 学習の記録 | ✅ insight に統合済み | S3 で削除 |
| `ledger-record` | 台帳への記録 | ✅ 独立（低レベル） | 維持 |
| `ledger-query` | 台帳の検索 | ✅ 独立（低レベル） | 維持 |
| `improvement-lifecycle` | 改善サイクル | △ insight と重複可能性 | 統合検討 |
| `debt-monitor` | 技術負債の監視 | ✅ 独立 | 維持 |
| `create-subagent` | サブエージェント作成 | ✅ 独立 | 維持 |
| `coverage-check` | カバレッジ確認 | ✅ 独立 | 維持 |
| `contribute-upstream` | OSS 貢献ガイド | ✅ 独立 | 維持 |
| `changelog-update` | 変更履歴の更新 | ✅ 独立 | 維持 |
| `add-repository` | リポジトリ追加 | ✅ 独立 | 維持 |

## 対応優先度

### 高（重複解消）

1. ~~`log-learning` → `insight` に統合~~ ✅ S3 で完了
2. `propose-change` → `increment` との関係を明確化

### 中（整合性確認）

3. `superpowers-brainstorming` → `discovery` との使い分けを明確化
4. `superpowers-writing-plans` → `sprint-goal` との使い分けを明確化
5. `task-orchestrator` → 契約体系との統合検討

### 低（現状維持）

- 独立性の高いスキルは維持

## 未作成スキル

| スキル | 目的 | 優先度 |
|-------|------|--------|
| `okr` | 四半期目標設定 | 低 |
| `release` | リリース管理 | 中 |
| `external-spec` | 外部仕様書管理 | 低 |
| `product-goal` | プロダクト目標設定 | 中 |
