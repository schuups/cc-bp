# Ubiquitous Language

Terms that have precise meanings in this domain. If a term appears in code, specs, conversations, or documentation, it means exactly what is defined here — nothing else. Ambiguous use of these terms is a defect.

---

## How to Use This File

- **Adding a term:** append a row to the relevant table. Consistency with `specs/domain-model.md` entity names is required.
- **Renaming a term:** update the entry AND update every use of the old name in code, specs, and documentation. A rename is a refactor — run the pre-commit reference check.
- **Resolving ambiguity:** if two people use a term differently, that is a gap — add a clarifying entry here and surface it as a finding.

---

## Project Domain Terms

| Term | Precise meaning | Must not be confused with | Source |
|------|----------------|--------------------------|--------|
| *(replace with first real project term)* | | | domain-model.md |

---

## Blueprint Acronyms and Methodology Terms

Terms specific to this blueprint's methodology. These apply to every project that uses it.

### Acronyms

| Acronym | Expands to | Meaning in this blueprint |
|---------|-----------|--------------------------|
| ADR | Architecture Decision Record | A file in `specs/adr/` recording a design decision, its options, rationale, and consequences. Immutable once Accepted. |
| ADV | Adversary | The role that attacks artifacts to find gaps, edge cases, and failure modes. |
| AdvSweep | Adversary Sweep | The adversary-led mode for systematically sweeping the codebase for attack surfaces and failure modes. Distinct from Sweep (fidelity audit). |
| ASM | Assumption (identifier prefix) | Used as ASM-NNN in `specs/assumptions.md`. |
| BDD | Behavior-Driven Development | Test specifications written as human-readable scenarios (Given/When/Then). |
| CI | Continuous Integration | Automated build and test pipeline triggered on commits or pull requests. |
| DD | Double Diamond | The four-phase design process model: Discover → Define → Develop → Deliver. |
| ESC | Escalation (identifier prefix) | Used as ESC-NNN in `specs/escalations.md`. |
| FM | Failure Mode (identifier prefix) | Used as FM-NNN in `specs/failure-modes.md`. |
| INV | Invariant (identifier prefix) | Used as INV-NNN in `specs/invariants.md`. |
| PII | Personally Identifiable Information | Protected personal data subject to privacy regulations. |
| PR | Pull Request | A code review and merge request in a git hosting platform. |
| VO | Value Object | A domain object with no identity; equality is determined entirely by its field values. |

### Fidelity Coverage Ratings

Used in `specs/fidelity-index.md` to rate how well each spec entry is covered by the implementation and tests.

| Rating | Meaning |
|--------|---------|
| `THOROUGH` | Strong coverage; implementation matches spec; edge cases handled |
| `MODERATE` | Reasonable coverage; some gaps; no critical divergences |
| `LOW` | Minimal coverage; significant gaps; improvement needed |
| `NONE` | Not yet audited, or no test or spec coverage |
| `[!] DIVERGENT` | Implementation diverges from spec — independent of coverage level |

### Finding Severity Levels

Used in `specs/findings.md`, `specs/escalations.md`, and adversary role output.

| Severity | Meaning | Effect |
|----------|---------|--------|
| `CRITICAL` | Serious defect, security issue, or invariant violation | Hard blocking — adversary gate will not clear until resolved |
| `HIGH` | Important issue requiring prompt attention | Non-blocking but must not be silently dropped; escalated to `specs/escalations.md` if AdvSweep |
| `MEDIUM` | Should be addressed; no immediate danger | Logged; addressed in a future session |
| `LOW` | Minor issue or informational observation | Logged; addressed when convenient |
| `INFO` | No action required; recorded for awareness | Logged only |
