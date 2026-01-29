# Task Orchestrator

v0.3 の5コンポーネントを接続し、セッション管理と状態遷移を管理する。

## トリガー

- ユーザーからタスクを受け取ったとき
- v0.3 の統合フローを実行するとき

## 契約

```yaml
name: TaskOrchestrator
version: "0.2.0"

input:
  user_request: string

output:
  status: "completed" | "failed" | "escalated"
  result:
    session_type: string
    task_type: string
    requires_persona: boolean
    artifacts: string[]
    pass: boolean
    issues: string[]
  session_init:
    debt_alerts: list
    coverage_report: object
    related_learnings: list
  execution_trace:
    - state: string
      timestamp: string
      component: string
      outcome: string
  ledger_entries: string[]

guarantees:
  - 必ず status を返す
  - 例外で落ちない
  - 全状態遷移を execution_trace に記録
  - 各イテレーション後に Learning Ledger に記録
  - セッション開始前に debt-monitor, coverage-check, ledger-query を実行
  - max_iterations: セッション定義に従う（デフォルト 3）

constraints:
  - Orchestrator はビジネスロジックを持たない
  - 状態遷移の判定は各コンポーネントの出力に基づく
  - セッション定義に従ってイテレーション回数を制御
```

## 状態遷移図

```
idle → session_selecting → session_initializing → classifying → [selecting_personas] → executing → watching → learning → completed
                                                                                           ↑_________|
                                                                                         (pass=false, iteration < max)
                                                                                              |
                                                                                        iteration >= max
                                                                                              ↓
                                                                                          escalated → Human
```

## 実行手順

### 初期化

```yaml
state: "idle"
iteration: 0
session_type: null
max_iterations: 3  # セッション定義で上書きされる
execution_trace: []
ledger_entries: []
session_init: {}
```

### Step 0a: session_selecting

1. **状態遷移**: `idle` → `session_selecting`
2. **アクション**: SessionSelector を呼び出す
   ```yaml
   input: user_request
   output: { session_type, requires_persona, recommended_axes, rationale }
   ```
3. **記録**: execution_trace に追加
4. **セッション定義の読み込み**:
   - `sessions/{session_type}.yaml` を読む
   - max_iterations をセッション定義から取得
5. **Learning Ledger 記録**:
   ```yaml
   component: "SessionSelector"
   event_type: "decision"
   content: "session_type = {session_type}"
   rationale: "{rationale}"
   ```
6. **次状態**: `session_initializing`

### Step 0b: session_initializing

1. **状態遷移**: `session_selecting` → `session_initializing`
2. **アクション**: SessionInit を呼び出す
   ```yaml
   input: { session_type, context: user_request }
   output: { debt_alerts, coverage_report, related_learnings, recommendations, ready }
   ```
3. **サブアクション**:
   - debt-monitor を実行 → DEBT アラートを収集
   - coverage-check を実行 → 未使用軸を確認
   - ledger-query を実行 → 関連する過去の学びを検索
4. **記録**: execution_trace に追加、session_init に保存
5. **報告**: ユーザーに以下を報告（アラートがある場合）
   - DEBT アラート
   - 推奨する歪み軸
   - 関連する過去の学び
6. **ブロック判定**:
   - `ready = false` の場合、ユーザーに確認
   - 重大なアラートがある場合、retrospective を推奨
7. **次状態**: `classifying`

### Step 1: classifying

1. **状態遷移**: `session_initializing` → `classifying`
2. **アクション**: TaskClassifier を呼び出す
   ```yaml
   input: user_request
   output: { task_type, requires_persona, rationale }
   ```
3. **記録**: execution_trace に追加
4. **Learning Ledger 記録**:
   ```yaml
   component: "TaskClassifier"
   event_type: "decision"
   content: "task_type = {task_type}, requires_persona = {requires_persona}"
   rationale: "{rationale}"
   ```
5. **次状態の決定**:
   - `requires_persona = true` → `selecting_personas`
   - `requires_persona = false` → `executing`

### Step 2: selecting_personas（条件付き）

**条件**: `requires_persona = true` の場合のみ実行

1. **状態遷移**: `classifying` → `selecting_personas`
2. **アクション**: PersonaStrategy を呼び出す
   ```yaml
   input: { task_type, requires_persona, task_context: user_request }
   output: { executor_persona, watcher_persona, rationale }
   ```
3. **記録**: execution_trace に追加
4. **Learning Ledger 記録**:
   ```yaml
   component: "PersonaStrategy"
   event_type: "decision"
   content: "executor = {executor_persona.name}, watcher = {watcher_persona.name}"
   rationale: "{rationale}"
   ```
5. **次状態**: `executing`

### Step 3: executing

1. **状態遷移**: `selecting_personas` or `classifying` → `executing`
2. **アクション**: executor-agent を呼び出す
   ```yaml
   input: { task_type, context: user_request, persona: executor_persona or null }
   output: { status, artifacts, execution_log, persona_applied }
   ```
3. **記録**: execution_trace に追加
4. **Learning Ledger 記録**:
   ```yaml
   component: "executor"
   event_type: "outcome"
   content: "status = {status}, artifacts = {artifacts}"
   rationale: "{execution_log の要約}"
   ```
5. **タイムアウト**: 30秒で `status = "blocked"`
6. **次状態**: `watching`

### Step 4: watching

1. **状態遷移**: `executing` → `watching`
2. **アクション**: watcher-agent を呼び出す
   ```yaml
   input: { executor_result, original_request: user_request, persona: watcher_persona or null }
   output: { pass, issues, inspection_log, persona_applied, blindspots_covered, misclassification_detected }
   ```
