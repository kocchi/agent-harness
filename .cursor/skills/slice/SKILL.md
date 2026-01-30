---
name: slice
description: |
  価値を分類（必須/既知/未知）し、slice-contract を作成するスキル。
  未知の価値を早期に検証する計画を立てる。
  使用タイミング: 機能を分解したいとき、「何から作るべきか」を決めたいとき、
  バーティカルスライスで実装を進めたいとき。
---

# Slice Skill

価値を分類し、slice-contract を作成する。

## トリガー

- 機能を分解したいとき
- 「何から作るべきか」を決めたいとき
- バーティカルスライスで実装を進めたいとき
- 「価値を分類して」と言われたとき

## 前提条件

- discovery-contract または sprint-goal-contract が存在する
- 実装対象の機能が特定されている

## 実行手順

### 1. 機能を分解

大きな機能を小さなスライスに分解する:

```yaml
slices:
  - id: S1
    name: "スライスの名前"
    description: "何を実現するか"
    size: "small | medium | large"
```

### 2. 価値を分類

各スライスを3種類に分類:

```yaml
classification:
  must:  # 必須（なければ価値がない）
    - slice_id: S1
      reason: "これがないと〇〇ができない"
      verify_timing: "実装前"
  
  known:  # 既知（作り方がわかっている）
    - slice_id: S2
      reason: "過去に同様の実装をした"
      verify_timing: "実装後"
  
  unknown:  # 未知（検証が必要）
    - slice_id: S3
      reason: "技術的に実現可能か不明"
      verify_timing: "最初のマイルストーン"
      experiment: "プロトタイプで検証"
```

**分類の基準**:

| 分類 | 基準 | 検証タイミング |
|------|------|---------------|
| 必須 | なければ価値がない | 実装前 |
| 既知 | 作り方がわかっている | 実装後 |
| 未知 | 検証が必要 | 最初のマイルストーン（早期） |

### 3. 実装順序を決める

```yaml
order:
  1: "未知のスライス（早期検証）"
  2: "必須のスライス（価値の核）"
  3: "既知のスライス（効率的に実装）"
```

**重要**: 未知を後回しにしない。早期に検証して方向転換できるようにする。

### 4. slice-contract を作成

```yaml
slice-contract:
  id: "SLC-YYYYMMDD-{feature}"
  
  parent: "SG-{sprint-goal-id}"
  
  feature:
    name: "機能の名前"
    description: "何を実現するか"
  
  slices:
    - id: S1
      name: ""
      description: ""
      size: ""
      classification: "must | known | unknown"
      reason: ""
      verify_timing: ""
      experiment: ""  # unknown の場合のみ
  
  order:
    - slice_id: S1
      priority: 1
      rationale: ""
  
  risks:
    - slice_id: S3
      risk: "技術的に実現不可能かもしれない"
      mitigation: "プロトタイプで早期検証"
  
  stakeholders:
    - name: ""
      role: "approver | reviewer"
  
  created_at: "YYYY-MM-DD"
  updated_at: "YYYY-MM-DD"
```

### 5. 合意を取る

- スライスの分類と順序を説明
- 未知のスライスの検証計画を確認
- approver の承認を得る

## 出力

- slice-contract（YAML形式）
- 実装順序と検証計画

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| 分解できない | より具体的な要件を質問 |
| 全部「必須」になる | 「これがなくても価値はあるか？」で再評価 |
| 全部「既知」になる | 「本当に検証不要か？」で再評価 |
| 順序が決まらない | リスクの大きさで優先順位付け |

## 検証基準

- スライスが具体的で実装可能
- 分類の理由が明確
- 未知のスライスに検証計画がある
- 実装順序にリスク対策が反映されている

## 参照

- [VISION.md - 価値分類](../../../docs/VISION.md)
- [philosophy.md - P7: 契約で価値をつなぐ](../../../docs/philosophy.md)
- [task-classifier スキル](../task-classifier/SKILL.md)
