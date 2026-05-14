# Python Coding Guide

Load this file when activating the **Architect** or **Implementer** role on a Python project. These are behavioral constraints for writing Python in this codebase, not suggestions.

---

## Style and Type Safety

- Follow PEP 8. Line length: 88 characters (Black default).
- **Type hints are required** on all function signatures and class attributes. No `Any` without a comment explaining why.
- Use `from __future__ import annotations` for forward references.
- Prefer `pathlib.Path` over `os.path` for all filesystem operations.
- Use `dataclasses` (or `pydantic` if validation is needed) for data-carrying objects. No plain dicts as function return values.

## Testing (pytest)

- Test files: `test_<module>.py` adjacent to the module or in a `tests/` directory.
- Use fixtures over `setUp`/`tearDown`. Fixtures should be the smallest scope that makes the test work.
- Parametrize when the same logic is tested with multiple inputs: `@pytest.mark.parametrize`.
- Mark slow tests explicitly: `@pytest.mark.slow` — never skip them silently.
- No `assert` in non-test code for invariant enforcement; raise a domain exception instead.
- Test commands: `pytest` (fast), `pytest -m slow` (slow), `pytest --cov` (coverage).

## Error Handling

- No bare `except:` or `except Exception:` without re-raising or logging with context.
- Prefer domain-specific exceptions (`class PaymentError(ValueError): ...`) over generic ones.
- Use `contextlib.suppress` only for genuinely expected, ignorable conditions — comment why.
- Log exceptions with `logger.exception(...)` to preserve tracebacks.

## Patterns

- Use `logging.getLogger(__name__)` — never `print()` in production code.
- Use `__slots__` on hot-path data classes to reduce memory overhead.
- Use `@property` for computed attributes; avoid `get_X()` naming.
- Prefer explicit `__init__.py` re-exports over deep import paths in public APIs.
- `if __name__ == "__main__":` guard in any executable module.

## Anti-Patterns to Avoid

- Mutable default arguments: `def f(x=[])` — always use `None` and guard inside.
- Catching and re-raising without context: `raise e` loses the traceback; use `raise` or `raise NewError(...) from e`.
- Wildcard imports: `from module import *` — always import explicitly.
- Deeply nested closures — extract to named functions.
- `global` variables — pass state explicitly or use a context object.

## Lint and Type-Check Commands

Substitute for `<lint-command>` in role checklists:

```bash
ruff check .          # fast linting
mypy .                # type checking
black --check .       # format check (use `black .` to fix)
```

Run all three before the pre-commit gate. All three must pass with zero errors.

## Dependency Management

- Use `pyproject.toml` (PEP 621) for project metadata and dependencies.
- Pin direct dependencies with version ranges; lock with `pip-compile` or `uv lock`.
- Dev dependencies (pytest, ruff, mypy, black) in `[project.optional-dependencies.dev]`.
- Never add a runtime dependency for something the standard library provides.
