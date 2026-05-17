# Claude Code Blueprint

A reusable `.claude/` scaffolding for software projects. It gives Claude a state machine, a set of behavioral roles, and a library of commands that make development work structured, traceable, and repeatable — regardless of the project.

---

## How to apply this blueprint

### Step 1 — Copy

Copy the `.claude/` folder from this repo into the root of your project, replacing whatever `.claude/` was there before:

```bash
cp -r /path/to/cc-bp/.claude /path/to/your-project/
```

Also copy the root `CLAUDE.md` into your project root:

```bash
cp /path/to/cc-bp/CLAUDE.md /path/to/your-project/CLAUDE.md
```

### Step 2 — Run `/onboard`

Open Claude Code in your project and run:

```
/onboard
```

This is the only mandatory first step. `/onboard` will:

1. **Analyse** the existing codebase (or detect greenfield if empty)
2. **Question** you about the project — purpose, users, features, execution context, constraints, git workflow — until everything is unambiguous
3. **Populate** the root `CLAUDE.md` Project Context section and the knowledge files under `.claude/knowledge/`
4. **Self-delete** — once confirmed, it removes itself and all references to it from the blueprint

### Step 3 — Work

Once onboarding is complete, Claude operates within the framework. Use the commands in `.claude/commands/` as your workflow:

| Command | When to use |
|---------|------------|
| `/ccbp:overview` | Start of every session — situational snapshot |
| `/ccbp:attack` | Before locking any design or architecture decision |
| `/ccbp:audit` | Before committing; periodic health check |
| `/ccbp:checkpoint` | Every commit — verified, gate-checked, rollback-safe |
| `/ccbp:map` | Before any cross-cutting change |
| `/ccbp:pause` / `/ccbp:resume` | Suspend and resume across sessions |
| `/ccbp:cleanup` | Remove stale content; keep knowledge files current |
| `/ccbp:docs` | Rebuild user-facing documentation from knowledge files |

---

## Onboarding state

The blueprint has exactly two states:

**Not yet onboarded** — `.claude/commands/onboard.md` exists and root `CLAUDE.md` contains `<!--` placeholders. Run `/onboard` before doing anything else.

**Onboarded** — `onboard.md` is gone, all `<!--` placeholders are filled in, knowledge files are populated. All commands are operational.

There is no in-between state. `/onboard` either hasn't run or has completed fully and removed itself.

---

## What gets populated

After onboarding, these files will be filled in:

| File | Content |
|------|---------|
| `CLAUDE.md` (root) | Project Context: vision, description, success criteria, stakeholders, technologies, execution context |
| `.claude/knowledge/requirements.md` | Full functional requirements inventory with status |
| `.claude/knowledge/architecture.md` | System boundaries, components, key decisions |
| `.claude/knowledge/coding-guide.md` | Language-specific rules for this project |
| `.claude/knowledge/backlog.md` | Explicitly deferred work items |
| `.claude/knowledge/domain.md` | Domain knowledge captured during discovery |
| `.claude/knowledge/adr/INDEX.md` | ADR index (initialised if not present) |

---

## Greenfield vs brownfield

**Greenfield** (empty project): `/onboard` skips the codebase analysis phase and goes directly to questioning. Gate checks that require existing code (tests, auditor sign-off) are marked N/A until code exists.

**Brownfield** (existing project): `/onboard` reads the codebase first — source files, package/config files, existing docs, git log — before asking questions, so it only asks what the analysis couldn't determine.
