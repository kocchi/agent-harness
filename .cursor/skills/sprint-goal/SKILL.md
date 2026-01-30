---
name: sprint-goal
description: |
  Sprint Goal を定義し、sprint-goal-contract を作成するスキル。
  使用タイミング: スプリント計画時、「このスプリントで何を達成するか」を決めたいとき、
  Sprint Planning を実行したいとき。
---

# Sprint Goal Skill

Sprint Planning を実行し、sprint-goal-contract を作成する。

## トリガー

- スプリント計画時
- 「このスプリントで何を達成するか」を決めたいとき
- 「Sprint Planning を開催して」と言われたとき
- Product Goal から Sprint Goal を導出したいとき

## 前提条件

- product-goal-contract または discovery-contract が存在する
- なければ discovery スキルを先に実行

## 実行手順

### 1. Product Goal を確認

```yaml
確認事項:
  - Product Goal は何か
  - このスプリントで Product Goal にどう貢献するか
  - 検証済みの仮説と未検証の仮説
```

### 2. Sprint Goal を設定

```yaml
sprint_goal:
  statement: "このスプリントで〇〇を達成する"
  contribution: "Product Goal への貢献"
  measurable: "何をもって達成とするか"
```

**良い Sprint Goal の条件**:
- 1文で表現できる
- 測定可能
- Product Goal に貢献する
- チームがコミットできる

### 3. 検証方法を定義

```yaml
verification:
  method: "テスト | デモ | ユーザーテスト | メトリクス"
  success_criteria: "何が確認できれば達成か"
  timing: "スプリント終了時 | Review 時"
```

### 4. スコープを決める

```yaml
scope:
  in_scope:
    - "含めるもの"
  out_of_scope:
    - "含めないもの"
  risks:
    - "リスクと対策"
```

### 5. sprint-goal-contract を作成

```yaml
sprint-goal-contract:
  id: "SG-YYYYMMDD-{sprint-number}"
  
  parent: "PG-{product-goal-id}"  # 親の Product Goal
  
  goal:
    statement: ""
    contribution: ""
    measurable: ""
  
  verification:
    method: ""
    success_criteria: ""
    timing: ""
  
  scope:
    in_scope: []
    out_of_scope: []
    risks: []
  
  stakeholders:
    - name: ""
      role: "approver | reviewer | informed"
      commitment: ""
  
  duration:
    start: "YYYY-MM-DD"
    end: "YYYY-MM-DD"
```

### 6. チームの合意を取る

- 開発チームがコミットできるか確認
- スコープが現実的か確認
- リスクへの対策が十分か確認

## 出力

- sprint-goal-contract（YAML形式）
- Sprint Backlog の方向性

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| Product Goal がない | discovery スキルを先に実行 |
| Goal が曖昧 | 「何をもって達成とするか」を質問 |
| スコープが大きすぎる | 分割を提案 |
| チームがコミットできない | スコープを調整 |

## 検証基準

- Sprint Goal が1文で表現できている
- 測定可能な成功基準がある
- スコープが明確
- チームがコミットしている

## 参照

- [VISION.md - 契約の階層](../../../docs/VISION.md)
- [Scrum Guide 2020 - Sprint Goal](https://scrumguides.org/)
