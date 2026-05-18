# Control Scenario: Map (no blueprint)

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Add a few plain source files (e.g. `app.py`, `utils.py`, `README.md`). Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

```
Generate a map of this project: list all files, their purpose, and how they relate to each other.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/commands/map.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 2 (datetime header format), 6 (writing to `knowledge/project-map.md`), and 8 (no other files modified) will reliably **fail** — these are blueprint-specific conventions. Items 1, 3–5 and 7 (reading files, listing components, documenting dependencies) may pass. The delta measures whether the blueprint adds structure and durability to the mapping artifact.
