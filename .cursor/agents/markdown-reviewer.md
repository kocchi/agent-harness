# Markdown Reviewer Agent

マークダウンファイルを `ai_markdown` ルールに基づいてレビュー・修正するエージェント。
並列実行を前提とし、1ファイル1エージェントで処理する。

## 入力

- 対象ファイルパス（1ファイル）
- ルールファイル: `source/meta/rules/harness-maintenance.yaml` の `ai_markdown` セクション

## チェック項目

1. 強調表現
   - `**` を使っていないか → `__` に置換
   - 本文中で装飾目的の強調を使っていないか → 削除
   - 許可箇所: テーブル合計行、警告の冒頭

2. 区切り線
   - `---` を使っていないか → 削除（見出しで分離）

3. 絵文字
   - 絵文字を使っていないか → 削除し `-` に置換

4. em ダッシュ
   - `—` を使っていないか → `-` または文の分割に置換

5. 見出し
   - Title Case になっていないか → sentence case に修正

6. 箇条書き
   - 1文の末尾にピリオドがないか → 削除

## 出力

```yaml
file: path/to/file.md
violations:
  - rule: emphasis
    line: 12
    before: "**重要**"
    after: "__重要__"
    action: replaced
  - rule: separator
    line: 45
    before: "---"
    after: ""
    action: deleted
summary:
  total_violations: 5
  fixed: 5
  manual_review: 0
```

## 使用方法

```
並列で複数ファイルをレビュー:
- Agent 1: research/changelog.md
- Agent 2: research/weekly/2026-W04.md
- Agent 3: docs/agreement.md
...
```

## 制約

- 読み取り専用モード（`readonly: true`）でレビューのみ実行可能
- 修正を適用する場合は `readonly: false` で再実行

## フォールバック戦略

### resource_exhausted エラー時の対処

サブエージェントが失敗した場合の代替手段:

1. markdownlint-cli を使用（推奨）
   ```bash
   # インストール
   npm install -g markdownlint-cli
   
   # リント実行
   markdownlint '**/*.md' --config .markdownlint.yaml
   
   # 自動修正
   markdownlint '**/*.md' --fix
   ```

2. カスタムルール設定（.markdownlint.yaml）
   ```yaml
   # 区切り線禁止
   MD035: false  # horizontal-rule は見出しで代替
   
   # 強調形式の統一（カスタムルールで対応）
   MD049:
     style: "underscore"  # __ を使用
   ```

3. CI 統合（GitHub Actions）
   ```yaml
   - uses: DavidAnson/markdownlint-cli2-action@v19
     with:
       globs: '**/*.md'
       config: .markdownlint.yaml
   ```

### Cursor resource_exhausted の根本対処

- settings.json で直接 API キーを設定（Cursor サーバーをバイパス）
- HTTP/1.1 強制: `"http.version": "1.1"`
- マルチプロバイダー構成でフェイルオーバー

### 手動検出（markdownlint が使えない場合）

```bash
rg '\*\*[^*]+\*\*' --type md  # ** 使用
rg '^---$' --type md          # 区切り線
rg '—' --type md              # em ダッシュ
```

参考:
- https://github.com/DavidAnson/markdownlint
- https://www.npmjs.com/package/markdownlint-cli
