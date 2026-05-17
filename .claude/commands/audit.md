# Command: /ccbp:audit

Verify integrity: references valid, documentation consistent, implementation aligned with decisions and architecture. Also invocable inline ("audit this"). Findings appended to `.claude/logs/audit.md`; CRITICAL and HIGH findings surfaced to the user.

This command is intentionally thorough and will take time. Do not skip checks, accept superficial passes, or mark items clean without verifying. Slowness is expected and correct.

---

## Principles

- **Truth over comfort** — report what is true, including when inconvenient; never soften findings.
- **Work over performance** — a good outcome, not a completed checklist; don't perform rigor, be rigorous.
- **Proportional skepticism** — scrutiny proportional to stakes; calibrate, don't apply uniform intensity.
- **Bring things to light** — surface problems immediately and plainly; don't defer, don't quietly fix.
- **Consequences before commitment** — map intended and unintended effects before every key decision.
- **Honest limits** — "I don't know" and "I need X first" are complete, valid answers.
- **Non-linear by default** — the first framing is rarely right; challenge initial assumptions and loop back when findings demand it.
- **Discipline as respect** — the gate exists because it produces better outcomes; don't shortcut it.

---

## Preconditions

**Hard stops:**

| Condition | Reason | Next step |
|-----------|--------|-----------|
| `onboard.md` still exists in `.claude/commands/` | Knowledge files not yet populated; checks are meaningless | Run `/onboard` first |
| `requirements.md` has no row with non-empty requirement text (column 2 is blank on every row) | A template-only file makes reference checks meaningless | Complete `/onboard` or populate manually |

**Map staleness check:** Read `.claude/logs/map.md`. Extract the datetime from `## Project Map — YYYY-MM-DD HH:MM`; compare against `git log -1 --format=%ci`. The map is stale if its datetime is earlier than the last commit timestamp. If absent or older than the last commit: warn "Map may be stale — category checks may miss recently added or removed components. Run `/ccbp:map` to refresh." Continue unless the user opts to refresh first.

**Urgency gate:** if the current urgency is `critical`, state the scope of the audit (full or named scope) and wait for explicit user confirmation before beginning the category checks.

Read `.claude/logs/audit.md` before starting to avoid duplicating already-open items.

---

## Category 1 — Reference Integrity

| Check | Severity | How |
|-------|----------|-----|
| Markdown links in `.claude/knowledge/`, `documentation/`, root `*.md` | HIGH | Grep for `\[.*\]\(.*\.md\)`; verify each path exists |
| ADR index complete — no ADR file missing a row, no row missing a file | MEDIUM | Cross-check `ls .claude/knowledge/adr/` against index rows |
| ADR supersession chains valid — `Superseded by NNNN` target exists and has status `Accepted` | MEDIUM | Read ADRs with Superseded-by; verify the target |
| Each accepted ADR has at least one grep-verifiable acceptance criterion (names a symbol, function, test, or config key) | MEDIUM | Read each ADR's Acceptance Criteria section; flag any where every criterion is purely narrative |

---

## Category 2 — Work Item Completeness

| Check | Severity | How |
|-------|----------|-----|
| `TODO` / `FIXME` / `HACK` in source without an open entry in `.claude/logs/attack.md` | MEDIUM | Grep source files; cross-reference |
| Skipped or `xfail` tests without an open entry in `.claude/logs/attack.md` | MEDIUM | Grep for `@pytest.mark.skip`, `test.skip`, `xit(`, `t.Skip`, `pending(` |
| Debug instrumentation left in source (`console.log`, `print(`, `debugger`, `binding.pry`, `fmt.Println`) | HIGH | Grep source files |
| Stub implementations (`raise NotImplementedError`, `panic("not implemented")`) without findings entry | MEDIUM | Grep source files; cross-reference |
| Real PII in test fixtures, seed data, or documentation examples (real names, emails, phone numbers, addresses) | CRITICAL | Grep test and seed files for patterns resembling real personal data; use synthetic data only (`user@example.com`, `+1-555-0100`) |

---

## Category 3 — Documentation Freshness

| Check | Severity | How |
|-------|----------|-----|
| `architecture.md` reflects current component structure | HIGH | Compare against source; flag discrepancies |
| Interfaces documented in `documentation/` still exist in source | MEDIUM | Read docs; grep source for referenced symbols |
| Each directory README describes actual current contents | MEDIUM | Read each README; compare to `ls` |
| `requirements.md` statuses are current — no `implemented` requirement that is clearly not implemented | HIGH | Cross-check against source |

---

## Category 4 — Spec Consistency

| Check | Severity | How |
|-------|----------|-----|
| No ADRs with status `Proposed` older than 14 days | LOW | Read each ADR; check date and status |
| `requirements.md` has no ambiguous or contradictory entries | MEDIUM | Read requirements; flag unclear scope or conflicting items |
| `backlog.md` entries are still relevant — none silently implemented or superseded | LOW | Read backlog; cross-check against current state |

---

## Category 5 — Open Item Triage

Informational only — not blocking, but must appear in the report.

- `.claude/logs/attack.md`: for each distinct target, read the most recent `## Attack —` entry; count its findings by severity (CRITICAL / HIGH / MEDIUM); flag any entry dated more than 30 days ago
- `.claude/logs/audit.md`: flag any FAIL sign-off that has no subsequent PASS for the same scope
- `.claude/knowledge/backlog.md`: count entries; flag any whose deferral condition is now met
- `.claude/knowledge/adr/`: count ADRs with status `Proposed`

---

## Report Format

Append to `.claude/logs/audit.md`:

```
## Audit — YYYY-MM-DD [scope or "full"]

Overall: PASS | FAIL
HEAD: <git short hash — run `git rev-parse --short HEAD`>
Diff: <run `git diff HEAD | shasum -a 256 | cut -c1-8`; write "clean" if output is empty>
Findings: N CRITICAL · N HIGH · N MEDIUM · N LOW

Cat 1 References:  <findings, or "clean">
Cat 2 Work items:  <findings, or "clean">
Cat 3 Docs:        <findings, or "clean">
Cat 4 Specs:       <findings, or "clean">
Cat 5 Triage:      N open findings · N unresolved FAIL sign-offs · N backlog activate-now · N proposed ADRs
```

Surface all CRITICAL and HIGH findings to the user. Ask which to address next.
