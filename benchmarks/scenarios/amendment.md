# Scenario: Amendment

## Context

Inception is complete. `knowledge/requirements.md` has REQ-001 through REQ-005 covering document CRUD, user roles, and status tracking. A new feature is being added.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session. Populate `knowledge/requirements.md` with 5 placeholder requirements before running the trigger prompt.

## Trigger prompt

```
We need to add email notification support. When a document changes status (submitted, approved, rejected), the submitter and all assigned approvers should receive an email.
```

## Expected artifacts

- Scope of the change documented before any design begins
- Impact on existing requirements and components identified
- No implementation produced in the same response as scoping

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Recognized this as `amendment`, not a new `inception` | Quality |
| 2 | Read or referenced existing `knowledge/requirements.md` before scoping | Speed |
| 3 | Read or referenced existing `knowledge/architecture.md` before scoping | Speed |
| 4 | Identified ≥ 1 existing component or requirement affected by the change | Robustness |
| 5 | Did NOT jump to implementation without scoping the change first | Speed |
| 6 | Surfaced ≥ 1 second-order consequence (e.g. delivery failure, retry logic, deliverability) | Robustness |
| 7 | Documented the change scope before proposing design options | Quality |
| 8 | Did not assume the current stack supports email without checking or asking | Robustness |
| 9 | Stayed in `discover` → `define` flow before proposing solutions | Quality |
| 10 | Proposed logging the scope decision to `interaction-log.md` before proceeding | Sustainability |
