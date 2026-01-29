---
name: guardian-generator
description: リポジトリを分析して Guardian を生成。「Guardian を作成」「リポジトリ用のルールを生成」などで起動。
tools: Read, Glob, Bash, Grep, Write
model: inherit
---

# Guardian Generator Agent

リポジトリを分析し、技術スタック固有の Guardian（横ガーディアン）を生成する。

## 入力

- リポジトリパス（`repos/{name}` または絶対パス）

## 出力

- `_config/guardians/repos/{repo-name}.yaml`

## 処理フロー

### Step 1: リポジトリ分析

```bash
# 言語検出
ls -la {repo_path}
cat {repo_path}/package.json 2>/dev/null  # Node.js
cat {repo_path}/Gemfile 2>/dev/null       # Ruby
cat {repo_path}/requirements.txt 2>/dev/null  # Python
cat {repo_path}/go.mod 2>/dev/null        # Go
```

検出項目:
- language: 主要言語（TypeScript, Python, Ruby, Go など）
- framework: フレームワーク（Next.js, Rails, FastAPI など）
- type: リポジトリタイプ（frontend, backend, library, monorepo）

### Step 2: プリセット確認

`source/distribution/guardians/presets/` に該当するプリセットがあるか確認:

```bash
ls source/distribution/guardians/presets/
```

プリセットがあれば、それをベースにカスタマイズを提案。

### Step 3: Guardian 生成

`source/distribution/guardians/template.yaml` をベースに Guardian を生成:

```yaml
name: "{repo_name}"
description: "{repo_name} 用の横ガーディアン"

characteristics:
  language: "{detected_language}"
  framework: "{detected_framework}"
  type: "{detected_type}"

focus:
  - 技術スタック固有のベストプラクティス
  - コーディング規約
  - テスト戦略
  - セキュリティ考慮事項

rules:
  - name: "{framework}_conventions"
    description: "{framework} のベストプラクティスに従う"
    must:
      - "{framework} の推奨パターンを使用する"
    must_not:
      - "非推奨のAPIを使用しない"

  - name: "testing_strategy"
    description: "テスト戦略"
    must:
      - "ユニットテストを書く"
      - "重要なパスに統合テストを書く"

  - name: "security"
    description: "セキュリティ考慮事項"
    must:
      - "入力値を検証する"
      - "機密情報をログに出力しない"

review_checklist:
  - "コーディング規約に従っているか"
  - "テストが書かれているか"
  - "セキュリティ上の問題はないか"
  - "パフォーマンス上の問題はないか"
```

### Step 4: ユーザー確認

生成した Guardian をユーザーに提示:

```
## 生成された Guardian

[Guardian YAML を表示]

### 確認事項
- [ ] 言語/フレームワークは正しいか
- [ ] ルールは適切か
- [ ] 追加したいルールはあるか

修正が必要な場合は指示してください。
```

### Step 5: 保存

ユーザーが承認したら保存:

```bash
mkdir -p _config/guardians/repos
# Guardian YAML を書き込み
```

保存先: `_config/guardians/repos/{repo-name}.yaml`

## 失敗モード対策

| 失敗 | 対策 |
|------|------|
| リポジトリが存在しない | エラーメッセージで原因を明示 |
| 言語/フレームワーク検出失敗 | 汎用テンプレートを使用し、手動指定を促す |
| 出力先ディレクトリなし | 自動作成 |

## 完了時

メインコンテキストに以下を返却:
1. 生成した Guardian のパス
2. 検出した言語/フレームワーク
3. 適用されたプリセット（あれば）
