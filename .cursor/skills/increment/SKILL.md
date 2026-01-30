---
name: increment
description: |
  成果物（Increment）を定義し、increment-contract を作成するスキル。
  DoD（Definition of Done）を含む完了条件を明確にする。
  使用タイミング: 実装完了時、「これで完了か」を確認したいとき、Review 前の準備時。
---

# Increment Skill

Increment（成果物）を定義し、increment-contract を作成する。

## トリガー

- 実装が完了したとき
- 「これで完了か」を確認したいとき
- Sprint Review の前準備
- 「increment-contract を作成して」と言われたとき

## 前提条件

- sprint-goal-contract が存在する
- 実装対象が明確である

## 実行手順

### 1. 成果物を特定

```yaml
確認事項:
  - 何を作ったか（機能、修正、ドキュメント）
  - Sprint Goal にどう貢献するか
  - 誰が使うか
```

### 2. 完了条件（DoD）を確認

```yaml
dod_checklist:
  code:
    - "コードがレビューされた"
    - "テストが通っている"
    - "リンターエラーがない"
  documentation:
    - "必要なドキュメントが更新された"
    - "API 仕様が最新"
  verification:
    - "動作確認ができた"
    - "受け入れ基準を満たす"
```

### 3. 検証結果を記録

```yaml
verification:
  method: "テスト | デモ | ユーザーテスト"
  results:
    - item: "機能 A"
      status: "pass | fail | partial"
      evidence: "テスト結果 URL、スクリーンショット"
    - item: "機能 B"
      status: "pass"
      evidence: "..."
```

### 4. increment-contract を作成

```yaml
increment-contract:
  id: "INC-YYYYMMDD-{number}"
  
  # Increment は Product Goal へのステップ（スクラムガイド準拠）
  product_goal: "PG-{product-goal-id}"
  
  # このスプリントで作成された場合
  created_in_sprint: "SG-{sprint-goal-id}"
  
  deliverable:
    name: "成果物の名前"
    type: "feature | fix | docs | refactor"
    description: "何を作ったか"
    path: "ファイルパス（該当する場合）"
  
  # Product Goal への貢献が主（Sprint Goal は焦点を提供）
  contribution:
    to_product_goal: "Product Goal へどう近づいたか"
    sprint_context: "このスプリントでなぜこれを作ったか"
  
  dod:
    code:
      - item: "コードレビュー"
        status: "done | pending | na"
      - item: "テスト"
        status: "done"
    documentation:
      - item: "ドキュメント更新"
        status: "done"
    verification:
      - item: "動作確認"
        status: "done"
  
  verification:
    method: ""
    results: []
  
  stakeholders:
    - name: ""
      role: "approver | reviewer"
      approved_at: null
  
  created_at: "YYYY-MM-DD"
  updated_at: "YYYY-MM-DD"
```

### 5. レビューを依頼

- approver に increment-contract を提示
- 検証結果を説明
- 承認を得る

## 出力

- increment-contract（YAML形式）
- Review 用のサマリ

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| DoD を満たさない | 不足項目を明示し、完了させる |
| 検証が不十分 | 追加の検証方法を提案 |
| 承認が得られない | フィードバックを受けて修正 |

## 検証基準

- 成果物が明確に特定されている
- DoD の全項目がチェックされている
- 検証結果が記録されている
- Sprint Goal への貢献が説明できる

## 参照

- [VISION.md - 契約の階層](../../../docs/VISION.md)
- [Scrum Guide - Increment](https://scrumguides.org/)
