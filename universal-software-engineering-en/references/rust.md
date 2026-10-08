# Rust

## Check commands before reporting done
Run the project's command if it has one. Otherwise:

```bash
cargo fmt --check
cargo clippy --all-targets -- -D warnings
cargo test
```

Use `-D warnings` only when the project already treats warnings as errors, or when creating a new project. Do not add this flag to a project that currently allows warnings.

## Practices
- **Handle errors with `Result` and the crate's existing error type.** Add `anyhow` for an app/CLI, or `thiserror` for a library, only when errors need context or their own type and the standard library would make the code harder to follow, and only when adding a dependency is allowed.
- **No `.unwrap()` in main code paths**, because a panic crashes the program on unexpected data. Use `?`, pattern matching, or `.expect("detailed reason why this cannot happen")`. In tests, `.unwrap()` is fine.
- **Avoid unnecessary `.clone()`**; prefer borrowing (`&str`, `&[T]`) over owning when only reading.
- Avoid over-engineering with complex generics/traits/lifetimes when a concrete type is enough. Abstract only when there are at least 3 real use cases.
- Use the standard library first; think carefully before adding a new crate.
- Use `unsafe` only when truly necessary, with a `// SAFETY:` comment explaining why it is sound.
