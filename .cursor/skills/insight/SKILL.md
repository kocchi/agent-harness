---
name: insight
description: |
  学びを記録し、insight-contract を作成するスキル。Review/Retro で得た学びを構造化し、
  ルール/スキル/ナレッジに還元する。使用タイミング: Sprint Review 後、Retro 後、
  「これは学びだ」と気づいたとき、「記録しておきたい」と思ったとき。
---

# Insight Skill

学びを構造化し、insight-contract を作成する。

## トリガー

- Sprint Review 後
- Sprint Retro 後
- **スライス完了後**（旧 log-learning の機能を統合）
- 「これは学びだ」と気づいたとき
- 「記録しておきたい」「学びを記録」と言われたとき
- 実験や検証の結果が出たとき

## 実行手順

### 0. Review/Retro の場合: 場の開催（必須ゲート）

**Sprint Review / Retro で insight を作成する場合、先に persona-council に委譲して場を開催する。**

- 委譲せずに insight-contract を直接作成・更新してはならない（implementation-rules: 場の開催）
- 議論の過程を research/discussions/ に記録する
- insight に discussion_ref を含める
- 完了時に sprint-goal-contract の retro_actions を done に更新する

※「スライス完了後」「これは学びだ」など、Review/Retro 以外のトリガーではこのステップは不要。

### 1. 学びを抽出

```yaml
質問:
  - 「何がわかりましたか？」
  - 「予想と違ったことは？」
  - 「次に活かせることは？」
  - 「他の人にも伝えたいことは？」
```

### 2. 学びを構造化

```yaml
insight:
  statement: "〇〇ということがわかった"
  source: "Review | Retro | スライス完了 | 実験 | 調査 | 実装"
  evidence:
    summary: "根拠の要約（1-2文）"
    references:  # オプション: 詳細への参照
      - type: "url | file | conversation"
        path: "https://... | research/... | transcript ID"
        description: "何が書いてあるか"
  confidence: "high | medium | low"
```

**evidence の書き方**:
- `summary`: 必須。根拠を1-2文で要約
- `references`: オプション。詳細を参照したい場合に追加
  - 外部 URL、research/ 内のファイル、会話のトランスクリプトなど
  - 本文を insight-contract に含めない（コード量削減）

### 3. 影響範囲を特定

```yaml
impact:
  affects:
    - "影響する契約（Product Goal, Sprint Goal など）"
  action_required: "アクションが必要か"
  urgency: "high | medium | low"
```

### 4. 次のアクションを決める

```yaml
next_action:
  type: "rule | skill | knowledge | none"
  description: "具体的なアクション"
  owner: "誰がやるか"
  deadline: "いつまでに"
```

| 学びの種類 | 還元先 |
|-----------|--------|
| 常に適用すべき制約 | ルール（.mdc） |
| 特定タスクの手順 | スキル（SKILL.md） |
| 参照用の情報 | ナレッジ（references/） |
| アクション不要 | 記録のみ |

### 5. insight-contract を作成

```yaml
insight-contract:
  id: "INS-YYYYMMDD-{number}"
  
  insight:
    statement: ""
    source: ""
    evidence: ""
    confidence: ""
  
  impact:
    affects: []
    action_required: true | false
    urgency: ""
  
  next_action:
    type: ""
    description: ""
    owner: ""
    deadline: ""
  
  status: "new | validated | applied | archived"
  
  created_at: "YYYY-MM-DD"
  updated_at: "YYYY-MM-DD"
```

### 6. 還元を実行（オプション）

`next_action.type` に応じて:

- **rule**: 新しいルールを作成または既存ルールを更新
- **skill**: 新しいスキルを作成または既存スキルを更新
- **knowledge**: references/ にドキュメントを追加
- **none**: 記録のみで終了

## 出力

- insight-contract（YAML形式）
- （オプション）新しいルール、スキル、ナレッジ

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| 学びが曖昧 | 「具体的に何がわかりましたか？」と質問 |
| 根拠がない | 「どうしてそう思いますか？」と質問 |
| アクションが不明確 | 「次に何をすべきですか？」と質問 |
| 優先度が不明 | 「どれくらい急ぎますか？」と質問 |

## 検証基準

- 学びが1文で表現できている
- 根拠が明記されている
- 影響範囲が特定されている
- 次のアクションが明確（または「アクション不要」と判断）

## 参照

- [VISION.md - 学習系契約](../../../docs/VISION.md)
- [philosophy.md - P4: 知識を蓄積](../../../docs/philosophy.md)
