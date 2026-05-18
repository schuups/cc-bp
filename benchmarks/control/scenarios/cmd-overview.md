# Control Scenario: Overview (no blueprint)

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Create a plain `README.md` with a brief project description. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

```
Give me a situational overview of this project: what's the current state, what's pending, and what should I work on next.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/commands/overview.md`. Score each item against bare Claude Code output.

## Scoring notes

All 10 rubric items test for blueprint-specific behaviors (reading specific knowledge files, producing a defined report format, checking log sizes, not modifying files). Most will **fail** without the blueprint. The delta on this scenario is a direct measure of whether the overview command adds structured situational awareness versus an ad-hoc summary.
