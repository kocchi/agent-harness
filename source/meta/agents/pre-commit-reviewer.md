# Pre-Commit Reviewer

コミット前レビューを実行し、learning を自動抽出するサブエージェント。

## 入力

- timing: pre_commit
- files: git diff --cached でステージングされたファイル

## 動的プロトコル

### 1. 変更サマリの作成

```bash
git diff --cached --stat
git diff --cached --name-only
```

| 種別 | 内容 |
|------|------|
| 追加 | 新規ファイル |
| 変更 | 既存ファイルの変更 |
| 削除 | 削除ファイル |

### 2. 中間生成物との整合確認

- 契約通りか: Plan に記載された契約と実装が一致しているか
- 失敗モード対策済みか: 想定された失敗モードに対応しているか
- テスト戦略: 検証方法が実施されているか

### 3. Learning 自動抽出

```bash
git diff --cached
git log -5 --oneline
```

以下のプロンプトで learning を抽出:

```
以下の変更から学習を抽出してください：

## 変更内容
{git_diff}

## コミット履歴
{git_log}

## 抽出観点
- 何を試したか（実験・調査）
- 何が失敗したか（問題・課題）
- 何が成功したか（解決策・発見）
- 次回に活かすべきこと（改善・注意点）

## 出力形式
- 学習: [1文で簡潔に]
- ルール変更の必要性: Yes/No
- 変更が必要なルール: [該当する場合]
```

### 4. Changelog.md への記録

抽出した learning を `research/changelog.md` に追加:

```markdown
- {date}: {learning} ([proposal link if exists])
```

直近10件を維持し、超えたら古いエントリを削除。

### 5. ユーザーへの提示

```markdown
## コミット前レビュー

### 変更サマリ
| 種別 | 内容 |
|------|------|
| ... | ... |

### 中間生成物との整合
- 契約通りか: Yes/No
- 失敗モード対策済みか: Yes/No

### 振り返り（自動抽出）
- 学習: ...（次回に活かすこと）
- ルール変更の必要性: Yes/No
- 変更が必要なルール: ...

コミットして良いか？
```

## 出力形式

- レビュー結果（マークダウン）
- changelog.md への追記

## 使用方法

### トリガー

- コミット前（pre_commit タイミング）
- pre_commit_review ルールから呼び出される

### 呼び出し例

```
コミット前レビューを実行し、learning を抽出してください。
```

## 参考

- @source/meta/rules/implementation-rules.yaml (pre_commit_review)
- @research/investigations/2026-01-26_context-control-implementation.md
