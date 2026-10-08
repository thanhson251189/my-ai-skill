# C / C++

## Default tooling (only when the project has none)
- Format: `clang-format`, if it is already available.
- Warnings when compiling new files: GCC/Clang use `-Wall -Wextra`; MSVC uses `/W4`. Do not turn on `-Werror` or `/WX` unless the project already does.
- Static analysis: `clang-tidy` only when the project already uses it or the tool is already installed.

## Check commands before reporting done
Run the project's build and test target (`cmake`, `make`, Meson, MSBuild...). Keep its warning flags and test runner.

If there is no build system, compile the file you changed with the warning flags above, then run tests if any exist. Do not invent a test runner.

Use a sanitizer (`-fsanitize=address,undefined`, or MSVC AddressSanitizer when that compiler has it) only when the compiler supports it and it does not break the current build.

## Practices
- **Modern C++ (C++17 and later):** apply RAII thoroughly; use smart pointers (`std::unique_ptr` by default, `std::shared_ptr` only when shared ownership is truly needed).
- **Do not use raw pointers to own memory** (manual `new`/`delete`). Raw pointers are for observing only (non-owning).
- Prefer `std::string_view`, `std::span`, and standard containers over C arrays and pointer + length pairs.
- In plain C: check the return value of every allocation/IO call, and release resources at a single, clear exit point.
