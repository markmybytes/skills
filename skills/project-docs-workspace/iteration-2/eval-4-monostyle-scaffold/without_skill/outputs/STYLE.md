# STYLE.md

Style and conventions for this repository. Fill in the details below as conventions are established.

## Go

- **Formatting:** `gofmt` (enforced by lint). Run `make fmt`.
- **Naming:** <TODO: naming rules — acronyms, receiver names, etc.>
- **Exported API:** <TODO: doc-comment requirements, what gets exported>
- **Errors:** <TODO: wrapping vs. sentinel errors, prefix convention>
- **Imports:** <TODO: grouping/ordering rules beyond stdlib vs. third-party>

## CLI

- **Flags:** <TODO: flag naming, defaults, short-flag policy>
- **Output:** <TODO: stdout vs. stderr usage, message format>
- **Exit codes:** <TODO: expected exit behavior on failure>

## Tests

- **Style:** <TODO: table-driven vs. plain, naming, assertions>
- **Coverage expectations:** <TODO: which packages must have tests>

## Git & PRs

- **Commits:** <TODO: message format, granularity>
- **PRs:** <TODO: size limits, review expectations>

## Tooling

- **Lint:** `golangci-lint` with the linters in `.golangci.yml` (gofmt, govet, staticcheck).
- **Build/test:** `make build`, `make test`.