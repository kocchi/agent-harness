# Sprint Planning S17
# 2026-04-10

## 入力

- product-goal-contract-Q2
- S16 完了（断捨離、superpowers ノウハウ保持）

## 分析

```yaml
focus: "OSS 境界の設計、上流追従の手順"
risk: "設計が複雑になりすぎる"
context: "OSS を閉じ込め、チーム固有を主にする。contribute-upstream は OSS への貢献。逆方向（upstream の取り込み）の手順が未定義"
```

## 人格: Mary Poppendieck, Tom Gilb

| speaker | statement |
|---------|-----------|
| Mary Poppendieck | OSS 境界の設計は Cursor の読み込み仕様に依存する。まず「上流追従の手順」を定義する。Fork から upstream を merge する手順が確立すれば、境界の設計は後から調整できる。 |
| Tom Gilb | 上流追従の手順: (1) upstream を remote に追加、(2) fetch、(3) merge、(4) コンフリクト解消。頻度は月 1 回。contribute-upstream の「逆」のスキルを追加する。 |

## 統合

S17 Goal: 上流追従の手順を定義し、follow-upstream スキル（または contribute-upstream の拡張）を作成する。OSS 境界の設計は調査として research に記録し、Cursor の仕様を確認してから具体化する。
