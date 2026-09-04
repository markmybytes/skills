# AGENTS.md

Notesync indexes a folder of Markdown notes into a JSON index. This is a small
Go CLI with no external dependencies.

## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [README.md](README.md)             | Starting work       | Human onboarding           |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |
| [STYLE.md](STYLE.md)               | Before code changes | Coding conventions         |

## Overview

- **Entry:** `main.go` (`main()`)
- **Run:** `go run . -dir ./notes -out index.json`, or `make build` then `./bin/notesync`
- **Processes:** none; single-shot CLI

## Tech Stack

- **Language:** Go 1.22 (`go.mod`), standard library only, no external dependencies.
- **Tooling:** Makefile targets; `golangci-lint` configured in `.golangci.yml`
  (linters: `gofmt`, `govet`, `staticcheck`).

## Repository Map

- `main.go` — CLI entry point: flag parsing (`-dir`, `-out`), directory walk, output
- `internal/store/` — `Entry` model and `Save`/`Load` for the JSON index
- `internal/store/store_test.go` — unit tests for the store package

Follow [STYLE.md](STYLE.md) for code placement and architecture rules. Do not
duplicate those rules in this map.

## Commands

Run from repository root. Apply fixes before verification.

| Scope   | Fix         | Verify      |
| ------- | ----------- | ----------- |
| Format  | `make fmt`  | `gofmt -l .` |
| Lint    | —           | `make lint` |
| Test    | —           | `make test` |
| Build   | —           | `make build` |

- `golangci-lint` must be installed separately; `make lint` runs `go vet` first.
- `make build` writes the binary to `bin/notesync`.

## Testing

- **Unit tests:** `go test ./...` — `internal/store/store_test.go`
- **Fixtures/mocks:** none; tests use `t.TempDir()`
- **Test database:** none
- **CI:** no CI configuration present

## Boundaries

### Always

- Read the documentation map and relevant linked docs before making edits.
- Follow explicit user instruction over repository documentation when they conflict.
- Use the exact commands and prerequisites from the docs.
- Run relevant verification or tests before declaring work complete.
- Keep changes scoped and minimal.
- Preserve single sources of truth; update owning docs when behavior or interfaces change.

### Ask First

- Destructive, security-sensitive, or ambiguous changes.

### Never

- Commit secrets or local environment values.
- Hand-edit generated, vendored, or protected files.