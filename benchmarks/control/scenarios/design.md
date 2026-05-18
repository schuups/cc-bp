# Control Scenario: Design

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/design.md`:

```
Research is done — transactional email API plus background queue is the direction. Now let's design the notification subsystem: what it does, how it fits into the existing system, and what the key trade-offs are. Do not decide the component structure yet.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/design.md`. Score each item against bare Claude Code output.

## Scoring notes

Item 1 (phase/role transition), item 9 (flagging `/attack` gate), and item 10 (halting for confirmation) will reliably **fail**. Items 3–5 (exploring options, naming alternatives, mapping consequences) may pass depending on Claude's default thoroughness. The delta measures the design phase scaffolding contribution.
