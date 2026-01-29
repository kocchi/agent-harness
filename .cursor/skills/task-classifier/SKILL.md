# Task Classifier Skill

ユーザーのリクエストを分類し、人格インストールの要否を判定する。

## トリガー

- ユーザーからタスクを受け取ったとき
- 新しい作業を開始するとき

## 契約

```yaml
name: TaskClassifier
version: "0.1.0"

input:
  user_request: string  # ユーザーのリクエスト

output:
  task_type: "implement" | "investigate" | "review" | "design" | "strategy"
  requires_persona: boolean  # 人格インストールが必要か
  rationale: string  # なぜこう分類したか
```

## 分類ルール

### 人格が必要な課題（requires_persona = true）

```yaml
conditions:
  - complexity: high  # 判断が必要
  - ambiguity: high   # 曖昧性が高い
  - multiple_valid_approaches: true  # 正解が複数ある
  - judgment_required: true  # メンタルモデルに依存する判断
```

### 人格が不要な課題（requires_persona = false）

```yaml
conditions:
  - deterministic: true  # 決定的に解ける
  - single_correct_answer: true  # 正解が一つ
  - mechanical: true  # 機械的に実行可能
```

## パターンマッチング（フォールバック）

| パターン | task_type | requires_persona |
|---------|-----------|------------------|
| 直して、修正、バグ、実装して | implement | false |
| 調べて、なぜ、原因、分析 | investigate | true |
| レビュー、チェック、確認 | review | conditional |
| 設計、アーキテクチャ、構造 | design | true |
| 方向性、戦略、判断、決定 | strategy | true |
| デフォルト | investigate | true |

### review の条件分岐

- **ハイレイヤーレビュー**（アーキテクチャ、設計）: requires_persona = true
- **構文/実装レビュー**（コード、テスト）: requires_persona = false

## 実行手順

1. **リクエスト分析**
   - ユーザーのリクエストを読む
   - キーワードとコンテキストを抽出

2. **分類判定**
   - パターンマッチングを適用
   - 曖昧な場合は判断基準（complexity, ambiguity）を適用
   - デフォルトは investigate + requires_persona = true

3. **rationale 生成**
   - なぜこの分類にしたかを明文化
   - 「決定的に解ける」「判断が必要」などの理由を含める

4. **Learning Ledger に記録**
   - ledger-record スキルを呼び出す
   - component: "TaskClassifier"
   - event_type: "decision"

5. **結果を返す**

## 例

### 例1: 単純実装（人格なし）

**入力**:
```
user_request: "fizzbuzz を作って"
```

**出力**:
```yaml
task_type: "implement"
requires_persona: false
rationale: "「fizzbuzz を作って」は決定的に解ける単純実装タスク。正解は一つであり、人格注入は不要。"
```

### 例2: 設計判断（人格あり）

**入力**:
```
user_request: "このAPIの設計方針を決めたい"
```

**出力**:
```yaml
task_type: "design"
requires_persona: true
rationale: "設計方針の決定は複数の有効なアプローチがあり、メンタルモデルに依存する判断が必要。人格注入により視点を明確化する。"
```

### 例3: 戦略判断（人格あり）

**入力**:
```
user_request: "次のスプリントの方向性を決めたい"
```

**出力**:
```yaml
task_type: "strategy"
requires_persona: true
rationale: "戦略的意思決定は高い曖昧性があり、複数の判断軸が存在する。人格注入により一貫した判断基準を適用する。"
```

### 例4: ハイレイヤーレビュー（人格あり）

**入力**:
```
user_request: "このアーキテクチャをレビューして"
```

**出力**:
```yaml
task_type: "review"
requires_persona: true
rationale: "アーキテクチャレビューは設計判断を含むハイレイヤーレビュー。異なる視点からの評価が有効。"
```

### 例5: 構文レビュー（人格なし）

**入力**:
```
user_request: "このPRのコードスタイルをチェックして"
```

**出力**:
```yaml
task_type: "review"
requires_persona: false
rationale: "コードスタイルチェックは機械的に実行可能な構文レビュー。人格注入は不要。"
```

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| 分類不能 | デフォルト = investigate + requires_persona = true |
| タイムアウト | デフォルトを返す |
| rationale 生成失敗 | 「分類基準を説明できない」と記録 |

## 検証基準

- 10個のサンプルで8割以上正解
- rationale が人間に理解可能
- VISION.md の「課題タイプと推奨パターン」と一致

## 参照

- [VISION.md - 課題タイプと推奨パターン](../../../../VISION.md)
- [v0.3 実装計画](../../../../research/proposals/2026-01-28_v0.3-implementation-plan.md)
