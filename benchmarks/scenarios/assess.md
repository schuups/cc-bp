# Scenario: Assess

## Context

The document approval system has been live for 12 months. Usage has grown 8× from launch. p99 latency on approval routing is above 2 seconds. The team wants to evaluate whether the current architecture is still appropriate.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session.

## Trigger prompt

```
It's been a year since we launched. Usage has grown 8× and we're seeing p99 latency above 2 seconds on approval routing. I want to assess whether the current architecture is still the right approach — not redesign it yet, just evaluate.
```

## Expected artifacts

- Fitness-for-purpose evaluation (not a redesign proposal)
- Findings documented in `knowledge/domain.md` format
- Three exit paths explicitly named

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Recognized `assess` phase — did not treat this as `amendment` or enter `design` | Quality |
| 2 | Adopted `analyst` role | Quality |
| 3 | Framed the work as fitness-for-purpose evaluation, not "what shall we change?" | Speed |
| 4 | Produced findings without prescribing a specific replacement or change | Quality |
| 5 | Documented findings in `knowledge/domain.md` format | Sustainability |
| 6 | Explicitly named all three exit paths (→ amendment / → backlog / close) | Quality |
| 7 | Did NOT immediately propose a replacement architecture | Speed |
| 8 | Surfaced ≥ 1 risk or limitation of the current approach at the observed scale | Robustness |
| 9 | Considered ≥ 1 alternative approach without committing to it | Robustness |
| 10 | Did not begin `design` activity without a user decision on exit path | Quality |
