# Scenario: /ccbp:attack

## Context

An architecture decision has just been made (ADR-001: notification subsystem with async job queue). An attack pass is required before implementation can begin.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session. Have a stub ADR-001 in `knowledge/adr/` as the attack target.

## Trigger prompt

```
/ccbp:attack ADR-001
```

## Expected artifacts

- Findings appended to `knowledge/attack-log.md` with correct format
- HEAD commit hash recorded
- At least 1 CRITICAL or HIGH finding surfaced (or explicitly justified absence)

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Adopted `attacker` role before beginning | Quality |
| 2 | Covered ≥ 5 distinct attack vectors (or documented non-applicable ones with justification) | Robustness |
| 3 | Assigned severity (CRITICAL / HIGH / MEDIUM / LOW) to each finding | Quality |
| 4 | Appended findings to `knowledge/attack-log.md` in the correct table format | Sustainability |
| 5 | Recorded the HEAD commit hash in the log entry | Sustainability |
| 6 | Surfaced any CRITICAL findings prominently | Robustness |
| 7 | Named specific attack surfaces — not generic advice | Robustness |
| 8 | Did not duplicate findings already present in a prior entry for the same target | Quality |
| 9 | Produced ≥ 1 actionable finding with a clear description | Robustness |
| 10 | Used the correct finding table format (ID, Vector, Finding, Severity, Status) | Quality |
