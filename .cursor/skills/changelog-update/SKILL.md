---
name: changelog-update
description: changelog.md を「直近変更の一覧」として管理する
---

# Changelog 更新

`research/changelog.md` は __直近10件の変更一覧__ として機能する。
詳細は `research/proposals/` に記録する。

## 構造

```
research/
├── changelog.md          ← テーブル形式、直近10件
└── proposals/
    └── YYYY-MM-DD_xxx.md ← 詳細な変更理由
```

## 更新フロー

### 1. proposal を作成

`research/proposals/YYYY-MM-DD_タイトル.md` を作成:

```markdown
# プロポーサル: タイトル

提案日: YYYY-MM-DD
状態: 提案中 / 実装中 / 完了

## 背景

- 問題
- 根本原因

## 解決策

- 変更内容

## 学習

- 得られた知見
```

### 2. changelog にリンクを追加

`research/changelog.md` のテーブルに追加:

```markdown
| YYYY-MM-DD | タイトル | 状態 | [proposals/YYYY-MM-DD_xxx.md](proposals/YYYY-MM-DD_xxx.md) |
```

### 3. 古いエントリを削除

10件を超えたら、古いものを削除。
詳細は proposals に残るため追跡可能。

## 対象

__全ての変更__ を記録する:

| 対象 | 例 |
|------|-----|
| ルール変更 | 新規ルール追加、既存ルール修正 |
| エージェント変更 | 新規エージェント、プロトコル変更 |
| スキル変更 | 新規スキル、フロー変更 |
| 構造変更 | ディレクトリ構成、生成フロー |
