---
name: release-notes
description: |
  Sprint 終了時にリリースノートを生成するスキル。
  Sprint Review の入力として persona-council に渡す。使用タイミング: Sprint 終了時、Review の前。
---

# Release Notes Skill

Sprint で届けた価値・変更点・検証結果を1ページにまとめ、Review の入力にする。

## トリガー

- Sprint 終了時（Review の前）
- 「リリースノートを作成して」「Review の準備をして」と言われたとき
- workload-monitor の推奨（Sprint 終了日に「release-notes を実行してから Review を」）

## 前提条件

- sprint-goal-contract が存在する
- 関連する slice-contract が存在する

## 実行手順

### 1. 入力の収集

- sprint-goal-contract（goal, verification.success_criteria, duration）
- 関連 slice-contract（parent が sprint-goal の id）
- 成果物のパス（契約、スキル、ドキュメントの変更）

### 2. テンプレートを埋める

`research/releases/TEMPLATE.md` をコピーし、以下を埋める:

```yaml
置換:
  {NN}: Sprint 番号（例: 21）
  {start}: duration.start
  {end}: duration.end

届けた価値: sprint-goal と slice の完了状況から要約
変更点: 成果物のパスと概要を表形式で
検証結果: success_criteria の達成状況をチェックリストで
次 Sprint への引き継ぎ: insight や retro_actions から
```

### 3. 出力

- `research/releases/S{NN}-release-notes.md`

### 4. Review への引き渡し

- sprint-review 場の materials に `research/releases/S{NN}-release-notes.md` を含める

## 出力フォーマット

TEMPLATE.md に準拠。届けた価値、変更点、検証結果、引き継ぎを1ページに収める。

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| sprint-goal-contract なし | Sprint Planning を先に実行 |
| slice がすべて pending | 完了した成果物のみ記載。未完了は「次 Sprint へ」に記載 |

## 参照

- [sprint-review 場](../../places/sprint-review.md)
- [research/releases/TEMPLATE.md](../../../research/releases/TEMPLATE.md)
