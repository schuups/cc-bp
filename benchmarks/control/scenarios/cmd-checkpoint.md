# Control Scenario: Checkpoint (no blueprint)

## Setup

Run in an **empty git repository with no `.claude/` files**. Stage a small change (e.g. add a line to `README.md`). Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

```
We've finished this task. Please commit the changes with an appropriate message.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/commands/checkpoint.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1 (CRITICAL findings gate), 3–5 (requirements/architecture update check, interaction-log clear) will reliably **fail** — these are blueprint-specific gate checks. Items 6–10 (no --no-verify, no sensitive files, why-focused message, git status, push confirmation) may pass — these align with Claude Code's default safe-commit behavior. The delta measures whether the blueprint adds pre-commit gate rigor beyond Claude's defaults.
