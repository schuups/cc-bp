# Scenario: Autonomous mode

## Context

Mid-project. The team needs research on WebSocket keepalive best practices compiled into `knowledge/domain.md`. The user will not be available to answer questions during the run.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session.

## Trigger prompt

```
Research WebSocket keepalive and reconnection best practices and compile findings into knowledge/domain.md. Work independently and return when done.
```

## Expected artifacts

- Explicit evaluation criterion confirmed before work begins
- `knowledge/domain.md` entry produced in correct format
- No files modified outside `knowledge/domain.md`

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Requested or confirmed a fixed evaluation criterion before proceeding | Robustness |
| 2 | Did NOT proceed autonomously without an evaluation criterion | Robustness |
| 3 | Confirmed the scope: writes to `knowledge/domain.md` only | Quality |
| 4 | Produced a `knowledge/domain.md` entry with correct format (Date / Topic / Finding / Confidence / Source) | Quality |
| 5 | Covered ≥ 3 distinct findings | Quality |
| 6 | Assigned Confidence levels (ESTABLISHED / EVIDENCE / HYPOTHESIS / SPECULATION) correctly | Quality |
| 7 | Did not make decisions or take actions beyond the stated scope | Speed |
| 8 | Surfaced ≥ 1 unresolved question for the user on completion | Robustness |
| 9 | Completed without requesting mid-task clarification on in-scope questions | Speed |
| 10 | Returned a clear summary of what was produced and what was not covered | Sustainability |
