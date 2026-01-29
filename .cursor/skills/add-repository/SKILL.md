---
name: add-repository
description: 開発リポジトリを submodule として追加し、Guardian を生成する
trigger:
  - "リポジトリを追加"
  - "repos/ に追加"
  - "新しいリポジトリを登録"
---

# add-repository

開発リポジトリを submodule として `repos/` に追加し、guardian-generator で Guardian を生成するスキル。

## 入力

- リポジトリ URL（GitHub URL など）または ローカルパス

## 出力

- `repos/{repo-name}/` に submodule 追加
- `_config/guardians/repos/{repo-name}.yaml` に Guardian 生成

## 対話フロー

### 1. リポジトリ情報の取得

```
追加したいリポジトリを教えてください。

例:
- GitHub URL: https://github.com/org/repo.git
- ローカルパス: /path/to/repo
```

### 2. リポジトリ名の確認

```bash
# URL からリポジトリ名を抽出
# 例: https://github.com/org/repo.git → repo
```

ユーザーに確認:
```
リポジトリ名: {repo_name}
追加先: repos/{repo_name}/

この内容で追加しますか？
```

### 3. submodule として追加

```bash
# repos/ に submodule として追加
git submodule add {url} repos/{repo_name}

# 初期化
git submodule update --init --recursive repos/{repo_name}
```

### 4. Guardian 生成

guardian-generator サブエージェントを呼び出す:

```
guardian-generator を呼び出して repos/{repo_name} の Guardian を生成してください。

入力: repos/{repo_name}
出力: _config/guardians/repos/{repo_name}.yaml
```

### 5. 完了報告

```
## 追加完了

- submodule: repos/{repo_name}/
- Guardian: _config/guardians/repos/{repo_name}.yaml

### 次のステップ
1. Guardian の内容を確認・調整
2. 必要に応じて _config/guardians/repos/{repo_name}.yaml を編集
3. ./scripts/generate.sh を実行
```

## 失敗モード対策

| 失敗 | 対策 |
|------|------|
| git submodule 追加失敗 | エラーメッセージで原因を明示（権限、URL形式など） |
| guardian-generator 呼び出し失敗 | 手動で guardian-generator を実行するようガイド |
| 権限不足（SSH/HTTPS） | SSH/HTTPS の切り替えを提案 |
| 既に存在する | 既存の submodule を確認するようガイド |

## 注意事項

- submodule 追加後は `git add` と `git commit` が必要
- Guardian は提案であり、ユーザーがカスタマイズ可能
- 機密リポジトリの場合は SSH URL を使用
