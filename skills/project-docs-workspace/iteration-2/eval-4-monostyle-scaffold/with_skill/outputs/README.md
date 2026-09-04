# notesync

Indexes a folder of Markdown notes into a JSON index.

## Scope

Command-line tool that walks a directory, collects `*.md` files, and writes a
JSON index of note paths, sizes, and modification times. For developers and
users who want a lightweight, scriptable index of their notes folder.

## Stack

| Layer    | Technology |
| -------- | ---------- |
| Language | Go 1.22    |

## Prerequisites

- Go 1.22 or newer

## Quick Start

```sh
go build -o bin/notesync .
./bin/notesync -dir ./notes -out index.json
```

Flags:

- `-dir` — notes directory to index (default `./notes`)
- `-out` — output index file (default `index.json`)

Output is a JSON array of `{path, size, modified}` entries, one per Markdown
file found under `-dir`.

## Documentation

- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents
- [STYLE.md](STYLE.md) — Coding conventions

## Development

| Command              | Purpose                              |
| -------------------- | ------------------------------------ |
| `go run .`           | Run the CLI without building         |
| `make build`         | Build `bin/notesync`                 |
| `make test`          | Run tests                            |
| `make lint`          | Vet and lint with golangci-lint      |
| `make fmt`           | Format source with gofmt             |

## Testing and Quality

```sh
gofmt -w .
go vet ./...
golangci-lint run
go test ./...
```

Tests live in `internal/store/`. Lint is configured by `.golangci.yml`
(gofmt, govet, staticcheck).

## Important Notes

- `index.json` and `bin/notesync` are generated outputs; do not hand-edit them.
- No external dependencies; uses only the Go standard library.