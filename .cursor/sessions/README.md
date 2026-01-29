# セッション（場）ガイド

## これは何？

AIエージェントがタスクを実行する際の「場」の定義です。
スクラムのイベント（Sprint Planning, Retrospective など）を型として、
AIが一貫したプロセスでタスクを進められるようにします。

## なぜ嬉しい？

| 課題 | 場を使わない場合 | 場を使う場合 |
|------|-----------------|-------------|
| イテレーション省略 | 「明らかだから」と判断を飛ばす | 場の定義に従い必要回数を実行 |
| 盲点の見落とし | 一人で進めて気づかない | executor + watcher で相互カバー |
| 学習の欠如 | 同じ失敗を繰り返す | セッション開始時に過去の学びを参照 |
| 属人化 | AIの判断が毎回異なる | 場の定義で一貫したプロセス |

## どの場を使う？

```
あなたのタスクは？
  │
  ├─ 計画・方針を決めたい → Planning Session
  │    「次のフェーズの計画を立てたい」
  │    「ロードマップを作りたい」
  │
  ├─ 設計・トレードオフを判断したい → Design Session
  │    「このAPIの設計方針を決めたい」
  │    「アーキテクチャをレビューして」
  │
  ├─ 実装・作成したい → Implement Session
  │    「fizzbuzz を作って」
  │    「このバグを直して」
  │
  └─ 振り返りたい → Retrospective
       「レトロスペクティブを開催して」
       「何がうまくいかなかったか振り返りたい」
```

## クイックリファレンス

| セッション | 目的 | 人格 | イテレーション |
|-----------|------|------|---------------|
| **Planning** | 方針・計画の決定 | 必須 | 2-3回 |
| **Design** | トレードオフ判断 | 必須 | 2-3回 |
| **Implement** | 仕様の実現 | 条件付き | 1-2回 |
| **Retrospective** | プロセス改善 | 必須 | 1回 |

## 使い方

### 自動選択（推奨）

タスクを依頼するだけで、SessionSelector が適切な場を選びます。

```
ユーザー: 「次のフェーズの計画を立てたい」

SessionSelector: Planning Session を選択
  → 人格: Senge（長期）+ Beck（YAGNI）
  → イテレーション: 最大3回
  → セッション開始時に debt-monitor, coverage-check を実行
```

### 明示的に指定

特定の場を使いたい場合は明示できます。

```
ユーザー: 「Design Session で設計を検討したい」
```

## セッション開始時に自動実行されること

1. **DEBT チェック** - 技術的負債のアラートを確認
2. **Coverage チェック** - 人格選択の多様性を確認
3. **学びの検索** - 関連する過去の Learning Ledger エントリを提示

これにより、過去の失敗を繰り返さず、学びを活かせます。

## 各セッションの詳細

### Planning Session

**いつ使う**: 方針や計画を決めるとき

**参加者**:
- executor: 長期視点（Senge, Sutherland など）
- watcher: YAGNI視点（Beck など）

**アウトプット**: 計画書、受け入れ基準

**例**:
```
「Phase 4 の計画を立てたい」
→ Senge が全体計画を提案
→ Beck が YAGNI で削ぎ落とし
→ 2-3回のイテレーションで計画が磨かれる
```

### Design Session

**いつ使う**: トレードオフを判断するとき

**参加者**:
- executor: 推進側（Beck, Fowler など）
- watcher: 直交する視点（Fowler, Taleb など）

**アウトプット**: 設計書、トレードオフ記録

**例**:
```
「このAPIの設計方針を決めたい」
→ Kent Beck がシンプルな設計を提案
→ Martin Fowler が構造化の視点で盲点をカバー
→ 統合提案を生成
```

### Implement Session

**いつ使う**: 仕様が決まった実装をするとき

**参加者**:
- executor: 実装者
- watcher: 検査者

**アウトプット**: artifacts、検証レポート

**例**:
```
「fizzbuzz を作って」
→ 人格なしで実装
→ watcher が品質検査
→ 1回で完了（通常）
```

### Retrospective

**いつ使う**: 振り返りたいとき、改善したいとき

**参加者**:
- executor: Facilitator（Derby など）
- watcher: System Thinker（Senge など）

**アウトプット**: 改善提案、学び

**例**:
```
「レトロスペクティブを開催して」
→ Esther Derby が 4Ls フォーマットで進行
→ Peter Senge がシステムパターンを観察
→ 改善提案リストを生成
```

## 関連ファイル

| ファイル | 内容 |
|---------|------|
| `session-schema.yaml` | スキーマ定義 |
| `planning.yaml` | Planning Session の詳細定義 |
| `design.yaml` | Design Session の詳細定義 |
| `implement.yaml` | Implement Session の詳細定義 |
| `retrospective.yaml` | Retrospective の詳細定義 |
| `../skills/session-selector.md` | セッション選択スキル |
| `../skills/session-init.md` | セッション初期化スキル |
