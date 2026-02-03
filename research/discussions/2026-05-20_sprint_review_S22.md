# Sprint Review S22 - 2026-05-20
# Place: sprint_review
# Sprint: S22

## 入力

- materials: contracts/sprint-goal-contract-S22.yaml, contracts/slice-contract-S22-release-notes-habit.yaml, **research/releases/S22-release-notes.md**
- Sprint Goal: release-notes の習慣化を検証する

## 分析（Step 1）

```yaml
analysis:
  focus: "release-notes 習慣化の初回検証"
  risk: "1回できただけで習慣化とは言えない"
  context: "S22 で release-notes を実行し、S22-release-notes.md を生成。本 Review で materials に含めている"
  mental_model_trap: "1回成功で習慣化完了と早合点する"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  qa: "検証可能性を厳格に問う人物。"
  value: "価値（何が届いたか）を重視する人物。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Lisa Crispin**（Agile Testing 著者）
- **Mary Poppendieck**（Lean Software Development 著者）

### 議論の過程

| speaker | statement |
|---------|-----------|
| Lisa Crispin | success_criteria を確認。(1) S22-release-notes.md が存在する。✓ (2) 本 Review で materials に含まれている。✓ 両方達成。 |
| Mary Poppendieck | 価値: S21 で作った仕組みが S22 で実際に動いた。release-notes が Sprint 終了時に生成され、Review の入力になった。習慣化の「初回」検証としては成功。ただし、習慣化は 3 回続けて初めて言える、という説もある。S23 で継続できれば、より確信が持てる。 |
| Lisa Crispin | 同意。S22 の success_criteria は満たしている。Sprint Goal は達成。次は S23 で release-notes が継続されるか。 |

### 収束

S22 の success_criteria は達成。Sprint Goal は満たしている。習慣化の確信は S23 の継続で高まる。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "release-notes が 3 Sprint 連続で実行されれば、習慣化と判断できる"
```

## 出力

- insight-contract-S22（Review の成果を反映）
