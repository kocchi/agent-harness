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

**課題**: Cursor は .cursor/rules/ を読み込む。vendor 内の rules をどう読ませるか。.cursor/rules/ に symlink を置くか、include する .mdc が必要。Cursor の include サポート要確認。

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
