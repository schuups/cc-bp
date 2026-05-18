# Control Scenario: Inception

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/inception.md`:

```
I want to build a document approval workflow system. Documents get submitted, routed to one or more approvers, and either approved or rejected with comments. Approvals may be sequential or parallel. Let's get started.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/inception.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1–2 (overview invocation, state transition announcement) will reliably **fail** — these behaviors are blueprint-specific. Items 3–7 (discovery questions, ambiguity surfacing) may pass if Claude's default behavior is thorough. Items 8–10 (structured artifacts, requirements.md) will fail without the blueprint prescribing these formats.

The delta between blueprint score and control score on this scenario is the measurable contribution of the inception phase scaffolding.
