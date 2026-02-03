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

### 0. 場の開催（必須ゲート）

**persona-council に委譲して Sprint Planning 場を開催してから、契約を作成する。**

- 委譲せずに sprint-goal-contract を直接作成・更新してはならない（implementation-rules: 場の開催）
- 議論の過程を research/discussions/ に記録する
- 契約に discussion_ref を含める

### 1. Product Goal を確認

```yaml
確認事項:
  - Product Goal は何か
  - このスプリントで Product Goal にどう貢献するか
  - 検証済みの仮説と未検証の仮説
```

### 2. 場の実行（persona-council に委譲）【必須】

Sprint Planning は**場**として実行する。[.cursor/places/sprint-planning.md](../../places/sprint-planning.md) に従い、**persona-council** サブエージェントに委譲する。

- メインエージェントは憑依せず、オーケストレーターとして委譲のみ
- persona-council が複数人格（実名）を憑依させ、議論する
- 議論の過程を research/discussions/ に記録する（人間の学習用）
- **委譲をスキップして契約を作成しない**（implementation-rules: 場の開催）

**場の効果観察**（S10）: 場開催後、insight に place_effect を記録すると H2 検証に役立つ。optional。

委譲できない場合の軽量フォールバック（推奨しない）: [VISION.md - 場の一覧](../../../docs/VISION.md) のチェックリストを確認:
- [ ] 検証する価値は明確か？（PdM）
- [ ] 実現可能か？（TechLead）
- [ ] 検証方法は測れるか？（QA）

### 3. Sprint Goal を設定

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

### 4. 検証方法を定義

```yaml
verification:
  method: "テスト | デモ | ユーザーテスト | メトリクス"
  success_criteria: "何が確認できれば達成か"
  timing: "スプリント終了時 | Review 時"
```

### 5. スコープを決める

```yaml
scope:
  in_scope:
    - "含めるもの"
  out_of_scope:
    - "含めないもの"
  risks:
    - "リスクと対策"
```

### 6. sprint-goal-contract を作成

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
    end: "YYYY-MM-DD"  # 目安。進捗ベースで Sprint を閉じる（core-rules）
```

### 7. チームの合意を取る

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
