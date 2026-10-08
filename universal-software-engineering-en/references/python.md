# Python

## Default tooling (only when the project has none)
- Environment and dependencies: `uv` (use `venv` only if uv cannot be installed)
- Lint and format: `ruff`
- Type checking: `pyright` (or `mypy` if the project already uses it)
- Tests: `pytest`

## Check commands before reporting done
If the project already has a check command (script, Makefile, CI), run that. Do not run `ruff` or `pyright` on a project that uses other tools.

If there is no toolchain and you are creating a new project, install the tools into the project and run them from that environment. Do not use a temporary install outside the lockfile:

```bash
uv add --dev ruff pyright pytest
uv run ruff check .
uv run ruff format --check .
uv run pyright
uv run pytest
```

If an existing project is missing a tool, say so. Do not add it yourself.

## Practices
- **Full type hints** on parameters and return types of every function. Type hints only have value when pyright/mypy actually checks them.
- Use `pathlib.Path` for paths; do not concatenate strings by hand.
- Use the `logging` module instead of `print()` in application code (print is acceptable in small scripts or intentional CLI output).
- **Catch specific exceptions**; never use a bare `except:` or `except Exception: pass`, because it hides real errors.
- Do not use mutable default arguments (`def f(x=[])`); use `None` and initialize inside the function.
- Read config/secrets from environment variables (`os.environ`); never hardcode them.
