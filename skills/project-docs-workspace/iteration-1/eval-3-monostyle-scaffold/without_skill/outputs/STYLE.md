# Style Guide

This file is a skeleton for notesync's coding conventions. Fill in each
section as the project settles on concrete rules.

## Formatting

- Enforced by `gofmt` (`make fmt`) and the `gofmt` linter.
- _TODO: any project-specific formatting exceptions or rules beyond gofmt._

## Naming

- _TODO: naming conventions for packages, types, functions, and variables._

## Error handling

- _TODO: wrap vs. propagate errors, when to use `log.Fatalf` vs. returning
  errors._

## Packages and layout

- `internal/` contains non-public packages; `main` is the only public entry
  point.
- _TODO: rules for package boundaries, dependencies between packages, and
  what belongs in `main` vs. `internal/`._

## Tests

- _TODO: test naming, table-driven test conventions, what must be covered._

## Linting

- Enabled linters live in `.golangci.yml`: `gofmt`, `govet`, `staticcheck`.
- _TODO: any lint rules the team intentionally disables or ignores, and why._

## Commits

- See `CONTRIBUTING.md` for conventional-commit message rules.
- _TODO: any additional commit or review conventions._

## CLI conventions

- Flags are defined in `main.go` with the standard library `flag` package.
- _TODO: flag naming, defaults, and output format conventions._