---
name: contribute-upstream
description: OSS本体（agent-harness）への貢献をガイドする
trigger:
  - "agent-harness を改善したい"
  - "OSS に貢献したい"
  - "上流に PR を送りたい"
---

# contribute-upstream

OSS本体（agent-harness）への貢献をガイドするスキル。

## 前提

- agent-harness は直接編集運用（`.cursor/` 内を直接編集）
- Fork して PR を送る標準的な OSS 貢献フロー

## 対話フロー

### 1. 変更したい内容を確認

```
どのような変更をしたいですか？
- 設計原理（constitution の P0-P7）の改善
- ルール（.cursor/rules/*.mdc）の追加・改善
- スキル（.cursor/skills/）の追加
- エージェント（.cursor/agents/）の追加
- 契約テンプレートの改善
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

該当するファイルを直接編集:

| 変更種別 | 対象パス |
|----------|----------|
| 設計原理 | `.cursor/rules/constitution.mdc` |
| ルール | `.cursor/rules/*.mdc` |
| スキル | `.cursor/skills/{name}/SKILL.md` |
| エージェント | `.cursor/agents/*.md` |
| セッション | `.cursor/sessions/*.yaml` |
| 契約 | `contracts/*.yaml`（テンプレートとして） |

### 6. ルール検査を実行

```bash
# rules-validator を呼び出して整合性チェック
# timing: pre_commit
```

### 7. コミットして Push

```bash
git add .
git commit -m "feat: add new feature"
git push origin feat/your-feature
```

### 8. PR を作成

```bash
gh pr create --title "feat: add new feature" --body "説明..."
```

## 注意事項

- 組織固有の内容は OSS に含めない
- 汎用的な改善のみを PR する
- `.cursor/` と `.claude/` は symlink で同期されている

## 参照

- [AGENTS.md](../../AGENTS.md)
- [philosophy.md](../../docs/philosophy.md)
- [core-rules.mdc](../rules/core-rules.mdc)
