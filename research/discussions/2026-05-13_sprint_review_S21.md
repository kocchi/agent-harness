# Sprint Review S21 - 2026-05-13
# Place: sprint_review
# Sprint: S21

## 入力

- materials: contracts/sprint-goal-contract-S21.yaml, contracts/slice-contract-S21-release-notes.yaml, **research/releases/S21-release-notes.md**
- Sprint Goal: リリースノートを Sprint 終了時に生成する仕組みを導入し、sprint-review の入力に含める

## 分析（Step 1）

```yaml
analysis:
  focus: "リリースノートの初回導入、success_criteria の確認"
  risk: "リリースノートが形骸化する"
  context: "S21 で全 slice 完了。release-notes スキル新設、workload-monitor 連携、TEMPLATE 作成"
  mental_model_trap: "作ったら終わり、で運用が続かない"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  qa: "検証可能性を厳格に問う人物。DoD や success_criteria の曖昧さを指摘する傾向。"
  value: "価値（何が届いたか、誰が得するか）を重視する人物。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Lisa Crispin**（Agile Testing 著者）: 検証可能性を厳格に問う
- **Mary Poppendieck**（Lean Software Development 著者）: 価値とプロセス改善を重視

### 議論の過程

| speaker | statement |
|---------|-----------|
| Lisa Crispin | success_criteria を確認する。(1) research/releases/S21-release-notes.md が存在する。✓ (2) sprint-review.md の入力に release_notes が追加されている。✓ 両方達成。 |
| Mary Poppendieck | 価値: 今回の Review 自体が、リリースノートを materials に含んでいる。S20 Retro で指摘された「契約だけ渡されて何をレビューすればいいかわからん」問題の解消を、今回から検証できる。メタ検証として、仕組みが機能した。 |
| Lisa Crispin | release-notes スキルは Sprint 終了時に手動実行か、workload-monitor の推奨から呼び出すか。現状は推奨のみ。S22 以降、実際に Sprint 終了時に release-notes が実行されるかが次の検証項目。 |
| Mary Poppendieck | 同意。S21 の success_criteria は満たしている。次 Sprint で release-notes の実行が習慣化されるか検証する。 |

### 収束

S21 の success_criteria は達成。Sprint Goal は満たしている。次 Sprint で release-notes の実行が習慣化されるか検証する。

## 統合（Step 4）

```yaml
integration:
  next_hypothesis: "リリースノートがあると、persona-council のレビュー品質が上がる（S22 で検証）"
```

## 出力

- insight-contract-S21（Review の成果を反映）
