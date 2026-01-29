# log-learning

スライス完了後の学びを構造化して記録する。

## トリガー

- 「学びを記録」「learning を記録」「これは効いた」「この落とし穴を記録」
- スライス完了時に手動呼び出し

## 入力

ユーザーから以下を収集:

| 項目 | 必須 | 説明 |
|------|------|------|
| slice_name | Yes | 関連スライス名 |
| problem | Yes | 発生した問題 |
| insight | Yes | 得られた知見 |
| importance | No | 重要度（デフォルト: medium） |
| tags | No | 検索用タグ |

## 処理フロー

### Step 1: 情報収集

```
ユーザーに以下を確認:
1. slice_name: どのスライスに関する学びか？
2. problem: 何が問題だったか？
3. insight: 何がわかったか？
4. importance: 重要度は？（high/medium/low）
5. tags: 検索用タグは？

省略された場合はデフォルト値を使用:
- importance: medium
- tags: slice_name から推測
```

### Step 2: 追加情報の推測

```
以下を自動推測（ユーザーに確認）:
- root_cause: 問題の根本原因
- correct_approach: 正しいアプローチ
- anti_pattern: 避けるべきパターン
- next_hypothesis: 次の仮説

推測が難しい場合は null を設定。
```

### Step 3: 証拠の収集

```
以下を自動収集:
- commit_hash: 直近のコミットハッシュ
- related_files: 関連ファイル（slice_name から推測）

related_learnings は既存の research/learnings/ から検索。
```

### Step 4: Learning Doc 生成

```
research/learnings/{slice_name}.yaml に以下を出力:

meta:
  created_at: 現在日時
  slice_name: {slice_name}
  tags: {tags}
  importance: {importance}

context:
  problem: {problem}
  root_cause: {root_cause}
  attempted_solutions: []

learning:
  insight: {insight}
  correct_approach: {correct_approach}
  anti_pattern: {anti_pattern}

action:
  rule_change: null
  skill_change: null
  next_hypothesis: {next_hypothesis}

evidence:
  commit_hash: {commit_hash}
  related_files: {related_files}
  related_learnings: {related_learnings}
```

### Step 5: バリデーション

```
生成された YAML が learning-doc-schema.yaml に準拠しているか確認:
- 必須フィールド（meta, context, learning）が存在するか
- importance が enum 値（high/medium/low）か
- created_at が ISO8601 形式か

エラーがあれば修正を促す。
```

## 出力

| 成果物 | パス |
|--------|------|
| Learning Doc | `research/learnings/{slice_name}.yaml` |

## 完了条件

- [ ] Learning Doc が生成された
- [ ] スキーマに準拠している
- [ ] research/learnings/ に保存された

## 使用例

```
User: 学びを記録して

Agent: 学びを記録します。以下を教えてください。

1. **slice_name**: どのスライスに関する学びですか？
2. **problem**: 何が問題でしたか？
3. **insight**: 何がわかりましたか？
4. **importance**: 重要度は？（high/medium/low、デフォルト: medium）
5. **tags**: 検索用タグは？（カンマ区切り）

User: slice_name: vertical-agent-experiment
      problem: PdM Agent と TechLead Agent の引き継ぎが手動だった
      insight: handoff_notes を構造化すれば自動連携できる

Agent: 追加情報を推測しました。確認してください。

- root_cause: エージェント間の受け渡しフォーマットが未定義だった
- correct_approach: handoff_notes を構造化し、チェックリストとして扱う
- anti_pattern: 非構造化テキストでの引き継ぎ
- next_hypothesis: 構造化された引き継ぎは他のエージェント連携にも適用できる

よろしいですか？

User: OK

Agent: research/learnings/vertical-agent-experiment.yaml を作成しました。
```

## 関連

- スキーマ: `source/_shared/learning-doc-schema.yaml`
- 保存先: `research/learnings/`
- 次のスキル: `search-learnings`（検索機能、Phase 2）
