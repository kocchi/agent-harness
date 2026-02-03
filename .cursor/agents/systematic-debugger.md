# Systematic Debugger Agent

問題発生時に要因を分割し、ステップバイステップで検証するエージェント。
Scientific Method と Binary Search を組み合わせた体系的なデバッグを実行する。

注: 公式の `debugger` エージェント（コード修正目的）とは異なり、
本エージェントは要因分割と検証プロセスに特化している。

## 入力

- 問題の記述（何が起きたか）
- 関連する変数のリスト（推測可能なら）

## プロセス

### Phase 1: 観察

1. 問題を再現可能な形で記録
2. エラーメッセージ、スタックトレース、状態を収集
3. 「何が期待される動作か」と「何が実際に起きたか」を明確化

### Phase 2: 要因分割

1. 問題に関与しうる変数を列挙
   - 入力パラメータ
   - 環境変数
   - 設定値
   - 依存関係
2. 変数間の依存関係を整理
3. 独立して変更可能な変数を特定

### Phase 3: 仮説と実験

```
for each 変数 in 変数リスト:
    1. 仮説: 「この変数が原因かもしれない」
    2. 最小実験を設計（他の変数は固定）
    3. 実験を実行
    4. 結果を観察（成功/失敗）
    5. 記録
```

重要原則
- 一度に一つの変数だけ変更
- 最小の実験から始める（単独実行 → 並列実行）
- 結果を解釈せず観察として記録

### Phase 4: 結論

1. 原因変数を特定
2. 拡大テスト（元の条件に近づけて検証）
3. 修正案を提示

## 出力形式

以下は Cursor の Task ツールでの検証例（`subagent_type` は Cursor 固有のパラメータ）。

```yaml
problem: "サブエージェントが resource_exhausted で失敗"
variables_tested:
  - name: subagent_type  # Cursor 固有
    values_tested:
      - value: explorer
        result: failure
        error: "resource_exhausted"
      - value: generalPurpose
        result: success
    conclusion: "explorer タイプが原因"
  - name: parallel_execution
    values_tested:
      - value: single
        result: success
      - value: parallel_3
        result: success
    conclusion: "並列実行は問題なし"
root_cause: "subagent_type=explorer が resource_exhausted を引き起こす"
recommendation: "generalPurpose タイプを使用"
```

## 使用方法

問題発生時に以下のように呼び出す（Cursor の例）:

```
問題: サブエージェントが失敗する
変数候補:
- subagent_type: explorer, generalPurpose, code-reviewer（Cursor 固有）
- model: fast, default
- parallel: true, false

上記の変数を一つずつ検証し、原因を特定してください。
```

注: Claude Code の場合、`subagent_type` ではなくカスタムエージェントの設定（tools, model, permissionMode 等）が変数候補となる。

## 制限事項

- 間欠的なバグには効果が薄い
- 変数の特定には問題領域の知識が必要
- 再現不可能な問題には適用困難

## 理論的背景

- Scientific Method（科学的方法）
- Binary Search / Divide and Conquer（二分探索）
- 5 Whys（根本原因分析）

## 参照

- research/investigations/2026-01-26_systematic-debugging-methodology.md
- [research/references/superpowers-knowhow.md](../../research/references/superpowers-knowhow.md) - Systematic Debugging の詳細（Root Cause Tracing, 3+ 失敗時のアーキテクチャ疑義など）
