---
name: universal-software-engineering-en
description: English edition. Multi-language software engineering standards for lean, clean code. Use this skill whenever the user writes, edits, refactors, reviews, debugs, or sets up a project in Python, Rust, TypeScript/JavaScript, Go, C/C++, Shell/Bash, SQL, or VBA/Apps Script/Office Scripts, even if they never mention "standards" or "best practices". It defines lint/format/test tooling, error handling, and the definition of "done".
compatibility: No extra runtime. Commands in the reference files are defaults for projects that do not already have a toolchain.
---

# General principles (all languages)

- **Follow the project first.** If the project already has tooling, scripts, a Makefile, CI, or config (pyproject, Cargo.toml, biome.json, .eslintrc...), use exactly that. Commands in the reference files apply only when the project has no equivalent check. Do not install a different tool, and do not reformat the repo to this skill's defaults.
- **Simple and readable, not shortest.** Pick the solution with the fewest moving parts that is still easy to understand. No code golf. Do not create an abstraction until there are at least 3 real places that reuse it.
- **Do not add a runtime dependency** if the standard library can do the job. Every new dependency adds maintenance surface and security risk. Add a linter, formatter, or test runner only while setting up a new project that has no toolchain. If an existing project is missing a tool, say so; do not add it yourself.
- **Never swallow errors silently.** Every error must be logged with context or returned through an explicit error-handling type, because a swallowed error becomes a hard-to-trace bug later. Never log secrets, tokens, or passwords.
- **Never hardcode secrets** (API keys, passwords, tokens). Read them from environment variables or a config file kept out of git.
- **Ask before big changes:** wide refactors, restructuring directories, replacing a main library or framework, adding a toolchain to an existing project.
- **Show the change, do not paste the whole file.** Edit files with the agent's edit tool; pasting a full file is not an edit. For a small change, name the file and the changed block, with a few lines of context. For a large or scattered change, list which files changed and what behavior changed.

# Comments and explanations (users who lean on vibe coding)

The user may not read every line of code, so they need to understand the project at a high level without the code being cluttered.

- **No line-by-line comments**, and no comments that repeat what the code already says clearly (`i += 1  # increment i`).
- **Comment only to explain "why"**: reasons, constraints, or non-obvious trade-offs (for example an API limit, a workaround for a bug, why an algorithm was chosen).
- **Add a short docstring to a public function or module only when the name and signature do not already say** what it does, what it takes, and what it returns. Use the language's native form (Python docstring, Rust `///`, JSDoc/TSDoc, Go doc comment...). Do not write a docstring that only repeats the function name.
- **Use clear names** instead of short names plus explanatory comments.
- **After finishing a feature or a significant change**, give a short summary in plain language: what the code does, how the flow runs, which files changed and why. Avoid jargon when not needed; if used, explain it briefly.
- For new projects, create or update a short `README.md` (or `NOTES.md`): purpose, directory structure, how to run/test, key design decisions.
- If the user asks for a deeper explanation of a specific part, explain it outside the code (in the reply), not by stuffing it into the file.

# Project structure

Organize by feature once a feature is large enough: **one directory for that feature**, with files split by responsibility inside it, **not one file per function**. A small feature stays in one file. Warning thresholds: about 300 lines per file, about 50 lines per function. Those are signals to report, not orders to split. Split thresholds and exceptions are in `references/project-structure.md`. When exceeded, report it and propose how to split, without refactoring on your own. Read that file when creating a new project, adding a new feature, or splitting/merging files.

# Working process

Applies to every language: when fixing a bug, find the root cause before patching (no guessing); stay within the scope of the request, and report other problems you notice instead of fixing them on your own; ask for confirmation before irreversible operations (bulk deletes, `reset --hard`, `DROP TABLE`...); use lockfiles and a correct `.gitignore`. Read `references/workflow.md` when fixing bugs, initializing a project, or about to do something dangerous.

# What "done" means

Work is done only after you have **actually run** the project's check commands and reported the real results. If the project has no check command, use the defaults in the reference file for the language you changed. Never claim "done" without running them. If you cannot run them, say why and give the user the commands to run.

1. The project's linter, formatter, or compiler reports no new warnings caused by this change. Do not turn on `-Werror` or switch linters on your own.
2. The code you changed is formatted with the project's formatter, if it has one.
3. New behavior or a bug you just fixed has a test if the project already has a place for tests. Do not create a new test framework, and do not add a fake test just to make the command exit 0, unless the user asked you to set up tests.

**Exception:** for a throwaway script or prototype, format only if a formatter already exists; tests are optional.

# Choosing a file by language

Read only the file for the language you are working in. Every path in this skill is relative to the directory that contains `SKILL.md`, not the working directory. Check commands in a reference file are the fallback, not a required command when the project already has its own check.

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
