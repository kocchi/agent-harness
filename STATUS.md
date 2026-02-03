# 進捗ダッシュボード

> workload-monitor が更新。読む人を眠らせないことを目指す。

**最終更新**: 2026-02-03

---

## 開催中の場

_なし（S25 Review/Retro 終了。議論は [Planning](research/discussions/2026-02-03_sprint_planning_S25.md) / [Review](research/discussions/2026-02-03_sprint_review_S25.md) / [Retro](research/discussions/2026-02-03_sprint_retro_S25.md)）_

---

## 今、何をしているか

**S25 完了**。次は S26 Planning。

**S25 でやったこと**:
- 方式 A（vendor 配置）の設計ドラフト作成
- oss-boundary-design に具体手順を追記

**次にやること**:
1. S26 Sprint Planning を開催（必要に応じて）
2. **コンテキストリフレッシュ: 新セッション開始を推奨**（Sprint 終了後）

---

## Q2 の野望と現実

| やりたいこと | 今の状態 |
|--------------|----------|
| **1チームが複数リポジトリで Sprint を回せる** | オンボーディングはできた。2人目に試してもらう段階（人がいれば） |
| **月1回、upstream の変更を取り込める** | 手順はできた。実際のマージは「いつか」 |
| **OSS を境界内に閉じ込める** | symlink は有効。方式 A（vendor 配置）が検討可能 |

---

## 直近の戦績

| Sprint | やったこと |
|--------|------------|
| **S25**（完了） | 方式 A の設計ドラフト作成。oss-boundary-design に具体手順追記 |
| S24（完了） | 進捗ベース運用確立、Retro 入力に release-notes 追加、STATUS.md 拡張 |
| S23 | OSS symlink 検証完了。release-notes 漏れ。Retro でトリガー改善の仮説 |

---

## 専門家が言ってた面白いこと

persona-council の議論から抽出（[highlights](research/highlights/README.md)）:

- **S17 Planning**: 「依存関係を先に、設計は後から」— 設計が先に立つと空中楼閣になる、という教訓
- **S8 Review**: 作った仕組みが自分自身の検証に使われている。自己言及の美学
- **S7 Review**: 検証の境界を明確にして収束。スコープ creep より学びを優先する判断（Don Reinertsen らしい）

---

## コンテキストサイクル

> ワークロードのサイクルごとにコンテキストをクリアすると劣化を防げる。

| 区切り | 推奨アクション |
|--------|----------------|
| Sprint 終了（Review/Retro 完了後） | **新セッション開始** |
| 長時間作業後（2h 超） | 新セッション開始を検討 |
| workload-monitor で「コンテキストリフレッシュ推奨」が出た場合 | 新セッション開始 |

---

## リンク

| 用途 | パス |
|------|------|
| **リリースノート一覧** | [research/releases/README.md](research/releases/README.md) |
| オンボーディング | [docs/onboarding-multi-repo.md](docs/onboarding-multi-repo.md) |
| Product Goal Q2 | [contracts/product-goal-contract-Q2.yaml](contracts/product-goal-contract-Q2.yaml) |
| 議論ログ | [research/discussions/](research/discussions/) |
| 四半期レポート | [research/quarterly/Q1-2026-report.md](research/quarterly/Q1-2026-report.md) |
