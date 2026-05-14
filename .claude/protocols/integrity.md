# Protocol: Integrity Audit

Repo-wide health check. Run proactively — not just after changes. Distinct from Sweep (fidelity against specs) and the pre-commit gate (per-diff checks).

Invoke with `/integrity`.

---

## Before Starting

Read `specs/fidelity-index.md`, `specs/escalations.md`, and `specs/findings.md` to avoid duplicating already-open items.

---

## Category 1: Reference Integrity

| Check | Severity |
|-------|----------|
| `specs/adr/README.md` index — every listed filename exists on disk | HIGH |
| Markdown `[text](path)` links in `docs/`, `specs/`, root `*.md` | HIGH |
| `@see` / `see also:` doc-comment cross-references in source | MEDIUM |
| `specs/fidelity-index.md` rows trace to domain-model or invariants entries | MEDIUM |
| `specs/findings.md` resolutions reference a real commit, ADR, or follow-up | LOW |

## Category 2: Work Item Completeness

| Check | Severity |
|-------|----------|
| `TODO` / `FIXME` / `HACK` without a `specs/findings.md` entry | MEDIUM |
| Skipped / `xfail` / `pending` tests without a `specs/findings.md` entry | MEDIUM |
| Debug instrumentation (`console.log`, `print(`, `debugger`, `binding.pry`, `fmt.Println`) | HIGH |
| Stub implementations (`raise NotImplementedError`, `panic("not implemented")`) without findings entry | MEDIUM |

## Category 3: Documentation Freshness

| Check | Severity |
|-------|----------|
| Each directory README describes actual current contents | MEDIUM |
| `docs/api/` documented interfaces still exist in code | MEDIUM |
| `specs/domain-model.md` named entities locatable in codebase | MEDIUM |
| `specs/invariants.md` enforcement points exist in code | HIGH |
| Root `CLAUDE.md` quick-reference paths all exist on disk | LOW |
| `specs/adr/README.md` index complete — no file missing a row, no row missing a file | MEDIUM |

## Category 4: Spec Consistency

| Check | Severity |
|-------|----------|
| No `Proposed` ADRs older than 14 days | LOW |
| ADR supersession chains valid — `Superseded by NNNN` target exists and is `Accepted` | MEDIUM |
| `specs/fidelity-index.md` row count consistent with domain-model + invariants + accepted ADRs | MEDIUM |
| No `[RETIRED]` invariants still appearing as open rows in fidelity-index | LOW |

## Category 5: Open Item Triage

Count and surface — no blocking findings, just a summary:
- `specs/fidelity-index.md`: NONE-rated and DIVERGENT counts
- `specs/escalations.md`: open escalations; flag any older than 30 days
- `specs/findings.md`: open HIGH/CRITICAL count

---

## Report

Append to `specs/findings.md`:

```
## Integrity Audit — YYYY-MM-DD

Overall: CLEAN | ISSUES FOUND
Findings: N CRITICAL · N HIGH · N MEDIUM · N LOW

Cat 1 References:  <findings or "clean">
Cat 2 Work items:  <findings or "clean">
Cat 3 Docs:        <findings or "clean">
Cat 4 Specs:       <findings or "clean">
Cat 5 Triage:      N NONE · N DIVERGENT · N escalations (oldest N days) · N open HIGH/CRITICAL
```

CRITICAL and HIGH findings: add to `specs/escalations.md` and surface to the user.

**The integrity audit finds. It does not fix.**
