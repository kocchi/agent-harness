# Sprint S26 リリースノート

> Sprint Review の入力用。persona-council が「何を届けたか」を一目で把握する。

**Sprint**: S26  
**期間**: 2026-02-03 ~ 2026-02-10

---

## 届けた価値

この Sprint で誰に何が届いたか。

- **Product Goal Q2**: 方式 A のセットアップ手順をドッグフーディングで検証。oss-boundary-design に抜け漏れ・修正を追記
- **チーム**: kp-agent-harness で方式 A の構成（vendor + symlink）が動作することを確認

---

## 変更点

主な成果物・変更ファイル。

| 種別 | パス | 概要 |
|------|------|------|
| ドキュメント | research/oss-boundary-design-S17.md | S26 ドッグフーディング結果、submodule URL 修正、symlink 具体コマンド追記 |
| 契約 | contracts/sprint-goal-contract-S26.yaml | S26 Goal |
| 契約 | contracts/slice-contract-S26-dogfooding.yaml | S1-S4 |
| 議論 | research/discussions/2026-02-03_sprint_planning_S26.md | Planning 議論 |
| ドッグフーディング | desc-dena/kp-agent-harness（ローカル） | vendor submodule + oss-* symlink 構成 |

---

## 検証結果

success_criteria の達成状況。

- [x] Fork リポジトリで方式 A のセットアップが完了している（kp-agent-harness）
- [x] research/oss-boundary-design-S17.md にドッグフーディングで発見した抜け漏れ・不明点が追記されている
- [x] research/releases/S26-release-notes.md が存在する

---

## 次 Sprint への引き継ぎ

- kp-agent-harness の変更は未コミット（Fork 作成・push はユーザー判断）
- 方式 A の実装はコンフリクト多発時に検討
