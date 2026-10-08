# Shell / Bash

## Check commands before reporting done
Run `shellcheck` on the file you actually changed. Do not hardcode the name `script.sh`. If several files changed, check each one. Run `shfmt -d` only when the tool is already installed; do not install it yourself.

```bash
shellcheck path/to/the-changed-script.sh
shfmt -d path/to/the-changed-script.sh
```

## Practices
- An executable script starts with:
  ```bash
  #!/usr/bin/env bash
  set -euo pipefail
  ```
  so the script stops on errors, unset variables, and failures inside pipes. Do not add this block to a file that is meant to be `source`d, unless that file already uses it. `grep` and `diff` return 1 when they find no match; do not let `set -e` abort a check that is intentionally looking for "not found".
- **Always quote variables:** `"$VAR"`, `"${arr[@]}"`, to avoid word splitting and globbing problems with filenames containing spaces.
- Use `$(command)` instead of backticks.
- Use `[[ ... ]]` instead of `[ ... ]` in Bash.
- Clean up temp files with `trap '...' EXIT`.
- If the logic gets complex (lots of string/data processing), consider moving to Python instead of forcing it into Bash.
