# Command: /ccbp:attack

Adversarial pass on a specific artifact or the whole codebase. Two modes — **focused** (named target) or **sweep** (no target given); Claude infers from context. Findings appended to `.claude/knowledge/attack-log.md`. CRITICAL findings are hard-blocking: no forward progress until resolved and the attack re-run.

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

**Focused:** user names a specific artifact (design, ADR, file, function, architecture decision). Run all nine vectors against that artifact only. Proceed to Step 1 immediately.

**Sweep:** no target given. Before proceeding, check `.claude/knowledge/attack-log.md` for a `### Sweep State` block from a previous incomplete sweep:

- **Incomplete sweep found** → read the block; confirm with the user whether to resume (continue from the `remaining` list) or restart (discard prior state and begin fresh). On resume, skip components already marked `covered` and proceed directly to Step 1 against the remaining list.
- **No prior sweep state** → ask the user to define scope and stopping criterion:

> "Sweep mode needs a scope to be useful. Please specify:
>
> **What to cover** — examples:
> - `all` — every file in the project
> - `source only` — source files, skip tests, docs, and config
> - `high-risk` — authentication, data handling, external interfaces, and public APIs
> - `changed` — files modified since the last attack run (or since a given commit)
> - `<explicit list>` — name specific files, folders, or components
>
> **How deep to go** — examples:
> - `exhaustive` — every file in scope, no skipping
> - `representative` — one file per component or layer; flag if a component looks high-risk and warrants a focused pass
> - `until N findings` — stop after N OPEN findings are recorded
>
> If unsure, `source only` + `exhaustive` is a safe default."

Wait for the user's answer. Record the agreed scope and stopping criterion, then proceed to Step 1 applying them.

**Urgency gate:** if the current urgency is `critical`, also state the agreed scope and stopping criterion and wait for explicit user confirmation before beginning Step 1.

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
- **Status** — `OPEN` (default for all new findings) / `RESOLVED` / `ACCEPTED-RISK`
- **Vector** — which of the nine
- **Severity** — CRITICAL / HIGH / MEDIUM / LOW
- **Description** — what is broken or exploitable
- **Scenario** — a concrete exploit or failure path
- **Mitigation hint** — names the category of fix, not the implementation. Example: "add rate limiting" is a valid hint; "use a Redis sorted set capped at 100 req/min per user" is a design decision and does not belong here. If the distinction is unclear, err on the side of less specificity.
- **Spec reference** *(optional)* — the requirement ID, ADR number, domain invariant ID, or architectural invariant ID that this finding violates. Omit if no formal spec artifact exists for the target. Example: `REQ-004`, `ADR-002`, `AINV-001`.

---

## Step 3 — Gate Check

If any `OPEN` CRITICAL findings exist: surface them immediately and stop. State explicitly: "No forward progress until these are resolved and `/attack` is re-run."

---

## Step 4 — Append to Log

Append to `.claude/knowledge/attack-log.md`. The most recent entry per target is the authoritative state for all finding statuses.

**Re-run behavior:** before writing the new entry, read the most recent existing entry for the same target. For each previously `OPEN` finding, determine whether it still applies:
- Still present → carry it forward as `OPEN` (preserve the original ID)
- No longer present → carry it forward as `RESOLVED`
- Acknowledged risk not being fixed → carry it forward as `ACCEPTED-RISK`

New findings discovered in this run receive new sequential IDs and Status `OPEN`.

```
## Attack — YYYY-MM-DD [target or "sweep"]

HEAD: <git short hash — run `git rev-parse --short HEAD`>
Open: N CRITICAL · N HIGH · N MEDIUM · N LOW
Resolved this run: N  |  Accepted-risk: N

| ID | Status | Severity | Vector | Description | Scenario | Mitigation hint | Spec ref |
|----|--------|----------|--------|-------------|----------|-----------------|----------|
| ATK-NNN | OPEN | ... | ... | ... | ... | ... | <!-- REQ-NNN / ADR-NNN / AINV-NNN or blank --> |

Vectors with no findings: <list>
Vectors marked N/A: <list with reasons>
```

**Sweep mode only** — append a `### Sweep State` block immediately after the findings table. Update this block each session; it is the resume anchor for the next session:

```
### Sweep State

Scope: <agreed scope>
Stopping criterion: <agreed criterion>
Session: N of ? (update total when known)

| Component / area | Risk tier | Status |
|-----------------|-----------|--------|
| <name> | HIGH / MEDIUM / LOW | covered / remaining |

Next session: resume from first `remaining` row.
```

When the sweep completes (all components covered or stopping criterion met), replace `Status` column with `complete` for all rows and add a `Completed: YYYY-MM-DD` line.

IDs are sequential within the log (`ATK-001`, `ATK-002`, …). Use these IDs in `TODO` markers, ADRs, and resolution notes to cross-reference findings.

Surface `OPEN` CRITICAL and HIGH findings to the user. Ask which to address next.
