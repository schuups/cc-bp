# Findings

Running log of adversary and auditor findings across all sessions. Append-only — never edit past entries.

---

## Entry Format

```markdown
## <Role> Finding | Auditor Sign-off — YYYY-MM-DD

**Mode:** Feature | Bugfix | Design | Sweep
**Artifact:** <file:line, ADR reference, spec section, or diff description>
**Severity:** CRITICAL | HIGH | MEDIUM | LOW | INFO  ← (findings only; omit for sign-offs)
**Description:** <what the issue is, or sign-off scope>
**Recommendation:** <what should change>  ← (findings only)
**Resolution:** open | resolved: <description> | accepted: <rationale>

---  ← separator between entries
```

**For auditor sign-offs, use this format instead:**

```markdown
## Auditor Sign-off — YYYY-MM-DD

**Result:** PASS | FAIL
**Diff scope:** <one-line description of what was reviewed>
**ADR reference:** <NNNN or "none">
**Findings:**
- <finding or "none">
```

---

<!-- Entries appear below in reverse chronological order (newest first). -->
