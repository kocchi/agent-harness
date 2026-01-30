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
| `superpowers-brainstorming` | 創造的作業の前に実行 | ✅ discovery と補完（下記参照） |
| `superpowers-writing-plans` | 計画作成 | ✅ sprint-goal と補完（下記参照） |
| `superpowers-systematic-debugging` | デバッグ | ✅ 独立 |

### 契約系と superpowers 系の使い分け

| 質問 | 使うスキル | 理由 |
|------|-----------|------|
| 「何を作るべきか」がわからない | **discovery** | 価値定義（Why） |
| 「どう作るか」を探索したい | **brainstorming** | 設計探索（How） |
| 「このスプリントの目標は」 | **sprint-goal** | 目標設定（Why） |
| 「実装計画を作りたい」 | **writing-plans** | 実装計画（How） |

**原則**: 契約系 = Why（価値・目標）、superpowers 系 = How（設計・計画）

### 契約体系と task-orchestrator の使い分け

| フェーズ | 使うスキル | 理由 |
|---------|-----------|------|
| 合意形成 | **契約系スキル** | 何を作るか（Why）を明文化 |
| タスク実行 | **task-orchestrator** | どう作るか（How）を自動化 |

**フロー例**:
```
Sprint Planning → sprint-goal スキル → sprint-goal-contract
        ↓
価値分類 → slice スキル → slice-contract
        ↓
タスク実行 → task-orchestrator → executor/watcher
        ↓
完了記録 → increment スキル → increment-contract
        ↓
学び記録 → insight スキル → insight-contract
```

**原則**: 契約体系 = 合意レイヤー、task-orchestrator = 実行レイヤー

### 契約体系の「場」と session-selector の違い

| レイヤー | 概念 | 例 |
|---------|------|-----|
| ビジネスプロセス | 契約を作る「場」 | Sprint Planning, Discovery, Review |
| タスク実行 | セッション種別 | planning, design, implement, retrospective |

**使い分け**:
- 「Sprint Planning を開催」→ **sprint-goal スキル**（契約を作る）
- 「この設計をレビューして」→ **session-selector** → design session（タスク実行）

### insight と improvement-lifecycle の使い分け

| 状況 | 使うスキル | 理由 |
|------|-----------|------|
| 「今わかった、すぐ反映したい」 | **insight** | 即時還元（1回で完結） |
| 「まず試してみたい」 | **improvement-lifecycle** | 段階的昇格（try → keep → rule） |

**フロー例**:
```
Retro で改善提案 → improvement-lifecycle で「try」登録
        ↓
試行期間（数回のスプリント）
        ↓
効果あり → keep → rule 昇格（ルール化）
効果なし → stop（廃止）
```

## 既存スキル（整理が必要）

| スキル | 目的 | 整合性 | 対応案 |
|-------|------|--------|--------|
| `task-classifier` | タスク分類、人格注入判定 | ✅ VISION と整合 | 維持 |
| `task-orchestrator` | タスクのルーティング | ✅ 契約体系と補完（下記参照） | 維持 |
| `session-init` | セッション開始時の初期化 | ✅ P0 と整合 | 維持 |
| `session-selector` | セッション種別の選択 | ✅ 契約体系と補完（下記参照） | 維持 |
| `propose-change` | 変更提案フロー | ✅ increment と補完（実装フロー） | 維持 |
| ~~`log-learning`~~ | 学習の記録 | ✅ insight に統合済み | S3 で削除 |
| `ledger-record` | 台帳への記録 | ✅ 独立（低レベル） | 維持 |
| `ledger-query` | 台帳の検索 | ✅ 独立（低レベル） | 維持 |
| `improvement-lifecycle` | 改善サイクル | ✅ insight と補完（下記参照） | 維持 |
| `debt-monitor` | 技術負債の監視 | ✅ 独立 | 維持 |
| `create-subagent` | サブエージェント作成 | ✅ 独立 | 維持 |
| `coverage-check` | カバレッジ確認 | ✅ 独立 | 維持 |
| `contribute-upstream` | OSS 貢献ガイド | ✅ 独立 | 維持 |
| `changelog-update` | 変更履歴の更新 | ✅ 独立 | 維持 |
| `add-repository` | リポジトリ追加 | ✅ 独立 | 維持 |

## 対応優先度

### 高（重複解消）

1. ~~`log-learning` → `insight` に統合~~ ✅ S3 で完了
2. ~~`propose-change` → `increment` との関係を明確化~~ ✅ S3 で完了（補完関係）

### 中（整合性確認）

3. ~~`superpowers-brainstorming` → `discovery` との使い分けを明確化~~ ✅ S3 で完了
4. ~~`superpowers-writing-plans` → `sprint-goal` との使い分けを明確化~~ ✅ S3 で完了
5. ~~`task-orchestrator` → 契約体系との統合検討~~ ✅ S3 で完了（補完関係）

### 低（現状維持）

- 独立性の高いスキルは維持

## 未作成スキル

| スキル | 目的 | 優先度 | 備考 |
|-------|------|--------|------|
| ~~`okr`~~ | 四半期目標設定 | - | product-goal-contract で代替 |
| ~~`release`~~ | リリース管理 | - | 現状不要 |
| ~~`external-spec`~~ | 外部仕様書管理 | - | 現状不要 |
| ~~`product-goal`~~ | プロダクト目標設定 | - | discovery スキルで代替 |

**結論**: 現状、新規スキルの追加は不要。既存スキルで十分カバーされている。
