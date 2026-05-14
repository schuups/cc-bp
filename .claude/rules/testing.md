# Rules: Testing

Testing rules for this project. These define what is required, how tests are structured, and the fidelity expectations that connect to `specs/fidelity-index.md`.

---

## What Always Requires Tests

Every item in this list is non-negotiable. A change that falls into these categories without tests does not pass the auditor gate.

- New domain behavior (any new path through the domain model)
- Bug fixes — a failing test that reproduces the bug must exist before the fix
- New invariant enforcement points
- Any code path that handles money, identity, or security-sensitive operations
- Public API endpoints or interfaces

---

## What Does Not Require Tests

- Pure type definitions (interfaces, enums with no logic)
- Scaffolding code (constructors that do no work, empty implementations)
- Generated code
- Log statements and observability instrumentation

---

## Test Tiers

Three tiers prevent slow-test friction in daily work while ensuring thoroughness at each gate. Configure the commands in root `CLAUDE.md`.

| Tier | Runs when | What it includes | Command placeholder |
|------|-----------|-----------------|---------------------|
| **Fast** | Every code edit; pre-commit | Unit tests; fast integration tests; BDD smoke scenarios | `<test-fast>` |
| **Slow** | Pre-PR / pre-merge | Fast tier + slow integration tests + full scenario suite | `<test-slow>` |
| **Full** | Pre-release; nightly CI | Slow tier + end-to-end tests + external integrations | `<test-full>` |

Mark slow tests explicitly so the fast tier skips them:

```python
# pytest
@pytest.mark.slow

# Go
t.Skip("slow: <reason>")

# Jest
test.skip("slow: <reason>", ...)
```

The pre-commit gate runs the **Fast** tier. The **Slow** tier is the PR gate. The **Full** tier gates releases.

---

## Fidelity Expectations by Coverage Level

The fidelity-index uses THOROUGH / MODERATE / LOW / NONE. Tests are the primary evidence for coverage ratings.

| Rating | What the tests must cover |
|--------|--------------------------|
| `THOROUGH` | Happy path + all documented failure modes + boundary conditions + at least one adversary-identified edge case |
| `MODERATE` | Happy path + primary failure modes + at least one boundary condition |
| `LOW` | Happy path only, or tests exist but are shallow (no assertions on side effects) |
| `NONE` | No tests, or tests that always pass regardless of implementation |

Feature mode requires `THOROUGH` on changed areas before done. Bugfix mode requires at least `MODERATE`.

---

## Test Naming

Tests must be readable as specifications:

```
test_<unit>_<condition>_<expected_outcome>
```

Examples:
- `test_payment_when_card_expired_raises_payment_error`
- `test_cart_add_item_when_stock_zero_returns_out_of_stock`

A test name that requires reading the body to understand what it tests is not well-named.

---

## Test Structure

Each test follows Arrange → Act → Assert (no interleaving):

```python
# Arrange
cart = Cart(items=[item_at_price(10)])

# Act
result = cart.apply_discount(percentage=50)

# Assert
assert result.total == Money(5, "USD")
```

---

## Test Isolation

- Each test is independent — no shared mutable state between tests
- Tests do not depend on execution order
- No test reads from or writes to production databases, filesystems, or network endpoints
- External dependencies are replaced with test doubles at the boundary — not deep in the implementation

---

## Mock Discipline

- Mock at system boundaries (HTTP clients, database connections, message queues)
- Do not mock domain logic
- Mocks must match the real interface contract — a diverged mock is a `[!] DIVERGENT` finding in `specs/fidelity-index.md`
- Prefer fakes (in-memory implementations) over mocks (interaction verifiers) for complex dependencies

---

## Slow Tests

Mark tests that take >1 second as slow so they can be excluded from fast local runs:

```python
@pytest.mark.slow          # Python / pytest
```
```javascript
test.skip('slow: ...')     // JavaScript (project convention — adapt)
```
```go
t.Skip("slow: ...")        // Go
```

Slow tests run in CI. They are never permanently skipped — that requires a `specs/findings.md` entry.

---

## Test Commands

<!--
FILL THIS IN per project — reference your language-specific coding guide.
-->

```bash
<test-command>               # full suite
<test-command> --fast        # fast tests only (exclude slow)
<test-command> --coverage    # with coverage report
```
