# Architecture

<!--
OPTIONAL. Populate this directory for projects where architecture is a first-class concern
— multi-module systems, service boundaries, significant internal structure.
For simple projects, architecture notes in knowledge/architecture.md are sufficient.
-->

Structured architecture artifacts, distinct from `specs/domain-model.md` (which captures the domain) and `.claude/knowledge/architecture.md` (which captures non-obvious runtime facts).

## Suggested files

Add files here as the project's architecture grows. Common starting points:

| File | Purpose |
|------|---------|
| `module-map.md` | High-level modules / packages and their responsibilities |
| `dependency-graph.md` | Which modules depend on which; what is allowed to depend on what |
| `interfaces/` | Public contracts between modules (function signatures, message shapes, API schemas) |
| `data-models/` | Persistent data structures and their evolution |
| `events/` | Domain events, commands, and their consumers |
| `error-taxonomy.md` | Error types, their meanings, and how they propagate |
| `enforcement-map.md` | Which invariants are enforced where (code location → INV-NNN) |

## Relationship to other spec files

- `specs/domain-model.md` — WHAT exists in the domain (entities, VOs, events)
- `specs/architecture/` — HOW the implementation is structured (modules, boundaries, contracts)
- `.claude/knowledge/architecture.md` — WHY certain decisions were made and non-obvious runtime facts

The adversary role uses `interfaces/` and `dependency-graph.md` when scoping the "external assumptions" and "concurrency" attack vectors.
