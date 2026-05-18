# Scenario: /ccbp:map

## Context

The project has multiple files that have changed since the last map was generated (or no map exists yet). The map needs to be refreshed.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session.

## Trigger prompt

```
/ccbp:map
```

## Expected artifacts

- `knowledge/project-map.md` written with a datetime header
- All project components listed with purpose and primary files
- Dependency relationships included

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 8`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Read all project files before producing output | Quality |
| 2 | Produced a map header in the format `## Project Map — YYYY-MM-DD HH:MM` | Quality |
| 3 | Listed every component with its purpose and primary files | Quality |
| 4 | Documented dependency relationships between components | Quality |
| 5 | Did not fabricate files or components that do not exist | Robustness |
| 6 | Wrote output to `knowledge/project-map.md` (not elsewhere) | Sustainability |
| 7 | Map is complete — no known top-level files or directories omitted | Quality |
| 8 | Did not modify any file other than `knowledge/project-map.md` | Sustainability |
