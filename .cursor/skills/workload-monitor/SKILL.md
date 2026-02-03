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
# duration.start <= today かつ Review/Retro 未開催のものを特定
# duration.end は目安。進捗ベースで Sprint を閉じる（core-rules）
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
| over_commit | pending が total の 50% 超 | スコープ縮小を提案 |
| no_active_sprint | アクティブ Sprint なし | Sprint Planning を提案 |
| sprint_ready | slice が完了、リリースノート未作成 | **release-notes を実行してから Review を開催する**。日付は待たない |

### 5. コンテキストサイクル

コンテキストは長時間で劣化する。以下の区切りで **新セッション開始** を推奨する:

| 区切り | recommendations に追加 |
|--------|------------------------|
| Sprint 終了（Review/Retro 完了後） | 「コンテキストリフレッシュ: 新セッション開始を推奨」 |
| 長時間作業後（目安 2h 超） | 同上（ユーザー依頼時のみ検出可能） |

STATUS.md の「コンテキストサイクル」セクションを更新する。

### 6. プロセス健全性

- **Sprint Goal 達成見込み**: completion_rate と残り日数から判定
- **場の実施状況**: sprint-goal-contract の planning_participants、retro_actions の有無
- **ルール整合性**: rules-validator の結果を参照（別実行の場合は要約のみ）
- **Sprint 完了可能時**: slice が完了かつ `research/releases/S{NN}-release-notes.md` がなければ、recommendations に「release-notes を実行してから Review を開催する」を追加（日付は待たない）

### 7. 場の開催中は STATUS.md をリアルタイム更新

**場（Sprint Planning, Review, Retro）開催中**は、以下を STATUS.md に追記する:

| セクション | 内容 |
|------------|------|
| 開催中の場 | 場名、対象（Sprint ID など）、開始日時 |
| 会話 | 議論の過程をリアルタイムで追記。人格名と発言を記録 |

場が終了したら、議論を research/discussions/ に移し、STATUS.md の「開催中の場」「会話」をクリアする。

### 8. レポート生成と STATUS.md 更新

workload_report を生成し、**STATUS.md** を更新する。STATUS.md は進捗の単一参照先。

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

**STATUS.md の構成**（読む人を眠らせない方針）:

| セクション | 内容 |
|------------|------|
| 開催中の場 | 場名、対象、開始。会話をリアルタイム追記。場終了でクリア |
| 今、何をしているか | アクティブ Sprint の焦点を1文。次にやることを具体的に2-3個 |
| Q2 の野望と現実 | Product Goal の success_metrics を「やりたいこと」「今の状態」で平易に |
| 直近の戦績 | 直近 3 Sprint の「やったこと」を結果ベースで |
| 専門家が言ってた面白いこと | research/highlights から 2-3 件を抜粋（安心・面白み） |
| コンテキストサイクル | 区切りごとの新セッション推奨。劣化防止 |
| リンク | オンボーディング、契約、議論ログ、四半期レポート |

**トーン**: ユーモアを軽く。抽象的な指標名より「何がわかるか」で書く。障害がないときは障害セクションを省略。

## 出力

- ユーザーへの Markdown レポート（簡潔な箇条書き）
- **STATUS.md を更新する**（進捗ダッシュボード。ここだけ見れば進捗が追える）
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
- [release-notes スキル](../release-notes/SKILL.md)（Sprint 終了時に Review の前で実行）
- [sprint-goal スキル](../sprint-goal/SKILL.md)
- [slice スキル](../slice/SKILL.md)
