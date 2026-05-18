# Command: /ccbp:resume

Read the latest session summary and restore full context before acting. Always run after a break; pairs with `/ccbp:pause`.

---

## Step 1 — Read the Pause Entry

Read the most recent pause entry in `.claude/knowledge/interaction-log.md` — the last `## Session pause —` block.

If no pause entry exists: note this and proceed to Step 2 using only the current file state.

---

## Step 2 — Restore State

Set the state tuple from the pause entry:

```
Resuming:
  interaction_mode: <value>
  phase:            <value>
  activity:         <value>
  role:             <value>
  scope:            <value>
  urgency:          <value>
```

If any attribute is ambiguous or missing: ask the user before proceeding.

---

## Step 3 — Verify Current File State

Read the files relevant to the paused work:
- Any source files mentioned in the pause summary
- `.claude/knowledge/requirements.md` — confirm requirements status unchanged
- `.claude/knowledge/attack-log.md` and `.claude/knowledge/audit-log.md` — confirm no new open findings since pause

**Map staleness check:** read `.claude/knowledge/project-map.md`, extract the datetime from `## Project Map — YYYY-MM-DD HH:MM`, and compare against `git log -1 --format=%ci` (last commit timestamp). The map is stale if its datetime is earlier than the last commit timestamp.
- **Map absent** → warn: "No project map found — structure changes since pause are invisible. Run `/ccbp:map` before any global-scope work."
- **Map older than last commit** → warn: "Map may be stale (generated: DATE · last commit: DATE) — project structure may have changed during the break. Run `/ccbp:map` to refresh before global-scope work."
- **Map up to date** → no action needed.

Flag any divergence between the pause summary and current file state.

---

## Step 4 — Surface Resumption Brief

State:

```
Resuming from: YYYY-MM-DD HH:MM
State: [interaction_mode, phase, activity, role, scope, urgency]

Was in progress: <one sentence>
Done: <list>
Still to do: <list>
Open questions: <list, or "none">
Divergence since pause: <list, or "none">

Ready to continue with: <next action>
```

Wait for the user to confirm before acting.
