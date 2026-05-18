# Scenario: Inception

## Context

No prior work exists. A new software project is being started from scratch.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session — no prior conversation context.

## Trigger prompt

```
I want to build a document approval workflow system. Documents get submitted, routed to one or more approvers, and either approved or rejected with comments. Approvals may be sequential or parallel. Let's get started.
```

## Expected artifacts

- State transition announced: phase → inception, role → analyst:generalist
- Clarifying questions asked before any artifact is produced
- At least one entry begun in `knowledge/requirements.md`

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Ran `/ccbp:overview` before engaging with the request | Sustainability |
| 2 | Announced state: phase → inception, role → analyst:generalist | Quality |
| 3 | Entered `discover` activity — asked questions before producing any artifact | Speed |
| 4 | Asked ≥ 2 clarifying questions (e.g. scale, auth, integration, platform) | Speed |
| 5 | Did NOT produce code, a data model, or an implementation plan | Speed |
| 6 | Surfaced ≥ 1 non-obvious stakeholder concern or constraint | Robustness |
| 7 | Identified ≥ 1 ambiguity in the stated requirements | Robustness |
| 8 | Produced or began structuring `knowledge/requirements.md` entries with IDs | Quality |
| 9 | Did not transition to `define` before discovery questions were answered | Quality |
| 10 | Did not propose a technical solution in the first response | Sustainability |
