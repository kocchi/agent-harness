# 3層構造（Team Topologies）

agent-harness は Team Topologies に基づく3層構造を採用しています。

## 構造

```
Layer 0: agent-harness (OSS本体)
    ↓ submodule
Layer 0.5: agent-harness-template (GitHub Template)
    ↓ Use this template
Layer 1: {org}-agent-harness (プラットフォームチーム, GitHub Template)
    ↓ Use this template
Layer 2: {team}-agent-harness (ストリームアラインドチーム)
```

## 各層の責任

### Layer 0: OSS本体（agent-harness）

- 汎用的な設計原理（P1-P4）
- 汎用的なルール・スキル・サブエージェント
- Guardian テンプレートと汎用プリセット

### Layer 0.5: テンプレート（agent-harness-template）

- source/ を submodule として参照
- _config/ ディレクトリ構造
- scripts/（generate.sh, update-source.sh）

### Layer 1: プラットフォームチーム

- 組織共通のコーディング規約
- 組織共通のレビュー観点
- 組織の技術スタック向け Guardian プリセット
- 組織固有のサブエージェント/スキル

### Layer 2: ストリームアラインドチーム

- チーム固有のルール・スキル
- 開発リポジトリ（repos/）
- リポジトリ固有の Guardian

## 貢献フロー

| 貢献先 | 方法 |
|--------|------|
| source/（OSS本体） | OSS本体を Fork して PR |
| _config/org/ | プラットフォームリポに手動で対応 |
| _config/team/ | 直接編集 |

## Volatility による分離

Righting Software のメンタルモデルに基づき、変化頻度（Volatility）で分離しています。

| Volatility | 内容 | 管理場所 |
|-----------|------|---------|
| 低 | 設計原理、汎用ルール | source/（submodule） |
| 中 | 組織カスタマイズ | _config/org/ |
| 高 | チームカスタマイズ、Guardian | _config/team/, _config/guardians/ |
| 最高 | 開発リポジトリ | repos/ |

依存関係は一方向: source/ → _config/ → repos/
