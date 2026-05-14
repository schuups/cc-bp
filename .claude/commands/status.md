Run ASSESS mode following `.claude/protocols/assess.md`.

Read the project state from:
- `specs/fidelity-index.md` — coverage distribution, DIVERGENT entries, last sweep date
- `specs/findings.md` — open CRITICAL/HIGH/MEDIUM findings
- `specs/escalations.md` — open escalations and their age
- `specs/deferred.md` — deferred work items and any whose activate-when condition is now met
- `specs/adr/` — any ADRs with status `Proposed`

Also evaluate re-sweep conditions (git log commit count, last sweep age, invariants changes, DIVERGENT count, LOW count) and include `Sweep recommended: YES/NO` in the report.

Produce the status report in the format defined in the protocol. Do not make any changes to any file.

This command runs automatically at the start of every new session before any other action.
