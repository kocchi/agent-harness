# Session Selector Skill

タスクに適切なセッション（場）を選択する。Task Classifier の拡張。

## トリガー

- ユーザーからタスクを受け取ったとき
- 新しい作業を開始するとき
- Task Classifier の分類後

## 契約

```yaml
name: SessionSelector
version: "1.0.0"

input:
  user_request: string  # ユーザーのリクエスト
  task_type: string     # Task Classifier からの分類結果（オプション）

output:
  session_type: "planning" | "design" | "implement" | "retrospective"
  requires_persona: boolean  # 人格インストールが必要か
  recommended_axes: list     # 推奨する歪み軸
  rationale: string          # なぜこのセッションを選んだか
```

## セッション種類

### 1. Planning Session

**目的**: 方針・計画の決定

**選択条件**:
```yaml
indicators:
  keywords:
    - "計画", "プラン", "方針", "戦略", "ロードマップ"
    - "次のステップ", "優先順位", "スコープ"
  characteristics:
    - 複数の選択肢から方向性を決める
    - 中長期的な視点が必要
    - リソース配分を決める

session_config:
  requires_persona: true
  recommended_axes: ["追加 vs 削減", "短期 vs 長期"]
  min_iterations: 2
  max_iterations: 3
```

### 2. Design Session

**目的**: トレードオフ判断

**選択条件**:
```yaml
indicators:
  keywords:
    - "設計", "アーキテクチャ", "構造", "インターフェース"
    - "トレードオフ", "選択肢", "比較"
  characteristics:
    - 正解が一つではない判断
    - 技術的な選択
    - 長期的な影響がある決定

session_config:
  requires_persona: true
  recommended_axes: ["構造 vs 柔軟", "リスク vs 機会"]
  min_iterations: 2
  max_iterations: 3
```

### 3. Implement Session

**目的**: 仕様の実現

**選択条件**:
```yaml
indicators:
  keywords:
    - "実装", "作成", "作って", "修正", "直して"
    - "コード", "ファイル", "追加"
  characteristics:
    - 決定的に解ける作業
    - 仕様が明確
    - 正解が一つ

session_config:
  requires_persona: false  # Task Classifier が判断
  recommended_axes: []
  min_iterations: 1
  max_iterations: 2
```

### 4. Retrospective Session

**目的**: プロセス改善

**選択条件**:
```yaml
indicators:
  keywords:
    - "振り返り", "ふりかえり", "レトロ", "レトロスペクティブ"
    - "改善", "反省", "学び"
  characteristics:
    - 過去の作業を振り返る
    - プロセスの問題を特定する
    - 改善策を導出する

session_config:
  requires_persona: true
  recommended_axes: ["局所 vs 全体"]
  min_iterations: 1
  max_iterations: 1
```

## 判定フロー

```
1. キーワードマッチング
   ↓ (マッチした場合はそのセッション)
   ↓ (マッチしない場合は次へ)
2. Task Classifier の結果を参照
   - strategy → planning
   - design → design
   - implement → implement
   - investigate → design（判断が必要）
   - review → implement または design（ハイレイヤーかどうか）
   ↓
3. 特性分析
   - 複数の有効なアプローチ → design
   - 決定的に解ける → implement
   - 方向性を決める → planning
   ↓
4. デフォルト
   - design（判断を要する場合が多いため）
```

## 実行手順

1. **リクエスト分析**
   - ユーザーのリクエストを読む
   - キーワードを抽出

2. **セッション選択**
   - キーワードマッチングを適用
   - Task Classifier の結果があれば参照
   - 特性分析でフォールバック

3. **セッション定義を読み込む**
   - `sessions/{session_type}.yaml` を読む
   - requires_persona, recommended_axes を取得

4. **rationale 生成**
   - なぜこのセッションを選んだかを明文化

5. **Learning Ledger に記録**
   - ledger-record スキルを呼び出す
   - component: "SessionSelector"
   - event_type: "decision"

6. **結果を返す**

## 例

### 例1: 計画タスク → Planning Session

**入力**:
```
user_request: "次のフェーズの計画を立てたい"
```

**出力**:
```yaml
session_type: "planning"
requires_persona: true
recommended_axes: ["追加 vs 削減", "短期 vs 長期"]
rationale: "「計画を立てたい」は方針決定タスク。複数の選択肢から方向性を決めるため Planning Session を選択。"
```

### 例2: 設計タスク → Design Session

**入力**:
```
user_request: "このAPIのインターフェース設計をレビューして"
```

**出力**:
```yaml
session_type: "design"
requires_persona: true
recommended_axes: ["構造 vs 柔軟", "リスク vs 機会"]
rationale: "「インターフェース設計をレビュー」は技術的判断を伴う。トレードオフが存在するため Design Session を選択。"
```

### 例3: 実装タスク → Implement Session

**入力**:
```
user_request: "fizzbuzz を作って"
```

**出力**:
```yaml
session_type: "implement"
requires_persona: false
recommended_axes: []
rationale: "「fizzbuzz を作って」は決定的に解ける単純実装。Implement Session を選択。人格インストールは不要。"
```

### 例4: 振り返り → Retrospective Session

**入力**:
```
user_request: "レトロスペクティブを開催してください"
```

**出力**:
```yaml
session_type: "retrospective"
requires_persona: true
recommended_axes: ["局所 vs 全体"]
rationale: "「レトロスペクティブを開催」は明示的な振り返り要求。Retrospective Session を選択。"
```

## Task Classifier との連携

| Task Classifier の task_type | 推奨セッション |
|------------------------------|----------------|
| strategy | planning |
| design | design |
| implement | implement |
| investigate | design |
| review（ハイレイヤー） | design |
| review（構文/実装） | implement |

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| 選択不能 | デフォルト = design |
| セッション定義が見つからない | planning を使用 |
| rationale 生成失敗 | 「選択基準を説明できない」と記録 |

## 参照

- [sessions/session-schema.yaml](../sessions/session-schema.yaml)
- [sessions/planning.yaml](../sessions/planning.yaml)
- [sessions/design.yaml](../sessions/design.yaml)
- [sessions/implement.yaml](../sessions/implement.yaml)
- [sessions/retrospective.yaml](../sessions/retrospective.yaml)
- [task-classifier.md](./task-classifier.md)
