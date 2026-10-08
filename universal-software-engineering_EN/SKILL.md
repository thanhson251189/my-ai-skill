---
name: universal-software-engineering
description: Multi-language software engineering standards for lean, clean code. Use this skill whenever the user writes, edits, refactors, reviews, debugs, or sets up a project in Python, Rust, TypeScript/JavaScript, Go, C/C++, Shell/Bash, SQL, or VBA/Apps Script/Office Scripts, even if they never mention "standards" or "best practices". It defines lint/format/test tooling, error handling, and the definition of "done".
---

# General principles (all languages)

- **Follow the project first.** If the project already has tooling or config (pyproject, Cargo.toml, biome.json, .eslintrc, Makefile...), use exactly that. Apply the defaults in the reference files only when the project has nothing set up.
- **Simple and readable, not shortest.** Pick the solution with the fewest moving parts that is still easy to understand. No code golf. Do not create an abstraction until there are at least 3 real places that reuse it.
- **Do not add dependencies** if the standard library can do the job. Every new dependency adds maintenance surface and security risk.
- **Never swallow errors silently.** Every error must be logged with context or returned through an explicit error-handling type, because a swallowed error becomes a hard-to-trace bug later.
- **Never hardcode secrets** (API keys, passwords, tokens). Read them from environment variables or a config file kept out of git.
- **Ask before big changes:** wide refactors, restructuring directories, replacing a main library or framework.
- **For small edits, show only the changed part** (the function or block) with a few lines of surrounding context so the location is clear. If the change is large or spread across many places, print the whole file for easier review.

# Comments and explanations (users who lean on vibe coding)

The user may not read every line of code, so they need to understand the project at a high level without the code being cluttered.

- **No line-by-line comments**, and no comments that repeat what the code already says clearly (`i += 1  # increment i`).
- **Comment only to explain "why"**: reasons, constraints, or non-obvious trade-offs (for example an API limit, a workaround for a bug, why an algorithm was chosen).
- **Every public function/module gets a short docstring** (one or two sentences): what it does, what it takes, what it returns. Use the language's native form (Python docstring, Rust `///`, JSDoc/TSDoc, Go doc comment...).
- **Use clear names** instead of short names plus explanatory comments.
- **After finishing a feature or a significant change**, give a short summary in plain language: what the code does, how the flow runs, which files changed and why. Avoid jargon when not needed; if used, explain it briefly.
- For new projects, create or update a short `README.md` (or `NOTES.md`): purpose, directory structure, how to run/test, key design decisions.
- If the user asks for a deeper explanation of a specific part, explain it outside the code (in the reply), not by stuffing it into the file.

# Project structure

Organize by feature: **one directory per feature**, with files split by responsibility inside it, **not one file per function**. Warning thresholds: about 300 lines per file, about 50 lines per function; when exceeded, report it and propose how to split, without refactoring on your own. Read `references/project-structure.md` when creating a new project, adding a new feature, or splitting/merging files.

# Working process

Applies to every language: when fixing a bug, find the root cause before patching (no guessing); stay within the scope of the request, and report other problems you notice instead of fixing them on your own; ask for confirmation before irreversible operations (bulk deletes, `reset --hard`, `DROP TABLE`...); use lockfiles and a correct `.gitignore`. Read `references/workflow.md` when fixing bugs, initializing a project, or about to do something dangerous.

# What "done" means

Work is done only after you have **actually run** the language's check commands (see the reference file) and reported the real results. Never claim "done" without running them.

1. The linter/compiler reports no warnings.
2. The code is formatted to the standard.
3. There are tests for the main flow and edge cases, and they pass.

**Exception:** for throwaway scripts or prototypes, format + lint is enough; tests are optional. If you cannot run the commands (missing tools, no environment), say so clearly and list the commands for the user to run.

# Choosing a file by language

Read only the file for the language you are working in:

| Topic / language | File |
|---|---|
| Directory structure, splitting/merging files | `references/project-structure.md` |
| Bug fixing, git, dangerous operations, lockfiles | `references/workflow.md` |
| Python | `references/python.md` |
| Rust | `references/rust.md` |
| TypeScript / JavaScript | `references/typescript.md` |
| Go | `references/go.md` |
| C / C++ | `references/cpp.md` |
| Shell / Bash | `references/bash.md` |
| SQL / Database | `references/sql.md` |
| VBA / Apps Script / Office Scripts | `references/office-automation.md` |

If the project uses several languages, read the file for each relevant language.
