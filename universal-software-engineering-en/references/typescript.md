# TypeScript / JavaScript

## Default tooling (only when the project has none)
- Lint and format: `Biome` (or ESLint + Prettier if the project already uses them)
- Type checking: `tsc --noEmit`, only when the project already has a `tsconfig.json`
- Tests: `Vitest`

## Check commands before reporting done
If the project already has a script, Makefile, or CI check, run that. Do not run Biome on a project that uses ESLint, and do not run Vitest on a project that uses Jest or `node:test`.

Run the binary or script declared by the project. Do not use `npx` to download a package that is not in the lockfile.

If there is no toolchain and you are creating a new project, add Biome, TypeScript, and Vitest as devDependencies, write the lockfile, then run the project's scripts. If an existing project is missing a tool, say so. Do not add it yourself.

A JavaScript project with no `tsconfig.json`: skip `tsc`. Do not add TypeScript on your own.

No test files: say there are no tests. Do not add an empty test, and do not change config just to make the command exit 0.

## Practices
- Enable `"strict": true` in `tsconfig.json` when creating a new TypeScript project. In an existing project, do not turn on strict if that breaks unrelated files.
- **Do not use `any`**; use `unknown` and narrow the type when the data shape is unclear. `any` throws away the whole benefit of TypeScript.
- Handle async code with `async/await`; avoid deeply nested `.then()` chains. Always handle Promise errors (`try/catch` or `.catch`).
- `const` by default; use `let` only when the value truly needs to change. Never use `var`.
- Validate external data (APIs, input, JSON) at the system boundary instead of trusting declared types.
- Never commit secrets; read them from environment variables.
