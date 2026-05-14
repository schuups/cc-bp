# Pre-commit Gate

**All checks must pass before claiming work is done or making a commit. No exceptions without an explicit user override command.**

This gate runs at the end of every Feature, Bugfix, and Sweep mode session that produces a commit.

---

## Checks

### 1. Tests Pass
Run the full test suite: `<test-command>`
(e.g. `pytest`, `npm test`, `go test ./...`, `bundle exec rspec`, `cargo test`)

Required:
- Zero failing tests
- Zero skipped or `xfail` tests without a corresponding `specs/findings.md` entry that explains why

### 2. Auditor Sign-off Present
- `specs/findings.md` contains an auditor sign-off for this specific diff
- The sign-off result is `PASS`
- If the result is `FAIL`: resolve all FAIL findings, re-run the auditor role, and obtain a new PASS sign-off before proceeding

### 3. No Stale Work Items
In the changed files only:
- No `TODO`, `FIXME`, or `HACK` comments without a corresponding `specs/findings.md` entry
- No commented-out code blocks without an explanation
- No `console.log` / `print` / `debugger` / `binding.pry` debug instrumentation left in

### 4. Specs Consistent
- If the change adds or modifies domain concepts: `specs/domain-model.md` is updated
- If the change adds or modifies invariants: `specs/invariants.md` is updated
- If the change was guided by an ADR: the ADR file exists in `specs/adr/` with status `Accepted`
- If new invariants were introduced by the ADR: they appear in `specs/invariants.md`

### 5. Fidelity Consistent
*For Feature and Bugfix checkpoints. Skip for Spec-only and Sweep checkpoints.*

- Coverage rating on all areas changed by this commit is `MODERATE` or higher
- For Feature mode: all changed areas must be `THOROUGH`
- No new `[!] DIVERGENT` entries in `specs/fidelity-index.md` without a corresponding open finding in `specs/findings.md`

### 6. No Stale References
If the diff moves, renames, or deletes any file, directory, function, class, module, or configuration key:
- All imports and in-code references are updated to the new name or path
- All README files, inline docs, and `specs/` entries that pointed to the old form are updated
- No dead links, missing-file references, or cross-references pointing to a name or path that no longer exists

A partial rename is an incomplete change, not an acceptable intermediate state.

### 7. Local Verification (No Subagent-only Confirmation)
- Test results, lint output, and type-check output have been observed in this session
- Do not accept a subagent's summary as a substitute for seeing the output directly

---

## Gate Report Format

Produce this report before committing. All seven must be `[x]`.

```
Pre-commit gate:
[x] Tests pass — <N> tests, 0 failed, 0 skipped
[x] Auditor sign-off: PASS — <YYYY-MM-DD>
[x] No stale work items in changed files
[x] Specs consistent with changeset
[x] Fidelity: THOROUGH (Feature) / MODERATE+ (Bugfix) on changed areas
[x] No stale references (moves/renames/deletes fully propagated)
[x] Verified locally (not subagent-only)
```

Any `[ ]` is a blocker. State what is blocking and what must happen to clear it.
