---
name: follow-upstream
description: |
  Fork した agent-harness に upstream の変更を取り込むスキル。
  使用タイミング: 上流の更新を取り込みたいとき、「upstream をマージして」と言われたとき。
---

# Follow Upstream Skill

Fork から upstream（kocchi/agent-harness）の変更を取り込む。

## トリガー

- 「upstream をマージして」「上流の変更を取り込んで」
- 月 1 回の定期追従（Product Goal Q2 の success_metrics）
- upstream に重要な更新があったとき

## 前提条件

- 本リポジトリが kocchi/agent-harness の Fork である
- ローカルに未コミットの変更がない（または stash 済み）

## 実行手順

### 1. upstream を remote に追加（初回のみ）

```bash
git remote add upstream https://github.com/kocchi/agent-harness.git
# 既に存在する場合はスキップ
git remote -v  # 確認
```

### 2. upstream を fetch

```bash
git fetch upstream
```

### 3. 現在のブランチを確認

```bash
git branch --show-current
# 通常は master または main
```

### 4. merge を実行

```bash
git merge upstream/master
# または upstream/main（upstream のデフォルトブランチに合わせる）
```

### 5. コンフリクトが発生した場合

```yaml
方針:
  - チーム固有の変更を優先する
  - OSS の変更は .cursor/vendor/ や境界内に閉じ込める設計であれば、コンフリクトしにくい
  - コンフリクト解消後: git add . && git commit -m "merge: upstream を取り込み"
```

**コンフリクト解消のガイド**:
- `git status` でコンフリクトファイルを確認
- 各ファイルを編集し、`<<<<<<<`, `=======`, `>>>>>>>` を解消
- 解消後 `git add <file>` でステージング
- 全て解消したら `git commit`（メッセージは自動生成される）

### 6. プッシュ

```bash
git push origin master
```

## 頻度

Product Goal Q2: 月 1 回以上を目標

## 失敗時の振る舞い

| 失敗 | 対応 |
|------|------|
| コンフリクト解消不能 | 人間にエスカレート。該当ファイルを特定し、判断を求める |
| upstream が存在しない | `git remote add upstream` を実行 |
| マージ後に動作不良 | rules-validator を実行して整合性を確認 |

## 参照

- [contribute-upstream](../contribute-upstream/SKILL.md) - OSS への貢献（逆方向）
- [product-goal-contract-Q2](../../../contracts/product-goal-contract-Q2.yaml)
