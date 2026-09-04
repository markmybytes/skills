<!-- Agent instructions template. Keep operational knowledge here, not style policy. -->

# AGENTS.md

imgtools is a small image conversion and resizing CLI built with Click and Pillow.

## Documentation map

| Doc                                | Read when         | Purpose / ownership            |
| ---------------------------------- | ----------------- | ------------------------------ |
| [README.md](README.md)             | Starting work     | Human onboarding               |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work | Branches, commits, and PRs     |

## Overview

- **Entry:** `imgtools` console script → `imgtools.cli:cli` (Click group)
- **Run:** `imgtools` — subcommands `resize` and `convert`

## Tech Stack

- **Language:** Python >= 3.11, installed with pip
- **CLI:** Click >= 8.1
- **Imaging:** Pillow >= 10.0
- **Lint:** ruff, `line-length = 100` per `pyproject.toml`

## Repository Map

- `src/imgtools/` — package source; `cli.py` defines the Click group and commands
- `tests/` — pytest suite (`test_cli.py`, uses `click.testing.CliRunner`)
- `.github/workflows/` — CI workflow

## Commands

Run from repository root. Apply fixes before verification.

| Scope | Fix                          | Verify                      |
| ----- | ---------------------------- | --------------------------- |
| Lint  | `ruff check --fix src tests` | `ruff check src tests`      |
| Test  | `pytest`                     | `pytest`                    |

- Install the package first: `pip install -e .` makes the `imgtools` command available.
- The CLI edits image files in place; run it only on files you can afford to overwrite.

## Testing

- **Suite:** pytest — `tests/test_cli.py`
- **Fixtures/mocks:** none — tests use `click.testing.CliRunner`
- **Test database:** none
- **CI:** `.github/workflows/ci.yml` — runs on push and pull_request, Python 3.11 and 3.12 matrix; installs `pip install -e . ruff pytest`, then runs `ruff check src tests` and `pytest`

## Boundaries

### Always

- Read the documentation map and relevant linked docs before making edits.
- Follow explicit user instruction over repository documentation when they conflict.
- Use the exact commands and prerequisites from the docs.
- Run relevant verification or tests before declaring work complete.
- Keep changes scoped and minimal.
- Preserve single sources of truth; update owning docs when behavior or interfaces change.

### Ask First

- Destructive, security-sensitive, or ambiguous changes — the CLI overwrites image files in place.

### Never

- Commit secrets or local environment values.
- Hand-edit generated, vendored, or protected files.