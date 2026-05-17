# TODO

Status: 🔴 open · 🟠 in progress · ✅ closed

Attack sweep completed: 2026-05-17

---

## HIGH — Breaks reliability or correctness in practice

1. ✅ **Attack findings never close** — `attack.md` is append-only and has no `resolved` status or field. The rule "the most recent entry per target is authoritative" is prose-only. A re-run that misses a prior finding silently drops it from the open set. Consumers scan narratively and can miss drift. Fix: add a `Status` column to the findings table (OPEN / RESOLVED / ACCEPTED-RISK) and include a resolution row or supersede declaration when a subsequent sweep of the same target clears a finding.

2. ✅ **"Current session" validity criterion is not machine-readable** — `checkpoint.md` Check 6 says an audit PASS is valid if "written in the current session" even if the HEAD hash doesn't match. Fresh contexts loading the same files cannot determine what was "this session." The escape hatch undermines the integrity check it nominally provides. Fix: remove the "current session" escape; require the audit HEAD hash to match `git rev-parse --short HEAD` exactly, or require a new audit run if it doesn't.

3. ✅ **Template placeholder rows bypass audit preconditions** — `requirements.md` ships with `REQ-001 | | planned | |` (empty content). The audit precondition checks "requirements.md has no populated rows" — this row passes because it exists, but the content is empty. Any audit on an un-onboarded project silently proceeds against a null requirement. Fix: change the precondition check to require at least one row with non-empty requirement text, or remove the template row entirely and document its absence as the "not yet populated" signal.

4. ✅ **No recovery path for partial onboarding** — if `/onboard` is interrupted mid-execution (context window, user abort), the project is in an ambiguous state: some knowledge files populated, others still templates. The only signal for "onboarding done" is the deletion of `onboard.md` — a binary that gives no information about partial progress. Fix: add a Phase 0 to `/onboard` that checks and reports which knowledge files have template-only content and which have been populated, so partial-onboard recovery is explicit.

5. ✅ **Memory system (`.claude/memory/`) is undocumented and inherited** — the `.claude/memory/` folder exists in the blueprint and gets copied into every project via `cp -r`. It contains project-specific memory entries (`project_blueprint.md`, `feedback_claude_next.md`) that are irrelevant and potentially misleading in consumer projects. Nothing in the README, CLAUDE.md, or knowledge files acknowledges this folder or tells adopters what it is. Fix: exclude `.claude/memory/` from the recommended copy command, or document it explicitly and ship an empty `MEMORY.md` index with a template.

6. ✅ **No blueprint versioning or migration path** — the blueprint has no version number, CHANGELOG, or release notes. When a consumer copies the blueprint, there is no mechanism to know which version they have or whether a bugfix applies to them. This becomes increasingly important as the blueprint stabilizes. Fix: add a `VERSION` field to `CLAUDE.md` (or a standalone `.claude/VERSION` file), a CHANGELOG.md, and a migration guide section in the README covering "how to update an existing project."

7. ✅ **No LICENSE file** — the project is designed to be copied into other projects. Without a license, the legal terms for use, modification, and redistribution are undefined. Fix: add a LICENSE file. MIT is the natural choice for a reusable developer tool.

8. ✅ **`CLAUDE_NEXT.md` is referenced in memory but does not exist** — `.claude/memory/feedback_claude_next.md` describes `CLAUDE_NEXT.md` as an active working file and instructs Claude never to edit it without permission. The file does not exist in the repository. Every future session will load this memory and potentially act on stale instructions about a non-existent file. Fix: either restore the file if the refactoring is still in progress, or update the memory entry to reflect its completion and removal.

---

## MEDIUM — Degrades correctness or maintainability over time

9. ✅ **Interaction log grows unbounded and has no archive strategy** — `interaction.md` is only cleared by `/checkpoint`. In long sessions with many decisions, it grows large and consumes significant context on every overview/resume/checkpoint run. `attack.md` and `audit.md` also grow forever with no truncation, archival, or compaction guidance. Fix: define a maximum line count or session count for `interaction.md` before auto-archival, and add a `/ccbp:cleanup` step to propose archiving old log segments to a dated file.

10. ✅ **Nine attack vectors are duplicated with diverging text** — the vector list appears in both `commands/attack.md` (authoritative command spec) and `roles/attacker.md` (role contract). The Performance vector reads: *"Latency, throughput, memory, I/O bottlenecks, algorithmic complexity"* in attack.md vs *"latency, throughput, memory, I/O bottlenecks"* in attacker.md (drops "algorithmic complexity"). If one is updated, the other will drift. Fix: make `roles/attacker.md` reference the vector list in `commands/attack.md` rather than duplicating it, or extract the vectors to a shared section in `CLAUDE.md`.

