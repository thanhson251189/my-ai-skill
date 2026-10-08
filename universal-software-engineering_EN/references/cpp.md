# C / C++

## Tooling
- Format: `clang-format`; static analysis: `clang-tidy`
- Always compile with `-Wall -Wextra -Werror`

## Check commands before reporting done
- Compile cleanly with the warning flags above.
- Run `clang-tidy` and `clang-format --dry-run --Werror`.
- Run tests with a sanitizer to catch memory errors:
  `-fsanitize=address,undefined` (or Valgrind).

## Practices
- **Modern C++ (C++17 and later):** apply RAII thoroughly; use smart pointers (`std::unique_ptr` by default, `std::shared_ptr` only when shared ownership is truly needed).
- **Do not use raw pointers to own memory** (manual `new`/`delete`). Raw pointers are for observing only (non-owning).
- Prefer `std::string_view`, `std::span`, and standard containers over C arrays and pointer + length pairs.
- In plain C: check the return value of every allocation/IO call, and release resources at a single, clear exit point.
