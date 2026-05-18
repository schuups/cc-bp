# Control Scenario: Architecture

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/architecture.md`:

```
The design has been attack-reviewed and the findings addressed. Let's move into architecture — decide the component boundaries, technology choices, and interfaces for the notification subsystem.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/architecture.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1 (gate acknowledgement), 2 (reading project-map), 3 (ADR format), 9 (requirements cross-reference), and 10 (ADR acceptance criteria) will reliably **fail**. Items 4–7 (alternatives, interfaces, data flow, invariants) may partially pass. The delta measures the architecture phase scaffolding contribution.
