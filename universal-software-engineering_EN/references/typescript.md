# TypeScript / JavaScript

## Default tooling (when the project has none)
- Lint & format: `Biome` (or ESLint + Prettier if the project already uses them)
- Type checking: `tsc --noEmit`
- Tests: `Vitest`

## Check commands before reporting done
```bash
npx biome check .
npx tsc --noEmit
npx vitest run
```

## Practices
- Enable `"strict": true` in `tsconfig.json`.
- **Do not use `any`**; use `unknown` and narrow the type when the data shape is unclear. `any` throws away the whole benefit of TypeScript.
- Handle async code with `async/await`; avoid deeply nested `.then()` chains. Always handle Promise errors (`try/catch` or `.catch`).
- `const` by default; use `let` only when the value truly needs to change. Never use `var`.
- Validate external data (APIs, input, JSON) at the system boundary instead of trusting declared types.
- Never commit secrets; read them from environment variables.
