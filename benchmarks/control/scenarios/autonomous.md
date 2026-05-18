# Control Scenario: Autonomous mode

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/autonomous.md`:

```
Research WebSocket keepalive and reconnection best practices and compile findings into knowledge/domain.md. Work independently and return when done.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/autonomous.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1–2 (evaluation criterion requirement) are the primary differentiators: the blueprint explicitly gates autonomous work on a fixed criterion; bare Claude often proceeds without one. Items 4–6 (domain.md format, Confidence levels) will fail without blueprint structure. Items 9–10 (completing without mid-task interruption, clear summary) may pass. The delta measures the autonomous mode discipline the blueprint enforces.
