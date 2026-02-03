# 複数リポジトリ運用オンボーディング

> 1 ストリームアラインドチームが agent-harness で複数リポジトリを運用するための導入ガイド

## 前提

- チーム: 1 ストリームアラインドチーム（Team Topologies）
- リポジトリ: 複数の開発リポジトリを運用
- agent-harness: OSS を Fork して使用し、上流の変更を追従する

## チェックリスト

### Phase 1: セットアップ

- [ ] agent-harness を Fork（GitHub で Fork ボタン）
- [ ] Fork をクローン: `git clone https://github.com/{team}/agent-harness.git`
- [ ] upstream を追加: `git remote add upstream https://github.com/kocchi/agent-harness.git`
- [ ] Cursor または Claude Code でリポジトリを開く

### Phase 2: 複数リポジトリの登録

- [ ] add-repository スキルを実行
- [ ] 開発リポジトリを `repos/` に submodule として追加
- [ ] guardian-generator で Guardian を生成（add-repository が案内）

**repos/ の構造**: agent-harness を親リポジトリとし、開発リポジトリを submodule で管理。`repos/{repo-name}/` に配置され、Guardian は `_config/guardians/repos/{repo-name}.yaml` に生成される。

### Phase 3: 契約フローの開始

- [ ] product-goal-contract を確認・更新
- [ ] Sprint Planning を開催（persona-council に委譲）
- [ ] sprint-goal-contract を作成
- [ ] slice-contract でスライスを定義

### Phase 4: 運用

- [ ] セッション開始時: workload-monitor でワークロード確認
- [ ] Sprint 終了時: Review/Retro を persona-council で開催
- [ ] 月 1 回: follow-upstream で upstream の変更を取り込む

## 参照スキル

| スキル | 用途 |
|--------|------|
| add-repository | 開発リポジトリの追加 |
| follow-upstream | upstream の変更を取り込み |
| contribute-upstream | OSS への貢献（改善を PR） |
| sprint-goal | Sprint Planning |
| workload-monitor | ワークロード監視 |

## トラブルシューティング

| 問題 | 対処 |
|------|------|
| コンフリクトが解消できない | follow-upstream のコンフリクト解消ガイドを参照。チーム固有を優先 |
| どのスキルを使うかわからない | workload-monitor の推奨に従う |
| 場をスキップしたくなった | implementation-rules: 場の開催は必須。persona-council に委譲する |
