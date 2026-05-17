# TODO

Status: 🔴 open · 🟠 in progress · ✅ closed

Attack sweep completed: 2026-05-17

---

1. 🟠 **Blueprint testing** — the blueprint is now substantially complete. Test suite to be written; once done, move to `backlog.md` with a concrete activation criterion. (R10)

2. 🔴 **Competitive comparison** — survey established Claude Code workflow frameworks, prompting systems, and agent scaffolding approaches available online. Identify what this blueprint does better, what it misses, and whether any patterns are worth adopting. Produce a findings entry in `knowledge/domain.md`.

3. 🔴 **Compaction for Claude effectiveness** — the blueprint files have grown verbose through iterative additions. Review all command and role files for redundancy, over-specification, and prose that restates what the state machine already enforces. Goal: reduce token load per session without losing any behavioural guarantee.

4. 🔴 **Simplify** - identify elements that can be stripped out.

18. 🔴 **Consider an `assess` phase** — the current phase inventory has no natural home for structured evaluation of an existing artifact (e.g. "evaluate this blueprint for ML effectiveness"). `amendment` implies a change is already intended; `audit` checks spec-implementation alignment; `research` is about gathering domain knowledge. An `assess` phase would cover: fitness-for-purpose reviews, technology evaluations, post-mortems, and gap analyses where the *outcome* (change, defer, or accept) is not yet decided. Needs: entry criteria, output artifact (findings doc), and a clear exit into `amendment` or `backlog`.

---

## Gap Analysis — Structural / Coverage (evaluated 2026-05-17)

Scope: both software and ML/DL projects. Deep learning (PyTorch/JAX/TF) is the primary ML target.

### ML / Deep Learning gaps

5. 🔴 **Add `experiments.md` knowledge file** — DL projects generate training runs as primary artifacts. `domain.md` is for research findings (confidence-labeled), not for experiment outcomes (run config, metrics, decision taken). Without a structured artifact, experiment history is lost between sessions. Add a template with: run ID, date, hypothesis, config highlights (model, optimizer, LR, batch size), key metrics, outcome, and what decision it drove. `overview` and `checkpoint` should reference it.

6. 🔴 **Develop the `experimentation` phase** — The phase appears in the inventory but has no command, no entry/exit criteria beyond a one-line gate, and no output artifacts. For DL this is often the *primary* phase, not an optional detour. Needs: stopping-rule format (when to abandon vs. continue), run log pointer, how outcome feeds `domain.md` and `experiments.md`, success and abandon paths. Consider a `/ccbp:experiment` command or a dedicated section in `map.md`.

7. 🔴 **Deepen DL declinations across roles** — Several DL-specific failure modes and decisions are absent:
   - **Attacker `:ml-engineer`:** add gradient instability, silent NaN propagation in training, LR schedule failure (warmup/decay misconfiguration), FP16 overflow, loss spike without recovery.
   - **Architect `:ml-engineer`:** add mixed precision decisions (FP16/BF16/FP8), gradient accumulation strategy, activation checkpointing, distributed training topology (DDP vs. FSDP vs. pipeline parallel), framework lock-in risk (PyTorch vs. JAX deployment path).
   - **Implementer `:ml-engineer`:** add deterministic CUDA handling (`torch.use_deterministic_algorithms`), dataloader worker seed handling, gradient clipping as a standard pattern, mixed precision training scaffold.
   - **Analyst `:ml-engineer`:** add GPU budget and training time budget as required elicitation items alongside inference SLA.

8. 🔴 **Add dataset management concept** — DL projects must track dataset versions, train/val/test splits, preprocessing pipeline state, and data lineage. No knowledge artifact currently covers this. Options: `knowledge/datasets.md` (separate file) or a "Datasets" section added to `architecture.md`. The analyst `:ml-engineer` and architect `:ml-engineer` declinations should prompt for dataset card info (source, version, license, class balance, known biases).

