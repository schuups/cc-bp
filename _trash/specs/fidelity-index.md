# Fidelity Index

Tracks how well the implementation covers the domain model, invariants, and ADR decisions. Updated by Sweep mode. Used by AdvSweep to prioritize adversary attention.

---

## Coverage Ratings

| Rating | Meaning |
|--------|---------|
| `THOROUGH` | Strong coverage; implementation matches spec; edge cases handled |
| `MODERATE` | Reasonable coverage; some gaps; no critical divergences |
| `LOW` | Minimal coverage; significant gaps; improvement needed |
| `NONE` | Not yet audited, or no test/spec coverage |

## Status Flags

| Flag | Meaning |
|------|---------|
| — | No divergence detected |
| `[!] DIVERGENT` | Implementation diverges from spec (even if tests pass) — see findings.md |

---

## Domain Model Coverage

<!--
Populate from specs/domain-model.md.
One row per entity, value object, aggregate, and domain event.
-->

| # | Concept | Type | Coverage | Status | Last audited | Findings ref |
|---|---------|------|----------|--------|-------------|-------------|
| 1 | *(populate from domain-model.md)* | Entity / VO / Aggregate / Event | `NONE` | — | — | — |

---

## Invariant Coverage

<!--
Populate from specs/invariants.md.
One row per INV-NNN entry.
-->

| Invariant | Title | Coverage | Status | Last audited | Findings ref |
|-----------|-------|----------|--------|-------------|-------------|
| INV-001 | *(populate from invariants.md)* | `NONE` | — | — | — |

---

## ADR Implementation Coverage

<!--
Populate from specs/adr/.
One row per Accepted ADR.
-->

| ADR | Title | Coverage | Status | Last audited | Findings ref |
|-----|-------|----------|--------|-------------|-------------|
| 0001 | *(populate from adr/)* | `NONE` | — | — | — |

---

## Sweep History

| Sweep date | Items reviewed | THOROUGH | MODERATE | LOW | NONE | DIVERGENT | Open escalations |
|-----------|--------------|---------|---------|-----|------|-----------|-----------------|
| *(none yet)* | | | | | | | |
