# Working process (applies to every language)

The four groups of rules below are language-independent. For language-specific commands, see the matching file in `references/`.

## 1. Bug fixing: find the root cause before patching

Do not guess and try random fixes. Follow this order:

1. **Reproduce the bug:** run it again to see the real failure, and record the command or steps that trigger it. If you cannot reproduce it, say so and ask for more information instead of fixing based on a hunch.
2. **Read the error message and stack trace** carefully; the failing location usually points the right way.
3. **Narrow the scope:** identify the code, input, or most recent change that causes it.
4. **State a hypothesis about the root cause** and verify it with logs, a debugger, or a small test, not just reasoning.
5. **Write a failing test** that reproduces the bug (when sensible), then make the minimal fix so the test passes. The test stays so the bug does not return.
6. **Re-run all checks** (lint, format, tests) to be sure nothing else broke.

Do not mask symptoms: no empty `try/catch`, no ignoring errors, no deleting a failing test to "make it pass". If you fixed it but cannot explain why the bug happened, treat it as not fixed.

## 2. Stay within scope

- Change only what the user asked for. No drive-by refactors, renames, bulk reformatting, or edits to unrelated files.
- If you notice other problems while working (a bug, ugly code, an outdated dependency), **report it briefly** and let the user decide; do not fix it on your own.
- Keep changes small and reviewable: do one thing at a time so the user can easily check and undo.
- If the request is ambiguous and the different readings would produce very different results, ask one short question before starting.

## 3. Git and operation safety

**Commits:**
- Encourage the user to commit often, one meaningful change per commit (one feature, one bug), with a clear message saying *what and why*.
- Before starting a large change, remind the user to commit the current working state so there is a point to return to.

**Ask for confirmation before irreversible actions:**
- Bulk deleting files or directories, `rm -rf`
- `git reset --hard`, `git clean -fd`, `git push --force`, rewriting history
- `DROP TABLE`, `TRUNCATE`, `DELETE` without `WHERE`, data-destroying migrations
- Overwriting user files/data, changes on a live (production) environment

State what you are about to do and the consequences, then wait for the user's approval. Prefer the safer option when one exists (rename/move instead of delete, back up first, try with `--dry-run`).

**A new project needs:**
- A `.gitignore` suited to the language, so secrets and generated files are not committed (see the table below).
- A `.env.example` listing the needed environment variable names (without real values); the real `.env` goes in `.gitignore`.
- If a secret was committed by mistake, tell the user right away: the secret must be **revoked/rotated**, because removing it from a commit is not enough (git history still keeps it).

## 4. Pinning dependency versions

Use a lockfile and **commit it to git** so the project can be reproduced identically months later. Pin versions deliberately; do not upgrade dependencies on your own unless the user asks.

| Language | Lockfile (commit it) | Add to `.gitignore` |
|---|---|---|
| Python (uv) | `uv.lock` | `.venv/`, `__pycache__/`, `.pytest_cache/`, `.ruff_cache/` |
| Rust | `Cargo.lock` | `target/` |
| TypeScript/JS | `package-lock.json`, `pnpm-lock.yaml`, or `bun.lock` (use exactly one) | `node_modules/`, `dist/`, `.next/` |
| Go | `go.sum` (with `go.mod`) | `bin/`, built executables |
| C/C++ | the lock file of the package manager in use (`conan.lock`, `vcpkg.json`...) | `build/`, `*.o`, `*.exe` |
| Any language | | `.env`, `*.log`, personal IDE files (`.idea/`, `.vscode/` except shared project config) |

Every language adds `.env` to `.gitignore`. If the project uses a package manager other than those in the table, follow the project's tooling.
