# Command: /docs

Rebuilds user-facing documentation from `.claude/knowledge/`.

## What It Rebuilds

1. Root `documentation/` folder — user-facing documentation
2. Root `README.md` — project overview with a brief section on blueprint mechanics

## How It Works

1. Read all files under `.claude/knowledge/`
2. Compare against current `documentation/` and `README.md`
3. Update incrementally — rewrite only what has changed or is missing; do not auto-delete
4. Identify stale sections: documentation covering features, APIs, or concepts no longer present in `.claude/knowledge/`; report these and propose actions rather than deleting automatically
5. Report: what was added, updated, found stale, and any gaps (knowledge content with no corresponding documentation)
