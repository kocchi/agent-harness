# Sprint Retro S22 - 2026-05-20
# Place: sprint_retro
# Sprint: S22

## 入力

- 上記 Sprint Review の議論
- insight-contract-S21

## 分析（Step 1）

```yaml
analysis:
  focus: "release-notes 習慣化のプロセス"
  risk: "Sprint 終了日に workload-monitor を実行し忘れる"
  context: "S22 で release-notes を実行し、Review で materials に含めた。初回検証成功"
  mental_model_trap: "仕組みが動いたからもう大丈夫"
```

## 召喚条件（Step 2）

```yaml
summon_criteria:
  critical: "プロセスの盲点を探す批判的視点。"
  learning: "学びを次のアクションに落とし込むことを重視する人物。"
  constraint: "LLM の内部知識から選出。実名で記録する。"
```

## 議論（Step 3）

### 人格選定

- **Esther Derby**（Agile Retrospectives 共著者）
- **Diana Larsen**（Agile Retrospectives 著者）

### 議論の過程

| speaker | statement |
|---------|-----------|
| Esther Derby | S22 のプロセス: release-notes は「Sprint 終了時に実行する」がトリガー。誰がそれを思い出すか。workload-monitor の推奨に含まれているが、workload-monitor 自体を実行する習慣が必要。 |
| Diana Larsen | 学び: S22 で release-notes が実行されたのは、自律的に進めた結果。Sprint 終了日の「やること」に release-notes が含まれていれば、忘れにくい。preserve: Sprint 終了日のチェックリストに「release-notes 実行」を明示する。 |
| Esther Derby | start_doing: 特になし。S21 で出尽くしている。preserve を強化: release-notes の実行を Sprint 終了日のルーティンに含める。 |

### 収束

S22 の学びは軽微。preserve を強化。S23 で習慣化の継続を検証する。

## 統合（Step 4）

```yaml
integration:
  stop_doing: []
  start_doing: []
  preserve:
    - "Sprint 終了日に release-notes を実行するルーティン"
    - "workload-monitor の推奨（Sprint 終了時は release-notes → Review）"
  next_hypothesis: "S23 で release-notes が継続されれば、習慣化と判断できる"
```

## 出力

- insight-contract-S22（Retro の学びを反映）
