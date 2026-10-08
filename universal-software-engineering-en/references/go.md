# Go

## Check commands before reporting done
Run the project's command if it has one. Otherwise:

```bash
gofmt -l .          # no output means pass
go vet ./...
go test ./...
```

Run `golangci-lint run` only when the project already configures it or the tool is already installed. Do not install it yourself. For code with goroutines, also run `go test -race ./...` if the toolchain can run the race detector.

## Practices
- **Check errors explicitly right after the call** and wrap them with context:
  ```go
  if err != nil {
      return fmt.Errorf("describe the operation: %w", err)
  }
  ```
- Prefer **table-driven tests** for unit tests when the project already has tests.
- Manage resources with `defer` right after successful initialization (close files, unlock mutexes, close response bodies).
- Keep interfaces small and define them where they are used, not where they are implemented.
