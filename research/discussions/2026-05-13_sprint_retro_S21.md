# Sprint Retro S21 - 2026-05-13
# Place: sprint_retro
# Sprint: S21

## 入力

- 上記 Sprint Review の議論
- insight-contract-S20（リリースノートを start_doing に追加済み）

## 分析（Step 1）

```yaml
analysis:
  focus: "リリースノートの習慣化、S21 のプロセス"
  risk: "release-notes が推奨されても実行されない"
  context: "S21 で全 slice 完了。Review でリリースノートを materials に含めた初回"
  mental_model_trap: "仕組みができたことで油断する"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  critical: "プロセスの盲点を探す批判的視点。"
  learning: "学びを抽出し、次のアクションに落とし込むことを重視する人物。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Esther Derby**（Agile Retrospectives 共著者）: プロセスの盲点を探す
- **Diana Larsen**（Agile Retrospectives 著者）: 学びを次のアクションに落とし込む

### 議論の過程

| speaker | statement |
|---------|-----------|
| Esther Derby | S21 の盲点: release-notes は workload-monitor の「推奨」に含まれるが、Sprint 終了時に自動で実行されるわけではない。人間が「1」を選ぶか、明示的に release-notes を実行する必要がある。S8 の retro_actions 照合と同様、トリガーが弱い。 |
| Diana Larsen | その指摘は妥当。現状は「Sprint 終了時に workload-monitor が release-notes を推奨する」まで。習慣化には、Sprint 終了日に workload-monitor を実行する習慣が必要。preserve: リリースノートのテンプレートと release-notes スキルは維持。S22 で検証: 実際に release-notes が実行され、Review の materials に含まれたか。 |
| Esther Derby | start_doing は S20 で出尽くしている。今回の追加アクションは不要。preserve を強化する。 |

### 収束

S21 の学びは S20 Retro で出尽くしている。S22 で release-notes の習慣化を検証する。

## 統合（Step 4）

```yaml
integration:
  stop_doing: []
  start_doing: []
  preserve:
    - "リリースノートのテンプレートと release-notes スキル"
    - "Sprint 終了時に workload-monitor を実行する習慣"
  next_hypothesis: "S22 で release-notes が実行され、Review の materials に含まれるか検証"
```

## 出力

- insight-contract-S21（Retro の学びを反映）
