# Contributing

Thanks for contributing to notesync.

## Development setup

Requirements:

- Go 1.22 or newer
- `golangci-lint` (for `make lint`)

Clone and build:

```sh
git clone <repo-url> notesync
cd notesync
make build
```

## Making changes

1. Create a branch for your change.
2. Make the change, keeping changes focused and small.
3. Add or update tests in `internal/store` for any behavior change.
4. Run checks locally:

   ```sh
   make test
   make lint
   make fmt
   ```

5. Commit with a clear, concise message (see below).
6. Open a pull request describing what changed and why.

## Commit messages

Follow the conventional commits style:

- `feat: ...` — new feature or flag
- `fix: ...` — bug fix
- `refactor: ...` — behavior-preserving change
- `docs: ...` — documentation only
- `test: ...` — test changes
- `chore: ...` — tooling, deps, misc

## Pull request checklist

- [ ] Tests pass (`make test`)
- [ ] Lint passes (`make lint`)
- [ ] Formatting applied (`make fmt`)
- [ ] README updated if CLI flags or JSON schema changed
- [ ] No build artifacts (`bin/`) committed

## Reporting issues

Include: the command you ran, the Go version, expected vs. actual behavior,
and a minimal reproduction if possible.