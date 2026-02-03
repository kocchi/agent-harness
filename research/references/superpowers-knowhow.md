# Superpowers 系 OSS のノウハウ

> S16 断捨離で削除した superpowers 系スキル（外部 OSS 由来）のノウハウを保持。
> 契約系スキル（slice, increment, propose-change）と併用する際の参照用。

## 1. Brainstorming（創造的作業前）

**トリガー**: 機能追加、コンポーネント作成、挙動変更の**前**

### 原則

- **1 問いずつ**: 複数質問を一度に投げない
- **選択肢を優先**: オープンエンドよりマルチプルチョイス
- **YAGNI を徹底**: 不要な機能は削る
- **代替案を提示**: 2-3 のアプローチとトレードオフを出してから決定
- **段階的検証**: 設計を 200-300 字のセクションに分け、各セクションで確認

### プロセス

1. **理解**: プロジェクト状態を確認 → 1 問ずつ掘り下げ → 目的・制約・成功基準を把握
2. **探索**: 2-3 のアプローチを提案し、推奨と理由を述べる
3. **設計提示**: セクションごとに提示し、「ここまでで問題ないか」を確認
4. **記録**: `docs/plans/YYYY-MM-DD-<topic>-design.md` に保存

### 契約系との接続

- slice 設計の「未知」分類で、brainstorming の 1 問ずつ・代替案提示を適用
- discovery の仮説検証で、2-3 アプローチの比較を活用

---

## 2. Writing Plans（実装計画）

**トリガー**: 仕様・要件がある多ステップタスクの**コード着手前**

### 原則

- **細かいタスク**: 1 ステップ = 2-5 分の 1 アクション
- **具体性**: ファイルパス、コード、コマンドを明示
- **DRY, YAGNI, TDD**: 頻繁にコミット

### タスク構造の例

```markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/existing.py:123-145`

**Step 1: Write the failing test**
**Step 2: Run test to verify it fails**
**Step 3: Write minimal implementation**
**Step 4: Run test to verify it passes**
**Step 5: Commit**
```

### 契約系との接続

- increment-contract の DoD を、この粒度で分解できる
- propose-change の「実装フロー」で、bite-sized タスクを参照

---

## 3. Systematic Debugging（体系的デバッグ）

**トリガー**: バグ、テスト失敗、想定外の挙動の**修正提案前**

### 鉄則

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

症状への対症療法は禁止。必ず根因を特定してから修正。

### 4 フェーズ

| Phase | 内容 | 成功基準 |
|-------|------|----------|
| **1. Root Cause** | エラー精読、再現、変更確認、証拠収集 | WHAT と WHY を理解 |
| **2. Pattern** | 動いている例を探す、参照と比較 | 差分を特定 |
| **3. Hypothesis** | 仮説を立て、最小変更で検証 | 仮説の支持 or 棄却 |
| **4. Implementation** | 失敗テスト作成、修正、検証 | バグ解消、テスト通過 |

### 3 回以上修正が失敗したら

→ **アーキテクチャを疑う**。パターンが根本的に不適切な可能性。人間と議論してから次に進む。

### 支援テクニック

- **Root Cause Tracing**: コールスタックを遡り、不正な値の発生源を特定
- **Defense in Depth**: 根因修正後、複数レイヤーで検証を追加
- **Condition-based Waiting**: 任意のタイムアウトではなく、条件ポーリングで待機

### 契約系との接続

- systematic-debugger エージェントがこのプロセスを実装
- 問題発生時は core-rules で systematic-debugger を呼び出す

---

## 参照元

- 元スキル: superpowers-brainstorming, superpowers-writing-plans, superpowers-systematic-debugging
- 断捨離: S16（参照の不整合により削除。ノウハウは本ドキュメントで保持）
