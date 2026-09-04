# Contributing

Thanks for contributing to `notesync`.

## Prerequisites

- Go 1.22+
- `golangci-lint` (only needed for `make lint`)

## Getting started

1. Fork the repository and clone it.
2. Run `make test` to confirm the baseline passes.
3. Create a branch for your change.

## Making changes

- Follow the conventions in [STYLE.md](STYLE.md) and [AGENTS.md](AGENTS.md).
- Keep changes small and focused. Prefer stdlib over new dependencies.
- Add or update tests in the affected package (`internal/store` has existing coverage).
- Run `make fmt`, `make test`, and `make lint` before committing.

## Submitting

- Open a pull request with a concise description of the change and why.
- Reference any related issue.
- Keep the commit message in the imperative mood, e.g. `Add --verbose flag`.

## Reporting bugs

Open an issue with:

- Command you ran
- Expected vs. actual output
- Go version and OS