3. **記録**: execution_trace に追加
4. **Learning Ledger 記録**（各イテレーション後）:
   ```yaml
   component: "watcher"
   event_type: "decision"
   content: "pass = {pass}, iteration = {iteration}"
   rationale: "{inspection_log の要約}"
   ```
5. **次状態の決定**:
   - `pass = true` → `learning`
   - `pass = false AND iteration < 3` → `executing`（iteration++）
   - `pass = false AND iteration >= 3` → `escalated`
6. **誤分類検出時**:
   - `misclassification_detected = true` の場合、Learning Ledger に記録

### Step 5: learning

1. **状態遷移**: `watching` → `learning`
2. **アクション**: 最終結果を Learning Ledger に記録
   ```yaml
   component: "TaskOrchestrator"
   event_type: "outcome"
   content: "status = completed, artifacts = {artifacts}"
   rationale: "全フロー完了。iteration = {iteration}"
   ```
3. **次状態**: `completed`

### Step 6: completed / escalated

**completed の場合**:
```yaml
status: "completed"
result:
  task_type: {task_type}
  requires_persona: {requires_persona}
  artifacts: {artifacts}
  pass: true
  issues: []
execution_trace: {全記録}
ledger_entries: {entry_id 一覧}
```

**escalated の場合**:
```yaml
status: "escalated"
result:
  task_type: {task_type}
  requires_persona: {requires_persona}
  artifacts: {最後の artifacts}
  pass: false
  issues: {最後の issues}
execution_trace: {全記録}
ledger_entries: {entry_id 一覧}
message: "3回のイテレーションで合意に達しませんでした。判断をお願いします。"
```

## 例

### 例1: fizzbuzz（人格なし）

```yaml
# 入力
user_request: "fizzbuzz を作って"

# フロー
1. classifying: task_type = "implement", requires_persona = false
2. selecting_personas: スキップ
3. executing: fizzbuzz.py 作成
4. watching: pass = true
5. learning: 記録
6. completed

# 出力
status: "completed"
result:
  task_type: "implement"
  requires_persona: false
  artifacts: ["fizzbuzz.py"]
  pass: true
  issues: []
execution_trace:
  - { state: "classifying", component: "TaskClassifier", outcome: "implement, no persona" }
  - { state: "executing", component: "executor", outcome: "done, fizzbuzz.py" }
  - { state: "watching", component: "watcher", outcome: "pass" }
  - { state: "learning", component: "LearningLedger", outcome: "recorded" }
ledger_entries: ["LEDGER-...", "LEDGER-...", "LEDGER-..."]
```

### 例2: API設計（人格あり）

```yaml
# 入力
user_request: "このAPIの設計方針を決めたい"

# フロー
1. classifying: task_type = "design", requires_persona = true
2. selecting_personas: Kent Beck + Martin Fowler
3. executing: Kent Beck として設計案
4. watching: Martin Fowler として盲点カバー、pass = true
5. learning: 記録
6. completed

# 出力
status: "completed"
result:
  task_type: "design"
  requires_persona: true
  artifacts: ["api-design-proposal.md"]
  pass: true
  issues: ["バージョニング", "エラー形式"]  # 軽微な懸念
execution_trace:
  - { state: "classifying", component: "TaskClassifier", outcome: "design, persona required" }
  - { state: "selecting_personas", component: "PersonaStrategy", outcome: "Kent Beck + Martin Fowler" }
  - { state: "executing", component: "executor", outcome: "done, Kent Beck" }
  - { state: "watching", component: "watcher", outcome: "pass, Martin Fowler" }
  - { state: "learning", component: "LearningLedger", outcome: "recorded" }
ledger_entries: ["LEDGER-...", "LEDGER-...", "LEDGER-...", "LEDGER-..."]
```

### 例3: エスカレーション

```yaml
# 入力
user_request: "難しい判断を要するタスク"

# フロー
1. classifying → selecting_personas → executing → watching (pass=false, iteration=1)
2. executing → watching (pass=false, iteration=2)
3. executing → watching (pass=false, iteration=3)
4. escalated

# 出力
status: "escalated"
result:
  pass: false
  issues: ["3回のイテレーションで合意に達しませんでした"]
message: "判断をお願いします。"
```

## 失敗時の振る舞い

| 状態 | 失敗 | 対策 |
|------|------|------|
| classifying | タイムアウト | task_type = "investigate", requires_persona = true |
| selecting_personas | 人格選択失敗 | デフォルト = Kent Beck + Martin Fowler |
| executing | タイムアウト | status = "blocked" |
| watching | 判定不能 | pass = false |
| learning | 記録失敗 | stderr にフォールバック |

## 参照

### セッション関連
- [Session Schema](../sessions/session-schema.yaml)
- [Session Selector](./session-selector.md)
- [Session Init](./session-init.md)
- [Planning Session](../sessions/planning.yaml)
- [Design Session](../sessions/design.yaml)
- [Implement Session](../sessions/implement.yaml)
- [Retrospective Session](../sessions/retrospective.yaml)

### コンポーネント
- [Task Classifier](./task-classifier.md)
- [Persona Strategy](../agents/persona-strategy.md)
- [executor-agent](../agents/executor.md)
- [watcher-agent](../agents/watcher.md)
- [Learning Ledger](../ledger/learning-ledger.yaml)

### 監視スキル
- [debt-monitor](./debt-monitor.md)
- [coverage-check](./coverage-check.md)
- [ledger-query](./ledger-query.md)
