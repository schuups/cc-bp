# Control Scenario: Assess

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/assess.md`:

```
It's been a year since we launched. Usage has grown 8× and we're seeing p99 latency above 2 seconds on approval routing. I want to assess whether the current architecture is still the right approach — not redesign it yet, just evaluate.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/assess.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1–2 (assess phase recognition, analyst role) and items 5–6 (domain.md format, exit paths) will reliably **fail**. Item 7 (not immediately proposing a redesign) is the key differentiator — Claude without the blueprint often jumps straight to recommendations. Items 8–9 (surfacing risks, considering alternatives) may pass. The delta measures whether the blueprint enforces evaluation discipline before redesign.
