# Control Scenario: Implementation

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Create a plain text file `adr-001.txt` with an informal description of the NotificationService architecture. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/implementation.md`:

```
The architecture is locked in — ADR-001 accepted. Please implement the NotificationService component. It receives a document status change event, creates a job record, and enqueues it for async delivery.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/implementation.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1 (gate check), 8 (requirements status update), and 9–10 (dependency flagging, micro-decision treatment) will reliably **fail**. Items 3–7 (component boundaries, scope, security) may partially pass. The delta measures the implementation phase scaffolding contribution.
