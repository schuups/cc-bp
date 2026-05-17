# Command: /ccbp:attack

Adversarial pass on a specific artifact or the whole codebase. Two modes — **focused** (named target) or **sweep** (no target given); Claude infers from context. Findings appended to `.claude/logs/attack.md`. CRITICAL findings are hard-blocking: no forward progress until resolved and the attack re-run.

This command is intentionally thorough and will take time. Do not skip vectors, summarise findings, or soft-pedal what is found. Slowness is expected and correct.

---

## Principles

- **Truth over comfort** — report what is true, including when inconvenient; never soften findings.
- **Work over performance** — a good outcome, not a completed checklist; don't perform rigor, be rigorous.
- **Proportional skepticism** — scrutiny proportional to stakes; calibrate, don't apply uniform intensity.
- **Bring things to light** — surface problems immediately and plainly; don't defer, don't quietly fix.
- **The artifact, not the person** — attack the work, never the effort behind it.
- **Consequences before commitment** — map intended and unintended effects before every key decision.
- **Honest limits** — "I don't know" and "I need X first" are complete, valid answers.
- **Non-linear by default** — the first framing is rarely right; challenge initial assumptions and loop back when findings demand it.
- **Discipline as respect** — the gate exists because it produces better outcomes; don't shortcut it.

---

## Step 0 — Identify Target and Mode

**Focused:** user names a specific artifact (design, ADR, file, function, architecture decision). Run all nine vectors against that artifact only.

**Sweep:** no target given. Run all nine vectors across the codebase systematically.

---

## Step 1 — Run the Nine Vectors

Evaluate every vector. Mark `N/A — <reason>` only when a vector genuinely does not apply. Never suppress a finding because it is inconvenient.

| Vector | What to look for |
|--------|-----------------|
| **Correctness** | Wrong outputs, edge cases, off-by-one, silent failures, wrong assumptions |
| **Security** | Auth bypass, injection, secrets exposure, privilege escalation, insecure defaults |
| **Scalability** | Bottlenecks, unbounded growth, thundering herd, resource exhaustion |
| **Reliability** | Single points of failure, cascade failures, missing error handling, split-brain |
| **Maintainability** | Hidden coupling, untestable code, implicit contracts, tribal knowledge |
| **Operability** | Unobservable failures, missing alerts, hard rollback, undocumented failure modes |
| **Data Integrity** | Corruption paths, silent data loss, inconsistency, lineage breaks |
| **Performance** | Latency, throughput, memory, I/O bottlenecks, algorithmic complexity |
| **Compliance** | Regulatory, licensing, PII handling, audit trail, data retention |

For each active declination (`:ml-engineer`, `:devops-engineer`, etc.), also apply the domain-specific sub-vectors defined in `roles/attacker.md`.

---

## Step 2 — Classify Each Finding

Every finding must state:
- **Vector** — which of the nine
- **Severity** — CRITICAL / HIGH / MEDIUM / LOW
- **Description** — what is broken or exploitable
- **Scenario** — a concrete exploit or failure path
- **Mitigation hint** — direction only; not a design decision

---

## Step 3 — Gate Check

If any CRITICAL findings exist: surface them immediately and stop. State explicitly: "No forward progress until these are resolved and `/attack` is re-run."

---

## Step 4 — Append to Log

Append to `.claude/logs/attack.md`. Each entry supersedes all previous entries for the same target — the most recent entry per target is the authoritative open state.

```
## Attack — YYYY-MM-DD [target or "sweep"]

Findings: N CRITICAL · N HIGH · N MEDIUM · N LOW

| ID | Severity | Vector | Description | Scenario | Mitigation hint |
|----|----------|--------|-------------|----------|-----------------|
| ATK-NNN | ... | ... | ... | ... | ... |

Vectors with no findings: <list>
Vectors marked N/A: <list with reasons>
```

IDs are sequential within the log (`ATK-001`, `ATK-002`, …). Use these IDs in `TODO` markers, ADRs, and resolution notes to cross-reference findings.

Surface CRITICAL and HIGH findings to the user. Ask which to address next.
