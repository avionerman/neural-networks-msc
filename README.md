# Report Deliverables

Consolidated report bundle for the PyFlyt fixed-wing DRL study.

## `notebooks/`

- **`3d_visual.ipynb`** — working Colab notebook with all rendered figures inline (fixed version with all bug fixes baked in) **[DONE]**
- **`pyflyt_drl_fixedwing_ppo_and_sac_phase_1.ipynb`** — original main training notebook **[DONE]**
- **`pyflyt_drl_fixedwing_tqc_phase_2.ipynb`** — TQC re-training notebook

## `models/`

- **`models_for_3d.zip`** — 9 SB3 final-step checkpoints (PPO/SAC/TQC, seeds 0–2). Upload to Colab.
- **`best_models_for_3d.zip`** — 9 SB3 best-during-training checkpoints (saved by `EvalCallback` at peak mean reward).

## `aggregated_results/` — the gold: everything aggregated, ready for the report

- **`final_evaluation.csv`** — 300 rows: 30 deterministic episodes per (algo, seed). Columns include `return`, `length`, `smoothness`, `env_complete`, `num_targets_reached`, `collision`, `out_of_bounds`, etc. Source of all aggregates.
- **`metrics_summary.csv`** — per-algorithm aggregate with 95% CI.
- **`final_results_table.csv`** — formatted table, ready to paste into the report.
- **`statistical_tests.csv`** — pairwise Welch's t-test + Mann–Whitney U.
- **`learning_curves.csv`** — timesteps vs. mean reward (3 seeds).
- **`learning_curves.png`** — main figure for sample-efficiency comparison.
- **`metrics_bar_charts.png`** — bar charts: return, success rate, waypoint count, crash rate, OOB rate, smoothness.
- **`random_baseline.csv`** — random-policy baseline (n = 30 episodes).
- **`tqc_runs.json`** — TQC training metadata.

## `training_logs/` — one folder per (algo, seed)

```
PPO_seed0/
...
TQC_seed2/
```

Each folder contains:

- **`eval.monitor.csv`** — per-episode return/length/time during eval (200 episodes per seed).
- **`train.monitor.csv`** — per-episode return/length during training.
- **`evaluations.npz`** — raw eval data used by `EvalCallback`.
- **`best_model.zip`** — best-mean-reward checkpoint.
- **`events.out.tfevents.*`** — TensorBoard logs (rare, not all seeds).

---

## Key Finding

**TQC solved the task once across 90 deterministic eval episodes** (`success_rate = 0.011`). The solving episode was:

- **Model:** `TQC_seed0.zip`
- **Env reset seed:** `10010` (= `10_000 + seed_offset * 100 + ep_index` = `10000 + 0 + 10`)
- **Return:** `974.8`
- **Length:** `361` steps
- **Waypoints reached:** all 4

**PPO and SAC never solved a single episode.** PPO's "best" checkpoint exhibited classic reward hacking: 3603 steps of pure loitering with zero waypoints.

See **[DONE] `pyflyt_drl_fixedwing_3D_visual_results_phase_3.ipynb`** for the rendered 3D trajectory figures.
