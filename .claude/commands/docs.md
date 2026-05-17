# Command: /ccbp:docs

Rebuilds user-facing documentation from `.claude/knowledge/`. Run explicitly after significant implementation changes or when documentation is known to be stale.

---

## What It Rebuilds

1. `documentation/` folder — user-facing guides and reference docs
2. Root `README.md` — project overview

---

## Step 1 — Map Check

Read `.claude/logs/map.md`. Extract the datetime from `## Project Map — YYYY-MM-DD HH:MM`; compare against `git log -1 --format=%ci`. The map is stale if its datetime is earlier than the last commit timestamp.
- **Absent or older than last commit** → warn "Map may be stale — documentation gaps check may miss recently added components. Run `/ccbp:map` to refresh." Continue unless the user opts to refresh first.
- **Up to date** → use the component list as a coverage checklist: every component in the map should have corresponding documentation.

---

## Step 2 — Read Knowledge Files

Read all files under `.claude/knowledge/`: `requirements.md`, `architecture.md`, `domain.md`, `coding-guide.md`, `adr/`, `backlog.md`.

This is the source of truth. Documentation must reflect what is in these files, not what was previously written in `documentation/`.

---

## Step 2a — Initial Creation Check

Check whether the `documentation/` folder exists.

**Folder absent** → this is a first-time run. State: "No `documentation/` folder found — creating from scratch." Create the folder, then skip to Step 4 and treat every knowledge item as a **Missing** section. Steps 3 and 5 do not apply on initial creation; there is nothing to compare against and nothing stale.

**Folder present** → continue to Step 3.

---

## Step 3 — Compare Against Current Documentation

Read all files under `documentation/` and `README.md`.

For each documentation section, determine:
- **Current** — matches what is in `.claude/knowledge/`; no change needed
- **Outdated** — describes something that has changed; needs rewrite
- **Missing** — knowledge content with no corresponding documentation; needs to be written
- **Stale** — covers a feature, API, or concept no longer present in `.claude/knowledge/`

---

## Step 4 — Update Incrementally

- Rewrite outdated sections to match current knowledge
- Write missing sections
- Do **not** auto-delete stale sections — see Step 4

---

## Step 5 — Handle Stale Sections

For each stale section, propose one of:
- **Delete** — content is fully superseded and has no reference value
- **Deprecate** — add a visible deprecation notice and leave the content for transition period
- **Archive** — move to `documentation/archive/` with a dated note

Present proposals to the user and wait for confirmation before acting.

---

## Step 6 — Report

```
Documentation rebuild — YYYY-MM-DD

Updated:  N sections
Added:    N sections
Stale:    N sections (awaiting confirmation)
Gaps:     N knowledge items with no documentation yet

Actions taken: <list of files changed>
Awaiting confirmation: <list of stale section proposals>
```
