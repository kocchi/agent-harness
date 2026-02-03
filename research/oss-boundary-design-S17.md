# OSS 境界の設計案（S17）

> Product Goal Q2: OSS を境界内に閉じ込め、チーム固有を主にする

## 設計の方向性

- **チームが主**: チームの .cursor/rules/, skills/, agents/ が作業の中心
- **OSS は従**: OSS（agent-harness）は境界内に閉じ込める
- **上流追従**: OSS の更新は境界内のファイルだけに影響し、チーム固有とコンフリクトしない

## 検討した方式

### 方式 A: git submodule（OSS を vendor に配置）

```
.cursor/
├── vendor/
│   └── agent-harness/   # submodule、upstream を追従
│       └── .cursor/
│           ├── rules/
│           ├── skills/
│           └── agents/
├── rules/               # チーム固有
├── skills/
└── agents/
```

**課題**: Cursor は .cursor/rules/ を読み込む。vendor 内の rules をどう読ませるか。.cursor/rules/ に symlink を置くか、include する .mdc が必要。

**S20 調査結果** (cursor-rules-investigation-S20.md): 公式ドキュメントでは `.cursor/rules` 直下またはサブディレクトリが前提。vendor 内パス・symlink・include の明示的記載なし。Remote rules (GitHub import) はサポート。次: symlink の動作検証。

### 方式 B: 編集禁止ルール（現状ベース）

- OSS のファイル（.cursor/rules/, skills/, agents/）は**編集しない**
- チーム固有は .cursor/team/ に追加
- 上流追従時、OSS 部分が更新され、team/ は触らない

**課題**: ユーザーは「OSS を閉じ込める」を選択。チームが主で OSS が従。方式 B は「チームを team に閉じ込める」に近い。

### 方式 C: ハイブリッド（段階的導入）

1. **Phase 1**: follow-upstream で上流追従を確立。境界の設計は後回し
2. **Phase 2**: コンフリクトが多発したら、方式 A を検討
3. **Phase 3**: Cursor の仕様が明確になったら、vendor 配置を具体化

## S17 の結論

- **follow-upstream スキル**で上流追従の手順を確立
- **OSS 境界の具体設計**は、実際の追従でコンフリクト状況を観察してから決定
- 本ドキュメントを research に残し、次 Sprint で検討を継続

## S20 更新（Cursor 仕様調査）

- **調査結果**: [research/cursor-rules-investigation-S20.md](cursor-rules-investigation-S20.md)
- **方式 A の前提**: Cursor は `.cursor/rules` を読む。vendor 内の直接参照はドキュメントに記載なし。symlink の動作は未検証。
- **次の検証**: symlink を置いた場合の動作、Remote rules での OSS import とチーム固有の優先順位

## S23 更新（symlink 検証）

- **実施**: `.cursor/rules/symlink-test.mdc` → `research/symlink-test/placeholder.mdc` の symlink を作成
- **構造**: 作成可能。git で追跡される
- **Cursor の読み込み**: ✅ symlink は有効。Cursor Settings > Rules で表示される
- **結論**: 方式 A（vendor 配置）は symlink で実現可能。OSS を vendor/ に submodule で配置し、.cursor/rules/ から symlink で参照する構成が検討できる

## S25 更新（方式 A の具体手順）

### 方式 A の実装手順（ドラフト）

1. **Fork 側のセットアップ**
   - agent-harness を Fork し、チームリポジトリとしてクローン
   - `git remote add upstream https://github.com/kocchi/agent-harness.git`

2. **vendor への配置**
   ```bash
   # .cursor/vendor/ に agent-harness を submodule として追加
   git submodule add https://github.com/{team}/agent-harness.git .cursor/vendor/agent-harness
   ```

3. **symlink の作成**
   - OSS の rules を参照する symlink を .cursor/rules/ に配置
   - 例: `.cursor/rules/oss-constitution.mdc` → `../vendor/agent-harness/.cursor/rules/constitution.mdc`
   - チーム固有の rules は .cursor/rules/ に直接配置（OSS より優先させる場合はファイル名の並びで制御、または .mdc の順序に依存）

4. **上流追従**
   - follow-upstream スキルで `vendor/agent-harness` を更新
   - コンフリクト時: チーム固有（.cursor/rules/ 直下の非 symlink）を優先

5. **注意事項**
   - Windows では symlink に管理者権限が必要な場合あり。その場合は方式 B を検討
   - symlink は git で追跡される（macOS/Linux）
