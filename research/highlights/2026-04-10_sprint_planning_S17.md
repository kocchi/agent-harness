---
event: sprint_planning
sprint: S17
date: 2026-04-10
type: interest
source: research/discussions/2026-04-10_sprint_planning_S17.md
personas: [Mary Poppendieck, Tom Gilb]
---

## 依存関係を先に、設計は後から

**Mary Poppendieck**: OSS 境界の設計は Cursor の読み込み仕様に依存する。**まず「上流追従の手順」を定義する。** Fork から upstream を merge する手順が確立すれば、境界の設計は後から調整できる。

**Tom Gilb**: 上流追従の手順: (1) upstream を remote に追加、(2) fetch、(3) merge、(4) コンフリクト解消。頻度は月 1 回。contribute-upstream の「逆」のスキルを追加する。

→ 設計の複雑さを避け、実行可能な手順を先に定義。依存関係（Cursor 仕様）を確認してから境界を具体化する順序。
