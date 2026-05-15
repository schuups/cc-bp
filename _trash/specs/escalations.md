# Escalations

Blockers and unresolved issues that require a user decision before work can proceed.

An escalation stays open until the user explicitly closes it with a decision. Closed escalations are kept for history — never delete them.

---

## Open Escalations

<!-- Add entries here when a blocking issue surfaces. An open escalation blocks the mode that raised it from advancing. -->

*(none)*

---

## Closed Escalations

<!-- Move resolved entries here with a resolution note and close date. -->

*(none)*

---

## Entry Format

```markdown
## ESC-NNN: <Short title>

**Opened:** YYYY-MM-DD
**Mode:** Feature | Bugfix | Design | Sweep
**Raised by:** Adversary | Auditor | Integrator | Implementer
**Blocker:** <what is blocked and why it cannot be resolved without a user decision>
**Options:**
1. <option A — describe the tradeoff>
2. <option B — describe the tradeoff>

**User decision:** open | decided: <description of what was chosen and why>
**Closed:** YYYY-MM-DD | open
```
