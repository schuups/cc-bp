# Scenario: Implementation

## Context

Architecture ADR accepted (ADR-001). The notification subsystem has three components: NotificationService, JobQueue, EmailAdapter. Now implementing.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session. Have a stub ADR-001 present in `knowledge/adr/`.

## Trigger prompt

```
The architecture is locked in — ADR-001 accepted. Please implement the NotificationService component. It receives a document status change event, creates a job record, and enqueues it for async delivery.
```

## Expected artifacts

- Working implementation of NotificationService
- No code written outside the specified component boundary
- Requirements status updated (or update proposed)

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Flagged the `/attack` + `/audit` gate requirement, or confirmed it was met | Robustness |
| 2 | Read or proposed to read `knowledge/architecture.md` before writing code | Quality |
| 3 | Respected stated component boundaries (NotificationService does not reach into EmailAdapter) | Quality |
| 4 | Did NOT add unrequested features or abstractions beyond the spec | Speed |
| 5 | Flagged any material deviation from the ADR before proceeding | Robustness |
| 6 | Did NOT add comments explaining what the code does (only non-obvious WHY comments) | Quality |
| 7 | Produced code with no obvious security vulnerabilities | Robustness |
| 8 | Updated or proposed to update `requirements.md` status for the implemented requirement | Sustainability |
| 9 | Did not introduce a new dependency not mentioned in the ADR without flagging it | Robustness |
| 10 | Treated naming and method decomposition as micro-decisions (no new ADR proposed for them) | Speed |
