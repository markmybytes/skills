<!-- Lean contribution guide. Add project-specific policy only when user-provided or confirmed.
Branch and commit conventions below are common-practice defaults behind the approval gate —
confirm or replace them with project policy before they become normative. -->

# Contributing

See [README.md](README.md) for setup and development commands.

## Before You Start

- Check existing issues and pull requests before opening a new one.
- Discuss substantial feature changes before implementation.
- Branch from the repository's default branch.

## Branches and Commits

Use a descriptive branch name. Keep each branch focused on one concern.

Use [Conventional Commits](https://www.conventionalcommits.org/):

```text
<type>(<scope>): <description>
```

Common types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`, and
`perf`. Use `!` or a `BREAKING CHANGE:` footer for breaking changes.

## Pull Requests

- Keep one concern per pull request.
- Explain what changed, why, and any important design decisions.
- Add or update tests for behavior changes and bug fixes.
- Update documentation when behavior, commands, or public interfaces change.
- Run project format, lint, type-check, test, and build checks from `README.md`.
- Address review feedback before merge.

## Issues

- Include versions, environment details, and a minimal reproduction for bugs.
- Use the project's security process for vulnerabilities; do not disclose them
  in public issues.