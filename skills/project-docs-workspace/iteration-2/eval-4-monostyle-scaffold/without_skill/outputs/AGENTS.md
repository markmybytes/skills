# AGENTS.md

Guidance for AI coding agents and humans working in this repository.

## Project overview

`notesync` is a small Go CLI that walks a directory, collects every `*.md` file, and writes a JSON index (`path`, `size`, `modified` per entry). The CLI logic lives in `main.go`; persistence lives in `internal/store`.

## Layout

- `main.go` — flag parsing, directory walk, entry collection, top-level errors
- `internal/store/` — `Entry` type, JSON `Save`/`Load`
- `Makefile` — build/test/lint/fmt targets
- `.golangci.yml` — lint config

## Commands

```sh
make build   # go build -o bin/notesync .
make test    # go test ./...
make lint    # go vet ./... && golangci-lint run
make fmt     # gofmt -w .
```

Requires Go 1.22+ and `golangci-lint` for `make lint`.

## Conventions

- No external dependencies; stdlib only. Do not add modules to `go.mod` without strong justification.
- Exported identifiers get doc comments (Go convention).
- Keep the public surface minimal — only what the CLI needs.
- Test changes that touch `internal/store` with table-driven tests in `internal/store/store_test.go`.
- Error strings use `%v` with lowercase, context prefix (e.g. `fmt.Errorf("walk: %v", err)`).

## Verification

- Run `make test` and `make lint` before considering a change done.
- `make fmt` before committing; the `gofmt` linter is enabled.