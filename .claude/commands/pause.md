# Command: /ccbp:pause

Capture the current session state so work can resume exactly where it left off. Use before a multi-day suspension, a handoff, or any break long enough that context will be lost.

---

## Step 1 — Capture State Tuple

Record the current state:

```
State at pause:
  interaction_mode: <value>
  phase:            <value>
  activity:         <value>
  role:             <value>
  scope:            <value>
  urgency:          <value>
```

---

## Step 2 — Summarise In-Progress Work

Write a brief summary covering:
- **What was being worked on**: the specific task, file, or decision in progress
- **What is done**: completed steps in the current task
- **What is not done**: remaining steps, open questions, blockers
- **Decisions made this session**: any choices logged or resolved informally
- **Next action**: the single most important thing to do when resuming

---

## Step 3 — Append to Interaction Log

Append to `.claude/knowledge/interaction-log.md`:

```
## Session pause — YYYY-MM-DD HH:MM

State: [interaction_mode, phase, activity, role, scope, urgency]

In progress: <one sentence>
Done: <bullet list>
Not done: <bullet list>
Open questions: <bullet list, or "none">
Next action: <one sentence>
```

---

## Step 4 — Confirm

State: "Session state saved to `.claude/knowledge/interaction-log.md`. Resume with `/ccbp:resume`."
