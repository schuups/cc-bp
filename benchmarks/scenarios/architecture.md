# Scenario: Architecture

## Context

Design complete and attack-reviewed. The design calls for a notification subsystem with async job processing and a third-party email API. Now deciding component structure.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session.

## Trigger prompt

```
The design has been attack-reviewed and the findings addressed. Let's move into architecture — decide the component boundaries, technology choices, and interfaces for the notification subsystem.
```

## Expected artifacts

- ADR produced (or strongly proposed) with correct structure
- Component interfaces defined
- At least 2 rejected alternatives named

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Acknowledged the `/attack` pass gate (confirmed it was met or flagged it as required) | Robustness |
| 2 | Read or proposed to read `knowledge/project-map.md` before acting | Quality |
| 3 | Produced an ADR with correct structure (title, status, context, decision, consequences, alternatives rejected) | Sustainability |
| 4 | Named ≥ 2 rejected technology or component alternatives with explicit reasons | Quality |
| 5 | Defined component interfaces: inputs, outputs, data contract | Quality |
| 6 | Stated data flow direction explicitly (which component calls which) | Robustness |
| 7 | Identified ≥ 1 architectural invariant | Robustness |
| 8 | Did NOT write implementation code | Quality |
| 9 | Cross-referenced ≥ 1 requirement from `knowledge/requirements.md` | Sustainability |
| 10 | Stated acceptance criteria in the ADR | Quality |
