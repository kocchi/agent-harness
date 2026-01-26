---
name: contribute-upstream
description: OSS本体（agent-harness）への貢献をガイドする
trigger:
  - "source/ を改善したい"
  - "OSS に貢献したい"
  - "上流に PR を送りたい"
---

# contribute-upstream

OSS本体（agent-harness）への貢献をガイドするスキル。

## 前提

- source/ は submodule として参照されているため、直接編集できない
- OSS本体への貢献は、別途 Fork して作業する必要がある

## 対話フロー

### 1. 変更したい内容を確認

```
どのような変更をしたいですか？
- 設計原理（P1-P4）の改善
- ルールの追加・改善
- スキル/サブエージェントの追加
- Guardian プリセットの追加
```

### 2. OSS本体を Fork

```bash
# GitHub CLI を使用
gh repo fork kocchi/agent-harness

# または GitHub UI で Fork ボタンをクリック
```

### 3. Fork をクローン

```bash
git clone https://github.com/{your-username}/agent-harness.git
cd agent-harness
```

### 4. ブランチを作成

```bash
git checkout -b feat/your-feature
```

### 5. 変更を加える

該当するファイルを編集:
- 設計原理: `source/_shared/principles.yaml`
- ルール: `source/meta/rules/*.yaml`
- スキル: `source/meta/skills/*.md`
- サブエージェント: `source/meta/agents/*.md`
- Guardian プリセット: `source/distribution/guardians/presets/*.yaml`

### 6. コミットして Push

```bash
git add .
git commit -m "feat: add new feature"
git push origin feat/your-feature
```

### 7. PR を作成

```bash
gh pr create --title "feat: add new feature" --body "説明..."
```

## 注意事項

- 組織固有の内容は OSS に含めない
- 汎用的な改善のみを PR する
- P5 以降の原理は `_config/org/` で定義する
