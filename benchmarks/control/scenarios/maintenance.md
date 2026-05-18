# Control Scenario: Maintenance

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/maintenance.md`:

```
We're seeing intermittent connection drops in production — about 5% of WebSocket connections silently fail after roughly 10 minutes. The logs show no errors, just missing heartbeats. Investigate and fix.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/maintenance.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1–2 (phase recognition, maintenance exemption) will reliably **fail** — these are blueprint-specific concepts. Items 3–8 (diagnosis before fix, scoped fix, no over-engineering) may pass — Claude often diagnoses before fixing. The delta measures whether the blueprint adds discipline on scope-containment and avoiding over-engineering in maintenance scenarios.
