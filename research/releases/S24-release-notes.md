# Sprint S24 リリースノート

> Sprint Review の入力用。persona-council が「何を届けたか」を一目で把握する。

**Sprint**: S24  
**期間**: 2026-02-03 ~ 2026-02-10

---

## 届けた価値

この Sprint で誰に何が届いたか。

- **プロセス**: Sprint を進捗ベースで進める方針を確立。日付でブロックしない
- **可視化**: STATUS.md に開催中の場・会話のリアルタイム更新、コンテキストサイクルを追加
- **ワークフロー**: コミットは Increment ごと、プッシュは Sprint 終了時。コミット時のユーザー確認を廃止

---

## 変更点

主な成果物・変更ファイル。

| 種別 | パス | 概要 |
|------|------|------|
| ルール | .cursor/rules/core-rules.mdc | Sprint 進捗ベース、コミット/プッシュ分離 |
| ルール | .cursor/rules/implementation-rules.mdc | コミット時ユーザー確認不要 |
| スキル | .cursor/skills/workload-monitor/SKILL.md | sprint_ready、コンテキストサイクル、場のリアルタイム更新 |
| スキル | .cursor/skills/release-notes/SKILL.md | トリガーを slice 完了時に変更 |
| 場 | .cursor/places/sprint-planning.md, sprint-review.md, sprint-retro.md | STATUS.md リアルタイム更新 |
| 契約 | contracts/sprint-goal-contract-S24.yaml | S24 Goal |
| 契約 | contracts/slice-contract-S24-trigger.yaml | S1-S3 |
| 契約 | contracts/product-goal-contract-Q2.yaml | iteration 更新 |
| ドキュメント | STATUS.md | 開催中の場、会話、コンテキストサイクル |
| 議論 | research/discussions/2026-02-03_sprint_planning_S24.md | Planning 議論 |
| 議論 | research/discussions/2026-02-03_sprint_retro_S23.md | Retro S23 |

---

## 検証結果

success_criteria の達成状況。

- [x] research/releases/S24-release-notes.md が存在する
- [x] Retro 開催時に release-notes が入力として揃っている（Review/Retro で検証済み）
- [x] product-goal-contract-Q2 の iteration.current が S24 を反映

---

## 次 Sprint への引き継ぎ

- S25 で sprint-retro の入力に release-notes を必須追加するか判断（S24 Retro で検証成功なら反映）
- 進捗ベースの Sprint 運用を継続
