# notesync

Indexes a folder of Markdown notes into a JSON index.

## Scope

Notesync is a small Go CLI that walks a directory tree, collects `.md` files,
and writes a JSON index of their paths, sizes, and modification times. It is
for note-based workflows that need a machine-readable index of note files.

## Stack

| Layer            | Technology              |
| ---------------- | ----------------------- |
| Runtime          | Go 1.22                 |
| Dependencies     | None (standard library) |
| Index format     | JSON (`index.json`)     |

## Prerequisites

- Go 1.22 or newer

## Quick Start

```sh
go build -o bin/notesync .
./bin/notesync -dir ./notes -out index.json
```

The `-dir` directory must exist; it defaults to `./notes`.

## Documentation

- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents
- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
- [STYLE.md](STYLE.md) — Coding conventions

## Development

| Command      | Purpose                        |
| ------------ | ------------------------------ |
| `make build` | Build the binary to `bin/notesync` |
| `make test`  | Run the test suite (`go test ./...`) |
| `make lint`  | Run `go vet` and `golangci-lint run` |
| `make fmt`   | Format the code (`gofmt -w .`) |

## Testing and Quality

```sh
make test
make lint
```

Tests live in `internal/store/` and cover the JSON index save/load round-trip
and error handling for missing files. Linting uses the `.golangci.yml` config
(`gofmt`, `govet`, `staticcheck`).

## Important Notes

- Only files ending in `.md` are indexed; subdirectories are walked recursively.
- The index file is written with mode `0644`.