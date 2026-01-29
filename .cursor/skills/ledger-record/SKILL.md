# Ledger Record Skill

Learning Ledger に判断を記録する。

## トリガー

- 判断を下したとき
- 成果物を出力したとき
- 異常を検知したとき
- 技術的負債を認識したとき

## 契約

```yaml
input:
  component: "TaskClassifier" | "PersonaStrategy" | "executor" | "watcher"
  event_type: "decision" | "outcome" | "anomaly" | "debt_recognized"
  content: string  # 何をしたか
  rationale: string  # なぜそうしたか（必須、空禁止）

output:
  entry_id: string  # LEDGER-{timestamp}
```

## 実行手順

1. **入力検証**
   - component が有効な値か確認
   - event_type が有効な値か確認
   - rationale が空でないか確認（空なら reject）

2. **エントリ作成**
   ```yaml
   - id: "LEDGER-{ISO8601タイムスタンプ}"
     timestamp: "{ISO8601タイムスタンプ}"
     component: "{入力のcomponent}"
     event_type: "{入力のevent_type}"
     content: "{入力のcontent}"
     rationale: "{入力のrationale}"
   ```

3. **記録**
   - `learnings/flow/entries.yaml` に追記
   - ファイルが存在しない場合は作成
   - 追記のみ（既存エントリを編集しない）

4. **entry_id を返す**

## 例

### 入力

```yaml
component: "TaskClassifier"
event_type: "decision"
content: "task_type = 'implement', requires_persona = false"
rationale: "「fizzbuzz を作って」は決定的に解ける単純実装タスクであり、人格注入は不要と判断"
```

### 出力

```yaml
entry_id: "LEDGER-2026-01-28T12:34:56Z"
```

### entries.yaml への追記

```yaml
- id: "LEDGER-2026-01-28T12:34:56Z"
  timestamp: "2026-01-28T12:34:56Z"
  component: "TaskClassifier"
  event_type: "decision"
  content: "task_type = 'implement', requires_persona = false"
  rationale: "「fizzbuzz を作って」は決定的に解ける単純実装タスクであり、人格注入は不要と判断"
```

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| rationale が空 | エラーを返し、記録しない |
| ファイル書き込み失敗 | stderr にフォールバックログを出力 |
| component/event_type が無効 | エラーを返し、記録しない |

## 注意事項

- **削除禁止**: 一度記録したエントリは削除しない
- **編集禁止**: 一度記録したエントリは編集しない
- **rationale 必須**: 理由のない判断は記録しない
- **schema_version**: 将来のスキーマ変更に備え、バージョンを意識する
