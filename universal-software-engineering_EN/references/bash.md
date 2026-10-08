# Shell / Bash

## Check commands before reporting done
```bash
shellcheck script.sh
shfmt -d script.sh     # if shfmt is installed
```

## Practices
- Always start with:
  ```bash
  #!/usr/bin/env bash
  set -euo pipefail
  ```
  so the script stops on errors, unset variables, and failures inside pipes.
- **Always quote variables:** `"$VAR"`, `"${arr[@]}"`, to avoid word splitting and globbing problems with filenames containing spaces.
- Use `$(command)` instead of backticks.
- Use `[[ ... ]]` instead of `[ ... ]` in Bash.
- Clean up temp files with `trap '...' EXIT`.
- If the logic gets complex (lots of string/data processing), consider moving to Python instead of forcing it into Bash.
