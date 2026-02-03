# Cursor .cursor/rules 読み込み仕様の調査（S20）

> OSS 境界の設計（方式 A: vendor 配置）の前提を明確にするため

## 調査日

2026-04-30

## 公式ドキュメントからの知見

**出典**: [Cursor Docs - Rules](https://cursor.com/docs/context/rules)

### 読み込みパス

| 項目 | 内容 |
|------|------|
| **ルート** | `.cursor/rules` |
| **形式** | `.md` または `.mdc` |
| **サブディレクトリ** | サポート（例: `frontend/components.md`） |
| **適用方法** | path patterns / 手動 @ メンション / relevance ベース |

### 適用の種類

| 種類 | 説明 |
|------|------|
| Always Apply | 毎セッション適用 |
| Apply Intelligently | Agent が relevance で判断 |
| Apply to Specific Files | globs で指定 |
| Apply Manually | @ メンション時のみ |

### include / 外部参照

- **公式ドキュメント**: 「Can rules reference other rules or files?」が FAQ にあり。詳細は未確認。
- **Remote rules**: GitHub リポジトリから import 可能。sync で自動更新。
- **vendor 内の rules**: ドキュメントに明示なし。`.cursor/rules` 直下またはサブディレクトリが前提。

### 結論（方式 A への影響）

| 方式 A の要素 | 判明事項 |
|---------------|----------|
| vendor/agent-harness/.cursor/rules/ | Cursor は `.cursor/rules` を読む。**vendor 内のパスはドキュメントに記載なし** |
| symlink | `.cursor/rules/` に symlink を置く場合、Cursor が解決するかは未検証 |
| include | .mdc の frontmatter に include 相当の指定があるかは未確認 |

### 次の検証項目

1. `.cursor/rules/` に submodule/vendor 内ファイルへの symlink を置いた場合の動作
2. Remote rules (GitHub) で OSS リポジトリを import した場合、チーム固有との優先順位
3. AGENTS.md のサブディレクトリ対応（「Nested AGENTS.md support」が Improvements に記載）

## S23 検証: symlink の動作（2026-05-20）

**実施内容**:
- `.cursor/rules/symlink-test.mdc` → `research/symlink-test/placeholder.mdc` の symlink を作成
- 構造は作成可能。git でも symlink は追跡される（macOS/Linux）

**確認方法**（手動）:
- Cursor Settings > Rules で `symlink-test` が表示されるか
- Agent が placeholder.mdc の内容を参照しているか（@symlink-test でメンションして応答を確認）

**結果**: ✅ symlink は有効。Cursor Settings > Rules で symlink-test が表示される。方式 A（vendor 配置）は symlink で実現可能。

## oss-boundary-design への反映

→ [oss-boundary-design-S17.md](oss-boundary-design-S17.md) に S20 更新を追記済み
