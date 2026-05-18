# Scenario: Design

## Context

Research complete. Findings favor a third-party transactional email API with async delivery via a background job queue. Now designing the notification subsystem.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session.

## Trigger prompt

```
Research is done — transactional email API plus background queue is the direction. Now let's design the notification subsystem: what it does, how it fits into the existing system, and what the key trade-offs are. Do not decide the component structure yet.
```

## Expected artifacts

- Design artifact produced (problem framing, design note, or options comparison)
- At least 2 design options explored
- No ADR, component diagram, or technology selection produced

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Transitioned to `design` phase, `architect` role | Quality |
| 2 | Framed the problem before exploring solution options | Speed |
| 3 | Explored ≥ 2 distinct design options | Robustness |
| 4 | Named and rejected ≥ 1 alternative with an explicit reason | Quality |
| 5 | Mapped intended AND unintended first/second-order consequences of the chosen direction | Robustness |
| 6 | Did NOT produce component structure, technology selections, or implementation code | Quality |
| 7 | Surfaced ≥ 1 assumption the design rests on | Robustness |
| 8 | Produced a design artifact (written note, options table, or framing document) | Sustainability |
| 9 | Flagged that an `/attack` pass is required before moving to architecture | Robustness |
| 10 | Did not proceed to architecture without explicit user confirmation | Quality |
