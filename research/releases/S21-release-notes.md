# Sprint S21 リリースノート

> Sprint Review の入力用。persona-council が「何を届けたか」を一目で把握する。

**Sprint**: S21  
**期間**: 2026-05-06 ~ 2026-05-13

---

## 届けた価値

この Sprint で誰に何が届いたか。

- **persona-council**: Review で「契約だけ渡されて何をレビューすればいいかわからん」問題が解消される。リリースノートで届けた価値・変更点・検証結果が一目でわかる
- **workload-monitor**: Sprint 終了時にリリースノート生成を推奨する流れに組み込んだ

---

## 変更点

主な成果物・変更ファイル。

| 種別 | パス | 概要 |
|------|------|------|
| テンプレート | research/releases/TEMPLATE.md | リリースノートのフォーマット |
| 場 | .cursor/places/sprint-review.md | 入力に release_notes を追加 |
| スキル | .cursor/skills/release-notes/SKILL.md | リリースノート生成スキル（新設） |
| 契約 | contracts/slice-contract-S21-release-notes.yaml | S21 の slice |

---

## 検証結果

success_criteria の達成状況。

- [x] research/releases/S21-release-notes.md が存在する
- [x] sprint-review.md の入力に release_notes が追加されている

---

## 次 Sprint への引き継ぎ

- S22 以降、Sprint 終了時に release-notes スキルを実行してから Review を開催する
