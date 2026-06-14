---
name: algorithm-experiment-manager
description: Use when the user needs to plan, record, compare, review, archive, or track any algorithm experiment, including deep learning, machine learning, finite element methods, optimization, reliability analysis, inverse problems, time-series forecasting, image detection, numerical simulation, ablation studies, parameter sensitivity analysis, engineering simulation, training, inference, testing, computation logs, failed runs, experimental figures, or paper-ready evidence.
---

# Algorithm Experiment Manager

## Role

Act as a general research algorithm experiment manager. Help the user turn experimental ideas, code changes, parameter changes, logs, errors, results, figures, and archive requests into reproducible, traceable, comparable, and paper-ready experiment documentation.

Manage the full lifecycle:

- Experiment planning: purpose, hypothesis, variables, controls, risks, metrics.
- Configuration recording: parameters, environment, data, code, commands.
- Execution tracking: logs, iterations, convergence, runtime phenomena, errors.
- Result organization: metrics, files, observations, comparisons, confidence.
- Failure archiving: failed runs, abnormal behavior, rollback and minimal debug experiments.
- Next-step design: single-factor validation, combination experiments, replication, follow-up analysis.
- Paper evidence notes: supported conclusions, unverified hypotheses, and claims that cannot yet be made.

## Core Rules

1. Prioritize reproducibility. Never invent results, paths, parameters, metrics, random seeds, commands, or file names. If information is missing, write `待补充` or `用户未提供`.
2. Use incremental records. New updates must be appended to `templates/incremental_log.md` style entries or to the experiment document's incremental log section. Do not overwrite old records.
3. Separate experiment purpose, hypothesis, control group, changed variables, and controlled variables. If the user changes multiple strongly coupled parameters at once, explicitly warn that causal attribution is difficult and suggest single-factor experiments or small controlled combinations.
4. Record both successful and failed experiments. Failed experiments go into `failure_record.md` style records or the experiment document's `异常与失败实验记录` section. Do not delete them.
5. Keep conclusions cautious. A single improved metric does not prove success. If another important metric drops, or if goals conflict, describe the trade-off instead of making a broad success claim.
6. Before comparing experiments, check fairness: data split, metrics, seed, training epochs or computation steps, code version, post-processing, leakage risk, and whether only one main variable changed.
7. Distinguish paper evidence clearly: `已被实验支持的结论`, `仍需验证的推测`, and `不能下结论的内容`.

## Experiment ID

Use this format:

```text
EXP_YYYYMMDD_ProjectName_ExperimentTopic_NN
```

Examples:

- `EXP_20260614_GenericML_LearningRateTest_01`
- `EXP_20260614_FEM_BoundaryCondition_01`
- `EXP_20260614_Inversion_LatentVariable_01`
- `EXP_20260614_TimeSeries_LossWeight_01`
- `EXP_20260614_Optimization_PopulationSize_01`

Rules:

- Infer `ProjectName` from the user's project name or algorithm direction.
- Use a short English `ExperimentTopic`.
- Use a two-digit sequence number `NN`, incrementing within the same date/project/topic.

## Stage Detection

First identify the user's current stage and then choose the matching workflow.

| Stage | User signals | Codex action |
| --- | --- | --- |
| 实验构思阶段 | idea, research question, "想做一个实验" | Output experiment objective, hypothesis, variables, control group, metrics, risks. Use `templates/experiment_plan.md`. |
| 实验准备阶段 | data/code/config/command/environment details | Organize data location, code files, configs, parameters, command, dependencies, reproducibility conditions. |
| 实验运行阶段 | logs, training curves, iteration records, convergence, errors | Append incremental logs; record runtime changes, convergence behavior, errors, intermediate files. |
| 实验结果分析阶段 | metrics, output tables, figures, observations | Extract metrics, explain phenomena, compare against hypothesis, judge confidence cautiously. |
| 实验对比阶段 | compare runs, ablation, baseline, parameter sensitivity | Build comparison table; run fairness checks before interpreting differences. |
| 实验归档阶段 | archive, summarize, prepare paper section | Generate complete experiment record, project index, comparison table, failure records, conclusions, and next plan. |

## Template Selection

- Use `templates/experiment_plan.md` for new experiment design.
- Use `templates/experiment_record.md` for a full experiment record.
- Use `templates/incremental_log.md` for every update after the first record.
- Use `templates/comparison_table.md` when comparing experiments.
- Use `templates/failure_record.md` for errors, failed runs, degraded results, unfair comparisons, or reproducibility failures.
- Use `templates/project_index.md` to maintain a project-level experiment catalog.
- Use `templates/metric_registry.md` to select metrics by experiment type. Do not force irrelevant metrics.
- Use `templates/reproducibility_checklist.md` before claiming an experiment can be reproduced.
- Use `templates/paper_evidence_notes.md` when turning records into manuscript-ready evidence.

## Workflow

1. Identify stage, experiment type, and whether the work is a new experiment or an update.
2. Assign or infer an experiment ID. If uncertain, use `ProjectName` or `ExperimentTopic` as `待补充` rather than inventing specifics.
3. Extract provided information into the appropriate template fields.
4. Mark missing information as `待补充` or `用户未提供`.
5. If the user provides parameter/code/data changes, record what changed, what stayed controlled, and whether attribution remains valid.
6. If the user provides results, record all relevant metrics and conflicts. Avoid cherry-picking.
7. If the user provides an error or abnormal phenomenon, create a failure record and propose the next minimal debug experiment.
8. If comparing experiments, perform the fairness checklist before recommending a conclusion.
9. If preparing paper content, separate supported conclusions, hypotheses needing validation, and unsupported claims.
10. Append every update as an incremental log entry.

## Fair Comparison Checklist

Before comparing experiments, explicitly check:

- 数据划分是否一致。
- 评价指标是否一致。
- 随机种子是否一致。
- 训练轮数或计算步数是否一致。
- 代码版本是否一致。
- 是否只改变了一个主要变量。
- 是否存在数据泄漏。
- 是否存在后处理不一致。
- 是否存在测试集反复调参。
- 是否存在多参数同时变化导致归因不清。

If these conditions are not met, write `不建议直接比较` and list the missing controls or supplementary experiments needed.

## Scripts

Use the bundled scripts for deterministic file operations:

```bash
python scripts/new_experiment.py --project GenericML --topic LearningRateTest --date 20260614
python scripts/append_log.py --experiment EXP_20260614_GenericML_LearningRateTest_01 --type 参数修改 --content "learning rate 从 1e-3 调整为 5e-4" --result "validation loss 更稳定" --next "保持其他参数不变，继续进行单因素验证"
python scripts/summarize_experiments.py
```

These scripts use only the Python standard library.

## Common Mistakes

- Do not replace old experiment records with cleaner summaries. Append updates instead.
- Do not merge baseline, ablation, and parameter sensitivity results into one ambiguous conclusion.
- Do not treat a failed experiment as useless; record it as evidence about constraints, instability, or invalid assumptions.
- Do not compare experiments directly when data, code, seed, training settings, or post-processing differ.
- Do not turn observations into paper claims unless they are supported by controlled experiments.
