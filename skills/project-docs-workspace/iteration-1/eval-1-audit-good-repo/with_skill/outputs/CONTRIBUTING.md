# Contributing

## Setup

See [README.md](README.md#quick-start) for clone-to-running and prerequisites.

> **Test database:** see [AGENTS.md](AGENTS.md#testing) — PHPUnit uses `h730_toolbox_test` (see `phpunit.xml`), and that database must pre-exist in MySQL; `RefreshDatabase` runs migrations against it but does not create it.

## Development Workflow

**Before making any code changes, read [STYLE.md](STYLE.md)** — it defines controller placement, API contract, Vue patterns, and naming conventions that all code must follow.

### Branches, Commits & PR Titles

Jira is the single traceability anchor. Put its key in the **branch name** and **PR title**; commits do not need it.

- **Branches** are named after the Jira issue they implement: the uppercase issue key followed by a short, `-` separated description. No `feat/` / `fix/` / `chore/` prefixes.

    ```
    IT-2-update-docs
    IT-13-fix-webvitals-timeout
    IT-27-add-campaign-clone
    ```

- **Commit messages** follow [Conventional Commits](https://www.conventionalcommits.org/) and should remain concise.

    ```
    feat: add campaign clone endpoint
    fix: handle empty PageSpeed response
    chore(deps): upgrade Vue to 3.5
    docs: update contributing guidelines
    ```

- **PR titles** must follow Conventional Commits and end with the Jira key.

    ```
    fix: correct Jira deployment traceability (IT-19)
    ```

### Merging

Use **Squash and Merge** only. Keep the PR title, including its Jira key, as the squash commit message for deployment linking.

### Code Style

1. Follow conventions defined in [STYLE.md](STYLE.md).
2. Apply auto-formatter and lint.
3. Fix any issues the linter reports before moving on.

## Testing

- **PHP (PHPUnit):** tests live in `tests/Unit/` and `tests/Feature/`. Requires a running MySQL; see the test database caveat under [Setup](#setup).
- **JS (Vitest, jsdom):** the `route()` helper is globally mocked, no backend required. Tests live in `resources/js/__tests__/`.
- For local app setup, use `php artisan migrate:fresh --seed` on your primary local database so the UI boots with demo data. Keep tests explicit; `DatabaseSeeder` is for `local` / `preview`, not `testing`.

## Pull Requests

### Before Submitting

- [ ] Branch is based on `main`, named `IT-xxx-description`, and addresses a single concern
- [ ] Auto-fix tools and check commands have been run (see [README.md](README.md#quality--testing))
- [ ] New features include tests; bug fixes include regression tests
- [ ] Documentation is updated if behaviour or interfaces change (README, AGENTS, CONTRIBUTING, STYLE, DEPLOYMENT)
- [ ] PR title follows Conventional Commits and ends with the Jira key

### When Opening

- Title your PR with a concise Conventional Commit title ending with the Jira key
- Mark as **Draft** while work is in progress
- Ensure CI checks (test + build workflows) are green
- Use **Squash and Merge** (see [Merging](#merging))