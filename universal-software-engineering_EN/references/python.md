# Python

## Default tooling (when the project has none)
- Environment & dependencies: `uv` (use `venv` only if uv cannot be installed)
- Lint & format: `ruff`
- Type checking: `pyright` (or `mypy` if the project already uses it)
- Tests: `pytest`

## Check commands before reporting done
```bash
ruff check .
ruff format --check .
pyright            # or: mypy .
pytest
```

## Practices
- **Full type hints** on parameters and return types of every function. Type hints only have value when pyright/mypy actually checks them.
- Use `pathlib.Path` for paths; do not concatenate strings by hand.
- Use the `logging` module instead of `print()` in application code (print is acceptable in small scripts or intentional CLI output).
- **Catch specific exceptions**; never use a bare `except:` or `except Exception: pass`, because it hides real errors.
- Do not use mutable default arguments (`def f(x=[])`); use `None` and initialize inside the function.
- Read config/secrets from environment variables (`os.environ`); never hardcode them.
