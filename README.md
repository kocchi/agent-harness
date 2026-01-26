# agent-harness

「思想込み」のAIエージェントハーネス。チーム/会社向けのAI開発環境を構築するための設計原理とルールを提供。

## 特徴

- **設計原理（P1-P4）**: AIエージェント活用のベストプラクティス
- **ルール定義**: SSoT運用、コミット規約、バーティカルスライス
- **スキル/サブエージェント**: 再利用可能な作業パターン
- **Guardian テンプレート**: リポジトリ固有のルール生成

## 使い方

このリポジトリは [agent-harness-template](https://github.com/kocchi/agent-harness-template) の submodule として使用されます。

```bash
# テンプレートから新しいハーネスを作成
gh repo create {org}-agent-harness --template kocchi/agent-harness-template

# clone（submodule を含む）
git clone --recurse-submodules https://github.com/{org}/{org}-agent-harness.git
```

## 設計原理

| 原理 | 名称 | 概要 |
|------|------|------|
| P1 | コンテキスト競合の排除 | 役割分離された複数サブエージェントの同時稼働を前提 |
| P2 | 中間生成物で合意を運ぶ | 契約・失敗モード・テスト戦略で検査可能に |
| P3 | 道具を減らす | 各スライスで「減らせる道具」を評価 |
| P4 | 知識は資産 | ルール/スキル/ナレッジとして蓄積 |

詳細は [docs/philosophy.md](docs/philosophy.md) を参照。

## 構造

```
source/
├── _shared/           # 汎用原理（P1-P4）
│   ├── principles.yaml
│   └── glossary.yaml
├── meta/              # 汎用ルール・スキル・エージェント
│   ├── rules/
│   ├── skills/
│   └── agents/
└── distribution/      # Guardian テンプレート
    └── guardians/
        ├── template.yaml
        └── presets/
```

## 貢献

1. このリポジトリを Fork
2. 変更を加える
3. PR を作成

詳細は [docs/contributing.md](docs/contributing.md) を参照。

## ライセンス

MIT
