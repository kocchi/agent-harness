# 進捗ダッシュボード

> workload-monitor が更新。読む人を眠らせないことを目指す。

**最終更新**: 2026-05-20

---

## 今、何をしているか

**S23**: release-notes の習慣化を継続しつつ、OSS 境界の symlink 検証に着手。神のみぞ知る領域に踏み込む。

**次にやること**（逃げられない）:
1. .cursor/rules に symlink を置いて Cursor が読むか検証
2. 検証結果を research に記録
3. S23 終了時に release-notes を実行

---

## Q2 の野望と現実

| やりたいこと | 今の状態 |
|--------------|----------|
| **1チームが複数リポジトリで Sprint を回せる** | オンボーディングはできた。2人目に試してもらう段階（人がいれば） |
| **月1回、upstream の変更を取り込める** | 手順はできた。実際のマージは「いつか」 |
| **OSS を境界内に閉じ込める** | Cursor の仕様は調べた。symlink が動くかは神のみぞ知る |

---

## 直近の戦績

| Sprint | やったこと |
|--------|------------|
| **S23**（進行中） | release-notes 継続、OSS symlink 検証 |
| S22 | release-notes 習慣化の初回検証成功。Review/Retro 完了 |
| S21 | リリースノートの仕組み導入。TEMPLATE、release-notes スキル、workload-monitor 連携 |

---

## 専門家が言ってた面白いこと

persona-council の議論から抽出（[highlights](research/highlights/README.md)）:

- **S17 Planning**: 「依存関係を先に、設計は後から」— 設計が先に立つと空中楼閣になる、という教訓
- **S8 Review**: 作った仕組みが自分自身の検証に使われている。自己言及の美学
- **S7 Review**: 検証の境界を明確にして収束。スコープ creep より学びを優先する判断（Don Reinertsen らしい）

---

## リンク

| 用途 | パス |
|------|------|
| オンボーディング | [docs/onboarding-multi-repo.md](docs/onboarding-multi-repo.md) |
| Product Goal Q2 | [contracts/product-goal-contract-Q2.yaml](contracts/product-goal-contract-Q2.yaml) |
| 議論ログ | [research/discussions/](research/discussions/) |
| 四半期レポート | [research/quarterly/Q1-2026-report.md](research/quarterly/Q1-2026-report.md) |