11. ✅ **Urgency attribute is defined but never enforced** — the state machine defines `critical` urgency as "require explicit user confirmation for non-trivial changes", but no command or role reads or enforces the urgency value. It exists only in the state tuple and the decision checklist intro. Claude must remember to consult it; there is no cross-reference to urgency in any command's preconditions or step instructions. Fix: add an urgency check to the decision checklist in each command that makes non-trivial changes (checkpoint, attack, audit, onboard), explicitly gating actions when urgency is `critical`.

12. ✅ **State transition logging format is orphaned in the spec** — `CLAUDE.md` defines a mandatory log format for state transitions (`-----\nFrom: [...]\nTo: [...]\nTrigger: ...`) but no command or role file references it. Claude has to remember to use it with no prompt or reinforcement in the places where transitions actually occur. Fix: add a "Log this transition to `interaction.md`" instruction to each command that causes a phase change.

13. ✅ **`/onboard` brownfield path has no sensitive data guidance** — Phase 1 reads "source files, package/config files, existing docs, git log, CI/CD configuration" for brownfield projects, but provides no instruction to avoid or redact secrets found in those files. A legacy project with credentials in config files (common in older codebases) would expose those credentials during onboarding analysis. Fix: add a pre-read sensitive file check mirroring the checkpoint sensitive pattern list; if patterns match, warn and ask the user before proceeding.

14. ✅ **`interaction.md` "cleared by checkpoint" is indistinguishable from "no activity"** — after `/checkpoint` runs, it adds `Previous commit: <hash>` and clears other entries. If a session starts, no work happens, and overview is run, it shows "no pending log entries" — identical to "checkpoint ran and nothing happened since." There is no record of _when_ the last checkpoint occurred or what it covered. Fix: when checkpoint clears interaction.md, append a timestamped summary of what was cleared (e.g., "Cleared N entries as of YYYY-MM-DD HH:MM").

15. ✅ **`/overview` is described as "automatically invoked at session start" but is not** — this is a behavioral aspiration, not a technical guarantee. No hook or Claude Code mechanism enforces it. In practice, users must manually type `/ccbp:overview` or know to expect Claude to run it. Fix: either implement it as an actual Claude Code hook (if the platform supports it), or reword the description to "Run at session start" (imperative, user-triggered) rather than "Invoked automatically."

16. ✅ **`/attack` sweep has no stopping criterion or depth guidance** — "run all nine vectors across the codebase systematically" gives no guidance for large codebases: which files take priority, how deep to go, when the sweep is "done." For a large brownfield project, this is effectively open-ended. Fix: add a Step 0 to sweep mode that catalogs files by category (source, config, docs, tests) and defines a scan order with explicit coverage criteria (e.g., "every file in scope" vs "a representative sample").

17. ✅ **`/ccbp:docs` does not handle initial creation (no existing `documentation/` folder)** — Step 3 compares against "all files under `documentation/`" — but if the folder doesn't exist yet, there's nothing to compare against. The command falls through to "write missing sections" implicitly, but never states what the initial documentation structure should be. Fix: add a Step 2a check: if `documentation/` does not exist, create it and document that this is initial creation, then proceed to Step 4 (write all sections as new).

18. ✅ **Sensitive file check in `/checkpoint` is incomplete** — the list covers `.env`, `*.key`, `*.pem`, etc., but omits `id_rsa` and `id_ed25519` (SSH keys without extension), `*.sqlite`/`*.db` (embedded databases with potentially sensitive data), `*.p8`, `*.crt`, `*.cer`, `gcreds.json`, `application-prod.yml`, and similar production config patterns. Fix: expand the sensitive pattern list to include common SSH key filenames and database files; consider a note that the list is a heuristic and not exhaustive.

19. ✅ **`/ccbp:resume` does not re-read the map** — Step 3 reads requirements.md, attack.md, and audit.md to verify state since pause, but does not re-read `map.md`. If the project structure changed significantly between pause and resume (refactor, new components), Claude resumes with a stale understanding of the codebase. Fix: add a map staleness check to Step 3 mirroring the check in `/audit` and `/cleanup`.

