# Algorithm Experiment Manager Skill

This directory contains the Codex Skill bundled by the `algorithm-experiment-manager` plugin.

Install the plugin from the repository root:

```bash
codex plugin marketplace add "$(pwd)"
codex plugin add algorithm-experiment-manager@aca-zzy
```

After installing, start a new Codex thread and ask:

```text
使用 algorithm-experiment-manager，帮我设计一个算法实验。
```

## Contents

```text
SKILL.md
templates/
  experiment_plan.md
  experiment_record.md
  incremental_log.md
  comparison_table.md
  failure_record.md
  project_index.md
  metric_registry.md
  reproducibility_checklist.md
  paper_evidence_notes.md
examples/
  generic_algorithm_experiment.md
scripts/
  new_experiment.py
  append_log.py
  summarize_experiments.py
```

## Script Usage

Create a new experiment record:

```bash
python3 scripts/new_experiment.py --project GenericML --topic LearningRateTest --date 20260614
```

Append an incremental log:

```bash
python3 scripts/append_log.py \
  --experiment EXP_20260614_GenericML_LearningRateTest_01 \
  --type 参数修改 \
  --content "learning rate 从 1e-3 调整为 5e-4" \
  --result "validation loss 更稳定" \
  --next "保持其他参数不变，继续进行单因素验证"
```

Generate a project index:

```bash
python3 scripts/summarize_experiments.py
```

## Recommended Workflow

1. Use `templates/experiment_plan.md` to define the objective, hypothesis, control group, changed variables, and controlled variables.
2. Record data location, code version, config files, command, environment, and metrics before running.
3. Append an incremental log whenever parameters, code, data, results, or conclusions change.
4. Keep failed experiments in the failure record.
5. Check fairness before comparing experiments.
6. Archive project index, comparison table, reproducibility checklist, and paper evidence notes.
