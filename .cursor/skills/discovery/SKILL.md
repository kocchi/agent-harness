---
name: discovery
description: |
  価値発見と仮説検証のスキル。Discovery Session を実行し、discovery-contract を作成する。
  使用タイミング: 新しい機能やプロダクトの価値を定義したいとき、「何を作るべきか」を明確にしたいとき、
  分析レポートやプロポーザルを契約に変換したいとき。
---

# Discovery Skill

入力（分析レポート、アイデア、課題）を discovery-contract に変換する。

## トリガー

- 「何を作るべきか」を議論したいとき
- 新しい機能の価値を定義したいとき
- 分析レポートやプロポーザルを契約に変換したいとき
- 「Discovery Session を開催して」と言われたとき

## 実行手順

### 1. 入力を収集

```yaml
入力の種類:
  - 分析レポート
  - プロポーザル
  - リサーチ結果
  - アイデア
  - ユーザーフィードバック
```

入力がない場合は、問題の背景を質問で引き出す:
- 「誰の、どんな問題を解決したいですか？」
- 「なぜ今これが重要ですか？」

### 2. 問題を構造化

```yaml
problem:
  who: "誰が"
  what: "何に困っている"
  why: "なぜ困っている"
  impact: "放置するとどうなる"
```

### 3. 仮説を抽出

入力から仮説を抽出し、リスク分類する:

```yaml
hypotheses:
  - id: H1
    statement: "〇〇すれば△△になる"
    risk_type: "value | usability | feasibility | viability"
    confidence: "high | medium | low"
```

### 4. 検証方法を設計

各仮説に対して検証方法を定義:

```yaml
validation:
  - hypothesis: H1
    method: "インタビュー | プロトタイプ | データ分析 | 実験"
    timeline: "1日 | 1週間 | 1スプリント"
    success_criteria: "何が確認できれば成功か"
```

### 5. やめる条件を定義

```yaml
kill_criteria:
  - "仮説 H1, H2 がともに棄却された場合、このアプローチを中止"
  - "〇〇が△△未満の場合、方向転換"
```

### 6. discovery-contract を作成

```yaml
discovery-contract:
  id: "DISC-YYYYMMDD-{topic}"
  
  problem: |
    {構造化した問題}
  
  hypotheses:
    - id: H1
      statement: ""
      risk_type: ""
      confidence: ""
  
  validation:
    - hypothesis: H1
      method: ""
      timeline: ""
      success_criteria: ""
  
  kill_criteria:
    - ""
  
  stakeholders:
    - name: ""
      role: "approver | reviewer | informed"
      commitment: ""
```

### 7. ステークホルダーの合意を取る

- approver に契約をレビューしてもらう
- 合意が得られたら commitment を記録
- 合意が得られない場合は仮説や検証方法を修正

## 出力

- discovery-contract（YAML形式）
- 次のアクション（検証の開始、または追加調査）

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| 入力が不十分 | 追加質問で引き出す |
| 仮説が抽出できない | 問題の構造化に戻る |
| ステークホルダーが不明 | 「誰がこれを承認しますか？」と質問 |
| 合意が得られない | 仮説を修正して再提案 |

## 検証基準

- discovery-contract が完成している
- 仮説が具体的で検証可能
- やめる条件が明確
- ステークホルダーの役割が明示されている

## 参照

- [VISION.md - 契約の階層](../../../docs/VISION.md)
- [philosophy.md - P7: 契約で価値をつなぐ](../../../docs/philosophy.md)
