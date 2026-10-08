# Go

## Check commands before reporting done
```bash
gofmt -l .          # no output means pass
go vet ./...
golangci-lint run
go test ./...
```

## Practices
- **Check errors explicitly right after the call** and wrap them with context:
  ```go
  if err != nil {
      return fmt.Errorf("describe the operation: %w", err)
  }
  ```
- Prefer **table-driven tests** for unit tests.
- Manage resources with `defer` right after successful initialization (close files, unlock mutexes, close response bodies).
- Keep interfaces small and define them where they are used, not where they are implemented.
- For code with goroutines, also run `go test -race ./...`.
