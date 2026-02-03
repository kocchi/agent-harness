---
name: workload-monitor
description: |
  ワークロード監視スキル。Sprint の進捗、障害、プロセス健全性を可視化する。
  scrum-master サブエージェントの主要処理。使用タイミング: セッション開始時、Daily Scrum 相当、
  「ワークロードチェック」「進捗どう？」と言われたとき。
---

# Workload Monitor Skill

Sprint のワークロードを監視し、障害を検出、プロセス健全性をレポートする。

## トリガー

- セッション開始時（rules-validator の後）
- 「Daily Scrum」「ワークロードチェック」「進捗どう？」「スクラムマスター呼んで」
- Sprint 中盤の進捗確認

## 前提条件

- `contracts/` に sprint-goal-contract が存在する
- アクティブな Sprint がある（duration 内）

## 実行手順

### 1. アクティブ Sprint の特定

```yaml
# contracts/sprint-goal-contract-S*.yaml を glob
# duration.start <= today <= duration.end のものを特定
# 複数ある場合は最新の Sprint ID を採用
```

### 2. 関連 slice-contract の収集

- sprint-goal-contract の id を親とする slice-contract を検索
- `parent: "SG-YYYYMMDD-Sn"` で grep

### 3. ワークロード集計

```yaml
各 slice の slices 配列を走査:
  status: completed | in_progress | pending
  size: small | medium | large

集計:
  completed_count
  in_progress_count
  pending_count
  total_count
  completion_rate = completed_count / total_count
```

### 4. 障害検出

| 障害タイプ | 検出条件 | 推奨アクション |
|-----------|---------|---------------|
| stalled_slice | in_progress が 2 日以上変化なし | ブロッカー確認、分割検討 |
| unfinished_retro | retro_actions に status: in_scope が残存 | **Review 時に sprint-goal-contract の retro_actions を done に更新する**。次 Sprint に持ち越し or スコープ調整 |
| over_commit | pending が total の 50% 超 かつ 残り日数が少ない | スコープ縮小を提案 |
| no_active_sprint | アクティブ Sprint なし | Sprint Planning を提案 |

### 5. プロセス健全性

- **Sprint Goal 達成見込み**: completion_rate と残り日数から判定
- **場の実施状況**: sprint-goal-contract の planning_participants、retro_actions の有無
- **ルール整合性**: rules-validator の結果を参照（別実行の場合は要約のみ）

### 6. レポート生成

```yaml
workload_report:
  generated_at: "YYYY-MM-DD HH:mm"
  timing: "session_start | daily | user_request"
  
  sprint:
    id: "SG-YYYYMMDD-Sn"
    goal: ""
    duration: "YYYY-MM-DD ~ YYYY-MM-DD"
    days_remaining: 0
  
  workload:
    completed: 0
    in_progress: 0
    pending: 0
    total: 0
    completion_rate: "0.0%"
  
  impediments:
    - type: ""
      detail: ""
      recommendation: ""
  
  health:
    sprint_goal_at_risk: false
    summary: ""
  
  recommendations:
    - ""
```

## 出力

- ユーザーへの Markdown レポート（簡潔な箇条書き）
- 必要に応じて `contracts/workload-report-{YYYY-MM-DD}.yaml` に記録

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| アクティブ Sprint なし | 「Sprint Planning を実行してください」と案内 |
| slice-contract なし | sprint-goal-contract のみ表示、slice 作成を提案 |
| 契約ファイルのパースエラー | エラー箇所を報告、手動確認を促す |

## 検証基準

- ワークロード数値が正確
- 障害が漏れなく検出されている
- 推奨アクションが具体的

## 参照

- [scrum-master エージェント](../../agents/scrum-master.md)
- [sprint-goal スキル](../sprint-goal/SKILL.md)
- [slice スキル](../slice/SKILL.md)
