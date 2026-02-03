# agent-harness

> 自律的に価値を届け続ける AI チームのハーネス

最終判断以外を AI チームに委譲できる。
契約体系で合意を明文化し、ペルソナ注入と相互監視で判断の質を維持。

**進捗**: [STATUS.md](STATUS.md)（ここだけ見れば進捗が追える）

## クイックスタート

```bash
# clone
git clone https://github.com/kocchi/agent-harness.git
cd agent-harness

# Cursor または Claude Code で開く
# ルールが自動適用される
```

## 特徴

- **契約体系**: 価値の合意を明文化（sprint-goal, slice, increment, insight）
- **設計原理（P0-P7）**: AI エージェント活用のベストプラクティス
- **ルール定義**: 直接編集運用、コミット規約、Human-in-the-Loop
- **スキル**: 契約系、プロセス監視、メトリクス

## 構造

```
.cursor/                # AI エージェント設定（正）
├── rules/              # ルール定義 (.mdc)
├── agents/             # サブエージェント定義
├── skills/             # スキル定義
├── sessions/           # セッション定義
└── ledger/             # 学習台帳

contracts/              # 契約ファイル
├── product-goal-*.yaml # プロダクト目標
├── sprint-goal-*.yaml  # スプリント目標
├── slice-*.yaml        # 価値分類
└── insight-*.yaml      # 学び

docs/                   # ドキュメント
├── VISION.md           # プロジェクトビジョン
├── philosophy.md       # 設計原理
├── skills-inventory.md # スキル一覧
└── contributing.md     # 貢献ガイド

.claude/                # → .cursor/ への symlink
```

## 設計原理

| 原理 | 名称 | 概要 |
|------|------|------|
| P0 | セッション開始時の検査 | rules-validator を自動実行 |
| P1 | コンテキスト分離 | 役割ごとにエージェントを分ける |
| P2 | 契約で合意 | コードの前に契約を作る |
| P3 | 道具を減らす | 各スライスで減らせる道具を評価 |
| P4 | 知識を蓄積 | ルール/スキル/ナレッジとして蓄積 |
| P5 | ペルソナ注入 | 著名人の視点を借りる |
| P6 | 相互監視 | 最低2人の異なる視点でレビュー |
| P7 | 契約で価値をつなぐ | Product Goal → Sprint Goal → Increment |

詳細は [docs/VISION.md](docs/VISION.md) を参照。

## 契約体系

```
Product Goal Contract → Sprint Goal Contract → Slice Contract → Increment Contract
                                                    ↓
                                              Insight Contract（学び）
```

## Cursor / Claude Code 対応

このリポジトリは Cursor と Claude Code の両方で動作します。

- `.cursor/` が正（ルール、エージェント、スキル）
- `.claude/` は `.cursor/` への symlink
- `CLAUDE.md` / `AGENTS.md` でエントリポイントを提供

## チーム導入

複数リポジトリ運用のオンボーディング: [docs/onboarding-multi-repo.md](docs/onboarding-multi-repo.md)

## 貢献

1. このリポジトリを Fork
2. 変更を加える
3. PR を作成

詳細は [docs/contributing.md](docs/contributing.md) を参照。

## ライセンス

MIT
