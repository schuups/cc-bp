# Control Scenario: Attack (no blueprint)

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Create a plain `adr-001.txt` with an informal architecture description. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

```
Review this architecture decision for weaknesses, security issues, and things that could go wrong. Be adversarial.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/commands/attack.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1 (attacker role), 4–5 (attack-log format, HEAD hash), and 8 (no duplication check) will reliably **fail**. Items 2–3 (covering vectors, assigning severity) and 6–7 (CRITICAL prominence, specific surfaces) may partially pass — adversarial reviews are within Claude's default capability. The delta measures whether the blueprint adds systematic vector coverage and durable log output.
