# AGENTS.md

Guidance for AI agents and humans working in this repository.

## Project overview

`notesync` is a small Go CLI that walks a directory, collects Markdown
files, and writes a JSON index. The binary lives in `main.go`; the index
format and persistence live in `internal/store`.

## Commands

```sh
make build   # compile to bin/notesync
make test    # go test ./...
make lint    # go vet ./... + golangci-lint run
make fmt     # gofmt -w .
```

## Code layout

- `main.go` — CLI entry point: flag parsing, directory walk, calls
  `store.Save`.
- `internal/store/store.go` — `Entry` type plus `Save`/`Load` for the JSON
  index.
- `internal/store/store_test.go` — unit tests for the store package.

## Conventions to respect

- Keep `internal/` boundaries: `main` imports `store`, never the reverse.
- `store` must not know about flags, walking, or the CLI.
- Errors are returned and handled by the caller; `main` may use
  `log.Fatalf`.
- JSON schema changes in `store.Entry` are breaking — update tests and the
  README example together.
- Run `make fmt` and `make lint` before finishing a change.

## Gotchas

- `make lint` requires `golangci-lint` to be installed locally.
- `bin/` is a build artifact; do not commit it.