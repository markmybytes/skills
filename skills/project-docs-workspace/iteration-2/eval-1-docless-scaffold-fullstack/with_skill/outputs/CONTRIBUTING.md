# Contributing

See [README.md](README.md) for setup and development commands.

## Before You Start

- Check existing issues and pull requests before opening a new one.
- Discuss substantial feature changes before implementation.
- Branch from the repository's default branch (`main`).
- Changes to routes must regenerate `resources/js/ziggy.d.ts` via `php artisan ziggy:generate --types-only` and commit the result.

## Branches and Commits

Use a descriptive branch name. Keep each branch focused on one concern.

Use [Conventional Commits](https://www.conventionalcommits.org/):

```text
<type>(<scope>): <description>
```

Common types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`, and
`perf`. Use `!` or a `BREAKING CHANGE:` footer for breaking changes.

## Pull Requests

PRs trigger the CI pipeline (`.github/workflows/ci.yml`); all checks must pass.

- Keep one concern per pull request.
- Explain what changed, why, and any important design decisions.
- Add or update tests for behavior changes and bug fixes.
- Update documentation when behavior, commands, or public interfaces change.
- Run project format, lint, type-check, test, and build checks from `README.md` before opening the PR:

  ```bash
  vendor/bin/pint --test
  npm run lint
  npm run format:check
  npm run type-check
  npm run test
  composer run test
  npm run build
  ```

- Address review feedback before merge.

## Issues

- Include versions, environment details, and a minimal reproduction for bugs.
- Use the project's security process for vulnerabilities; do not disclose them in public issues.