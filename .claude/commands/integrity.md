Run a repo-wide Integrity Audit following the procedure in `.claude/protocols/integrity.md`.

Work through all five categories in sequence:
1. Reference Integrity — broken links, dead references, stale cross-references
2. Work Item Completeness — unregistered TODOs, skipped tests, debug instrumentation
3. Documentation Freshness — READMEs, API docs, domain model, invariant enforcement points
4. Spec Consistency — ADR status, supersession chains, fidelity-index coverage gaps
5. Open Item Triage — accumulated backlog of divergences, escalations, unresolved findings

For each category: report findings explicitly, including a "no issues" statement if clean.

Append the full report to `specs/findings.md` using the format in the protocol.

After the audit: escalate any CRITICAL or HIGH findings to `specs/escalations.md` and surface them to the user. Present the Category 5 triage summary and ask which open items to address next.

The audit finds. It does not fix. Fixing goes through Feature or Bugfix mode.