20. ✅ **CI/CD information is read during onboarding but has no designated storage** — `onboard.md` Phase 1 explicitly reads CI/CD configuration, but neither `architecture.md` nor any other knowledge file has a dedicated CI/CD section. Any CI/CD insight from brownfield analysis disappears after onboarding. Fix: add a CI/CD section to `architecture.md`'s "Infrastructure and Environment" or create a dedicated `coding-guide.md` subsection for build/deploy conventions.

21. ✅ **ADR acceptance criteria have no machine-verifiable form** — ADR checkboxes are Markdown `- [ ]` / `- [x]` notation. The `/audit` command checks them by having Claude read the criterion text and assess the code — a purely narrative check. A Claude session that disagrees with the ADR author's criteria definition can mark items PASS incorrectly with no detection mechanism. Fix: require at least one acceptance criterion per ADR to be a grep-verifiable code symbol or test name, making the audit step partially mechanical.

22. ✅ **`/onboard` autonomous-mode escape** — onboard Step 4 says "Show a summary and ask the user to confirm before finalising" and Step 5 runs self-cleanup "once the user confirms." But in `autonomous` interaction mode, Claude is authorized to "resolve ambiguity within the evaluation criterion." A confirmation request might be interpreted as "proceed" if there's no explicit evaluation criterion blocking self-cleanup. Fix: mark the onboard confirmation gate as a mandatory STOP that applies regardless of interaction mode.

---

## LOW — Quality of life, future-proofing, minor inconsistencies

23. ✅ **Map date format is date-only; git timestamp is datetime** — `map.md` records `## Project Map — YYYY-MM-DD`. `git log -1 --format=%ci` returns a full datetime. A map generated at 23:59 and a commit made at 00:01 the next day would falsely appear stale when comparing date-only strings. Fix: record the map timestamp as a full ISO-8601 datetime (`YYYY-MM-DD HH:MM`) and compare against the full git datetime.

24. ✅ **`/ccbp:overview` "No other output" instruction is overly rigid** — the command ends "No other output. The user decides what to do next." This prevents Claude from adding useful context even when genuinely needed (e.g., a CRITICAL finding is open). Fix: revise to "No other output unless there are CRITICAL findings, in which case surface them immediately below the report."

25. ✅ **"Decision" is undefined for logging purposes** — `interaction.md` logging rules say "every decision and agreed intention must be logged automatically," but "decision" is never defined. Is a phrasing preference a decision? Is user approval of a proposed path a decision? The ambiguity causes inconsistent logging and incomplete audit trails. Fix: define "decision" as "any choice between named alternatives that affects scope, approach, or deliverable" in the logging rules section.

26. ✅ **Role `implementer:*` MUST NOT "make architectural decisions" conflicts with micro-decisions** — implementation always requires thousands of micro-decisions (variable naming, null handling, local structure). CLAUDE.md defines "micro-decisions authorized by the prior `decide`" as exempt, but the boundary is subjective and Claude will draw it differently each session. Fix: add a concrete example of what counts as a "material change → back to develop" (e.g., "switching to a different library than what the ADR named") vs. an authorized micro-decision.

27. 🟠 **Blueprint testing** — deferred; the blueprint is now substantially complete. Test suite to be tackled; once done, move the work item to `backlog.md` with a concrete activation criterion. (R10)

28. ✅ **`.claudeignore` references `_trash/` with no documentation** — `_trash/` is excluded by `.claudeignore` but is never mentioned in any command, knowledge file, or README. Users inheriting the blueprint won't know what this directory is for, whether it's safe to delete, or whether `/ccbp:cleanup` should use it. Fix: add a one-line comment in `.claudeignore` explaining the purpose of `_trash/`, and document it in `/ccbp:cleanup` as an optional staging area for files proposed for deletion but not yet removed.

29. ✅ **`/ccbp:attack` "mitigation hint — direction only" boundary is vague** — the distinction between a "mitigation hint" and "a design decision" is not defined. Claude will interpret this boundary inconsistently across runs, sometimes providing architectural guidance that should belong to the architect role, and sometimes withholding useful hints. Fix: define the boundary with an example: "A mitigation hint names the category of fix (e.g., 'add rate limiting') without specifying implementation (e.g., not 'use Redis sorted sets at 100 req/min')."

30. ✅ **Root `CLAUDE.md` has two "Explicitly out of scope" declarations** — the root template field "Explicitly out of scope" and `architecture.md`'s "Explicitly out of scope" section cover overlapping territory with no stated relationship. After onboarding, both may be populated independently and may drift. Fix: clarify their relationship: root CLAUDE.md should contain _project_-level scope (what the project doesn't do), while architecture.md should contain _system_-level scope (what the system component doesn't own). Document this distinction in the template comments.

