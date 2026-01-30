# 貢献ガイド（Contributing）

agent-harness への貢献方法を説明します。

## 貢献の種類

### 1. 設計原理（P1-P4）への改善

`source/_shared/principles.yaml` を編集します。

- 新しい原理の提案: Issue で議論してから PR
- 既存原理の改善: 直接 PR

### 2. ルールの追加・改善

`source/meta/rules/*.yaml` を編集します。

- 新しいルールの追加: 既存のルールファイルに追加
- 新しいルールカテゴリ: 新しい YAML ファイルを作成

### 3. スキル/サブエージェントの追加

`source/meta/skills/*.md` または `source/meta/agents/*.md` を作成します。

### 4. Guardian プリセットの追加

`source/distribution/guardians/presets/*.yaml` を作成します。

## 貢献フロー

```bash
# 1. Fork
gh repo fork kocchi/agent-harness

# 2. Clone
git clone https://github.com/{your-username}/agent-harness.git
cd agent-harness

# 3. ブランチを作成
git checkout -b feat/your-feature

# 4. 変更を加える
# ...

# 5. コミット
git add .
git commit -m "feat: add new feature"

# 6. Push
git push origin feat/your-feature

# 7. PR を作成
gh pr create
```

## コミット規約

```
feat: 新機能
fix: バグ修正
docs: ドキュメント
refactor: リファクタリング
```

## レビュー観点

- 設計原理（P1-P7）との整合性
- 汎用性（組織固有ではないか）
- 検査可能性（テストや検証ができるか）

---

## 届け方：GitHub Template チェーン

agent-harness は GitHub Template を使って段階的にカスタマイズできます。

```
agent-harness（OSS本体）
    ↓ submodule
agent-harness-template（GitHub Template）
    ↓ "Use this template"
{org}-agent-harness（組織のプラットフォーム）
    ↓ "Use this template"
{team}-agent-harness（チームのストリーム）
```

### L0: agent-harness（OSS本体）

- 設計原理（P1-P7）
- 共通ルール
- 基本スキル/エージェント
- Guardian プリセット

### L1: {org}-agent-harness（組織レベル）

- 組織固有のルール（P8以降）
- セキュリティポリシー
- コンプライアンス要件
- 共通インフラ設定

### L2: {team}-agent-harness（チームレベル）

- チーム固有のルール
- ドメイン知識
- リポジトリ Guardian

### 貢献先の判断

| 変更の性質 | 貢献先 |
|-----------|--------|
| 汎用的な原理・ルール | L0（agent-harness） |
| 組織固有のポリシー | L1（{org}-agent-harness） |
| チーム固有の知識 | L2（{team}-agent-harness） |
