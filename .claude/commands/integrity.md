# Command: /integrity

Active structural audit. Finds what is broken — broken links, stale documentation, unregistered work items, spec inconsistencies. Run on demand. Not a dashboard.

Append the full report to `.claude/knowledge/health/integrity-log.md`. Escalate any HIGH or CRITICAL findings to `.claude/knowledge/health/escalations.md`. This command finds; it does not fix.

---

## Preconditions

Check these before running any category.

**Hard stops** — if the condition is met, state the reason and suggested next step; do not proceed.

| Condition | Reason | Suggested next step |
|-----------|--------|---------------------|
| `.claude/commands/onboard.md` exists | Knowledge files not yet populated; most checks are meaningless | Run `/onboard` first |
| `requirements.md` contains no populated rows | Cat 1 fidelity trace and Cat 4 fidelity checks are meaningless without requirements | Complete `/onboard` or populate `requirements.md` manually |

**Degraded run** — if the condition is met, skip the affected categories and note what was skipped in the report.

| Condition | Skip | Suggested next step |
|-----------|------|---------------------|
| `fidelity-index.md` has no coverage entries (Sweep never run) | Cat 1 fidelity trace check; Cat 4 fidelity checks; Cat 5 NONE/DIVERGENT counts | Run Sweep after this command to enable full checks |
| `fidelity-index.md` `Last sweep` date is more than 30 days ago | Same as above — results may not reflect current code | Run Sweep to refresh before relying on Cat 4 and Cat 5 results |

---

## Before Starting

Read `.claude/knowledge/health/integrity-log.md` and `.claude/knowledge/health/escalations.md` to avoid duplicating already-open items.

---

## Category 1 — Reference Integrity

| Check | Severity | How |
|-------|----------|-----|
| Markdown `[text](path)` links in `documentation/`, `.claude/knowledge/`, root `*.md` | HIGH | Grep for `\[.*\]\(.*\.md\)`; verify each path exists |
| `.claude/knowledge/health/fidelity-index.md` rows trace to a requirement ID in `requirements.md` | MEDIUM | Cross-reference row descriptions against requirement IDs |
| Resolved entries in `adversary-findings.md` reference a real commit hash, ADR, or follow-up item | LOW | Read resolved entries; check references |

---

## Category 2 — Work Item Completeness

| Check | Severity | How |
|-------|----------|-----|
| `TODO` / `FIXME` / `HACK` in source files without an open entry in `adversary-findings.md` | MEDIUM | Grep source files for these markers; cross-reference |
| Skipped or `xfail` tests without an open entry in `adversary-findings.md` | MEDIUM | Grep for `@pytest.mark.skip`, `test.skip`, `xit(`, `t.Skip`, `pending(` |
| Debug instrumentation left in source (`console.log`, `print(`, `debugger`, `binding.pry`, `fmt.Println`) | HIGH | Grep source files |
| Stub implementations (`raise NotImplementedError`, `panic("not implemented")`) without findings entry | MEDIUM | Grep source files; cross-reference |

---

## Category 3 — Documentation Freshness

| Check | Severity | How |
|-------|----------|-----|
| Each directory README describes actual current directory contents | MEDIUM | Read each README; compare to `ls` |
| Interfaces documented in `documentation/` still exist in source | MEDIUM | Read documentation files; grep source for referenced symbols |
| Invariants in `.claude/knowledge/context/invariants.md` have identifiable enforcement points in source | HIGH | For each invariant, grep source for enforcement |
| `.claude/knowledge/context/adr/INDEX.md` is complete — no ADR file missing a row, no row missing a file | MEDIUM | Cross-check `ls adr/` against index rows; skip `0001-template.md` |

---

## Category 4 — Spec Consistency

| Check | Severity | How |
|-------|----------|-----|
| No ADRs with status `Proposed` older than 14 days | LOW | Read each ADR; check date and status |
| ADR supersession chains valid — `Superseded by NNNN` target exists and has status `Accepted` | MEDIUM | Read ADRs with Superseded-by; verify the target ADR |
| Fidelity-index row count is consistent with the number of `implemented` requirements in `requirements.md` | MEDIUM | Count rows (THOROUGH + MODERATE + LOW + NONE); compare to implemented count |
| No requirement marked `implemented` in `requirements.md` has NONE fidelity coverage | MEDIUM | Cross-reference both files |

---

## Category 5 — Open Item Triage

Count and surface — informational only, not blocking:

- `fidelity-index.md`: count NONE-rated and DIVERGENT entries
- `escalations.md`: count open entries; flag any open longer than 30 days
- `adversary-findings.md`: count open HIGH and CRITICAL findings
- `audit-log.md`: flag any FAIL sign-off that has no subsequent PASS for the same scope

---

## Report Format

Append to `.claude/knowledge/health/integrity-log.md`:

```
## Integrity Audit — YYYY-MM-DD

Overall: CLEAN | ISSUES FOUND
Findings: N CRITICAL · N HIGH · N MEDIUM · N LOW

Cat 1 References:  <findings, or "clean">
Cat 2 Work items:  <findings, or "clean">
Cat 3 Docs:        <findings, or "clean">
Cat 4 Specs:       <findings, or "clean">
Cat 5 Triage:      N NONE · N DIVERGENT · N escalations (oldest N days) · N open HIGH/CRITICAL · N FAIL without PASS
```

After appending: surface all CRITICAL and HIGH findings to the user and ask which open items to address next.
