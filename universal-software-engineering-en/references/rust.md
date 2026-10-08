# Rust

## Check commands before reporting done
```bash
cargo fmt --check
cargo clippy --all-targets -- -D warnings
cargo test
```
`-D warnings` is required so that warnings actually fail the command; without it, "no warnings" is just a claim.

## Practices
- **Error handling:** applications/CLIs use `anyhow::Result` (with `.context(...)`); libraries use `thiserror` to define their own error types.
- **No `.unwrap()` in main code paths**, because a panic crashes the program on unexpected data. Use `?`, pattern matching, or `.expect("detailed reason why this cannot happen")`. In tests, `.unwrap()` is fine.
- **Avoid unnecessary `.clone()`**; prefer borrowing (`&str`, `&[T]`) over owning when only reading.
- Avoid over-engineering with complex generics/traits/lifetimes when a concrete type is enough. Abstract only when there are at least 3 real use cases.
- Use the standard library first; think carefully before adding a new crate.
- Use `unsafe` only when truly necessary, with a `// SAFETY:` comment explaining why it is sound.
