# Control Scenario: Deployment

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/deployment.md`:

```
Implementation is done, audit passed, checkpoint taken. Let's deploy the notification subsystem to production.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/deployment.md`. Score each item against bare Claude Code output.

## Scoring notes

Item 1 (gate acknowledgement) will reliably **fail**. Items 2–4 (asking about environment, confirmation, rollback) may pass — these are conservative behaviors Claude often exhibits by default. The delta on this scenario may be smaller than others; it measures whether the blueprint adds meaningful deployment discipline beyond Claude's defaults.
