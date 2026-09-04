# AGENTS.md

Indexes a folder of Markdown notes into a JSON index.

## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [README.md](README.md)             | Starting work       | Human onboarding           |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |
| [STYLE.md](STYLE.md)               | Before code changes | Coding conventions         |

## Overview

- **Entry:** `main.go`
- **Run:** `go run .`
- **Processes:** none; single-shot CLI

## Tech Stack

- **Go:** 1.22, module `github.com/user/notesync`
- **Dependencies:** none — standard library only
- **CLI:** `flag` package, no framework
- **Lint:** `golangci-lint` with `.golangci.yml` (gofmt, govet, staticcheck)

## Repository Map

- `main.go` — CLI entry: `-dir`/`-out` flags, walks the notes directory, collects entries
- `internal/store/` — `Entry` type plus JSON `Save`/`Load`
- `internal/store/store_test.go` — package tests
- `Makefile` — build, test, lint, and fmt targets
- `.golangci.yml` — linter configuration

Follow [STYLE.md](STYLE.md) for code placement and architecture rules. Do not
duplicate those rules in this map.

## Commands

Run from repository root. Apply fixes before verification.

| Scope   | Fix            | Verify             |
| ------- | -------------- | ------------------ |
| format  | `make fmt`     | `gofmt -l .`       |
| lint    | `make lint`    | `golangci-lint run`|
| test    | `make test`    | `go test ./...`    |
| build   | `make build`   | `go build ./...`   |

## Testing

- **Suite:** standard library `testing` — `go test ./...`
- **Location:** `internal/store/store_test.go`
- **Fixtures/mocks:** none; tests use `t.TempDir()`
- **CI:** not configured in this repository

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