9. 🔴 **Add notebook workflow guidance** — DL projects typically start in Jupyter notebooks and graduate to scripts. The blueprint has no guidance on: when to graduate, how notebook experiments relate to `experiments.md`, or how to treat notebooks in the `map.md`. The implementer `:ml-engineer` declination should address this lifecycle.

### Structural gaps (software + ML)

10. 🔴 **Add lightweight decision track** — The ADR process (Context / Options / Decision / Consequences / Acceptance Criteria) is the right tool for architectural decisions but too heavy for mid-tier choices (optimizer selection, preprocessing approach, library pick within a known category). Consider a "Decision Log" section in `interaction.md` or a `decisions.md` lightweight format for choices that need traceability without a full ADR.

11. 🔴 **Address role rigidity during maintenance/debugging** — The rule "exactly one role at a time; complete the first fully before transitioning" is correct for design and architecture phases but creates unnecessary friction during maintenance, where analyst + implementer + auditor activities naturally interleave within a single bug-fix cycle. Add an explicit exemption or a "maintenance mode" that permits role blending under defined conditions (e.g., scope is `local`, urgency is `standard` or lower, no ADR is pending).

12. 🔴 **Add model card concept to documentarian role** — For DL projects, a model card is the standard artifact documenting purpose, training data, limitations, intended use, and known failure modes. The documentarian role has no equivalent output type. Add to the `:ml-engineer` declination.

---

## Compaction Analysis (evaluated 2026-05-17)

Goal: reduce constant token load per session without losing behavioural guarantees. Items are prioritized by impact — files loaded most often get addressed first.

### Constant session load (highest impact)

13. 🔴 **Compact `.claude/CLAUDE.md`** — This file is loaded every session and is the largest single source of constant overhead. Specific candidates:
   - **State Transition Triggers / Implicit Triggers section** (~200 tokens): restates what the decision checklist already enforces; redundant.
   - **Within-Response State Tracking section** (~100 tokens): repeats rules already stated in the Activity section above it.
   - **Role inference table** (~100 tokens): useful but could be compressed to a single line per role (remove the prose "Infer when…" column, keep keyword triggers only).
   - **Phase sequence narrative** (~80 tokens): the arrow diagram plus the prose paragraph below it cover the same information twice.
   - Estimated saving: **~400–500 tokens** per session.

14. 🔴 **Compact knowledge file prose headers** — Each knowledge file (requirements.md, architecture.md, domain.md, coding-guide.md, backlog.md) opens with a multi-line `> **Purpose:** / **Producers:** / **Consumers:** / **Column definitions:**` block. These are loaded on every `/ccbp:overview` run (6 files × ~80–120 tokens each). The information in these headers is redundant with `CLAUDE.md`'s Folders and Files section and the command descriptions. Move to a single `knowledge/README.md` read once during onboarding, or strip to a one-line comment. Estimated saving: **~500–600 tokens** per overview invocation.

### On-demand load (lower impact, still worth doing)

15. 🔴 **Compact role file declination boilerplate** — Every declination opens with "Only domain-specific additions; the contract above applies in full." This is implied by the structure and adds ~14 tokens × (7 roles × ~6 declinations) = ~600 tokens across all role files. The boilerplate is invisible when a role isn't active, but cleaning it improves readability and sets a tighter precedent.

16. 🔴 **Resolve attack vector duplication** — The attacker role says "Authoritative list and per-vector guidance: `.claude/commands/attack.md` Step 1. That table is the single source of truth — do not duplicate it here." This is correct design, but the nine vectors are then *named* in the attack command and implicitly referenced in the role's binding checklist. Verify there is no duplication in practice after compaction of CLAUDE.md; if the role's "Nine Attack Vectors" header is empty (it currently redirects), remove the heading entirely.

17. 🔴 **Compress `overview.md` command** — The overview command has 7 steps with detailed sub-bullets. Steps 2–6 are read-only file checks that could be expressed as a compact checklist with file paths and decision rules, rather than prose. Estimated saving: ~300 tokens per invocation (the command file is loaded when `/ccbp:overview` runs).