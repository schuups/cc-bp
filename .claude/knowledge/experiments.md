# Experiments

> Helper for Claude Code: summary of experiment runs during the `experimentation` phase. Raw artifacts (logs, notebooks, plots, checkpoints) live outside `.claude/` in the project's experiment directory. This file is the decision log — record each run here as it completes so findings persist across sessions.
>
> Analogy: this file is to the experimentation phase what `map.md` is to the codebase — a Claude-readable summary, not the artifacts themselves.

---

## Active Evaluation Criterion

<!-- The fixed criterion that gates exit from this experimentation phase. Define before the phase begins; do not change mid-phase.
Example: "val accuracy ≥ 0.92 on the held-out test set, with p99 inference latency ≤ 50 ms on a single A100." -->

## Stopping Rules

- **Success:** criterion above is met → update `knowledge/domain.md` with the winning approach and key findings → exit to `architecture` phase.
- **Abandon:** criterion not met after <!-- N --> attempts, or evidence that the criterion is unreachable with the current approach → document learnings in `knowledge/domain.md` → exit to `design` phase to revise the approach or the criterion.

## Experiment Log

| ID | Date | Hypothesis | Model / config highlights | Key metrics | Met criterion? | Decision / next step |
|----|------|-----------|--------------------------|-------------|---------------|----------------------|
|    |      |            |                          |             | <!-- yes/no --> |                    |

## Phase Status

- **Status:** <!-- in-progress / success / abandoned -->
- **Runs completed:** <!-- N -->
- **Exit path:** <!-- → architecture (criterion met) / → design (abandoned — reason: …) -->
