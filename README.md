# agent-harness

> 自律的に価値を届け続ける AI チームのハーネス

著名人の人格を降ろした複数の AI が、相互監視しながら動く。
いつ、何を、どう動くか。型を一気通貫で提供。

## クイックスタート

```bash
# clone
git clone https://github.com/kocchi/agent-harness.git
cd agent-harness

# Cursor または Claude Code で開く
# ルールが自動適用される
```

## 特徴

- **設計原理（P1-P6）**: AI エージェント活用のベストプラクティス
- **ルール定義**: 直接編集運用、コミット規約、Human-in-the-Loop
- **スキル**: brainstorming, systematic-debugging, writing-plans
- **サブエージェント**: rules-validator, deep-research, systematic-debugger

## 構造

```
.cursor/                # AI エージェント設定（正）
├── rules/              # ルール定義 (.mdc)
├── agents/             # サブエージェント定義
└── skills/             # スキル定義

.claude/                # → .cursor/ への symlink

docs/                   # ドキュメント
├── VISION.md           # プロジェクトビジョン
├── philosophy.md       # 設計原理（P1-P4）
├── contributing.md     # 貢献ガイド
└── team-topologies.md  # チームトポロジー

source/                 # 参照用定義（YAML）
├── _shared/            # 汎用原理
└── meta/               # ルール・スキル・エージェント（YAML）
```

## 設計原理

| 原理 | 名称 | 概要 |
|------|------|------|
| P1 | コンテキスト競合の排除 | 役割分離された複数サブエージェントの同時稼働を前提 |
| P2 | 中間生成物で合意を運ぶ | 契約・失敗モード・テスト戦略で検査可能に |
| P3 | 道具を減らす | 各スライスで「減らせる道具」を評価 |
| P4 | 知識は資産 | ルール/スキル/ナレッジとして蓄積 |
| P5 | FIC | 摩擦を注入し、A/B では到達できない C を生む |
| P6 | 相互監視 | 最低2人（ベスト3人）で自律的に動く |

詳細は [docs/VISION.md](docs/VISION.md) を参照。

## Cursor / Claude Code 対応

このリポジトリは Cursor と Claude Code の両方で動作します。

- `.cursor/` が正（ルール、エージェント、スキル）
- `.claude/` は `.cursor/` への symlink
- `CLAUDE.md` / `AGENTS.md` でエントリポイントを提供

## 貢献

1. このリポジトリを Fork
2. 変更を加える
3. PR を作成

詳細は [docs/contributing.md](docs/contributing.md) を参照。

## ライセンス

MIT
