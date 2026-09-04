# AGENTS.md

Small Python CLI for batch image resizing and format conversion.

## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [README.md](README.md)             | Starting work       | Human onboarding and setup |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |

## Overview

- **Entry:** `imgtools.cli:cli` (console script: `imgtools`)
- **Run:** `imgtools --help` after `python -m pip install -e .`
- **Processes:** none — CLI-only batch tool, no services or daemons

## Tech Stack

- **Language/runtime:** Python >= 3.11
- **CLI:** Click >= 8.1
- **Imaging:** Pillow >= 10.0
- **Lint:** Ruff (line-length 100, configured in `pyproject.toml`)
- **Tests:** pytest

## Repository Map

- `src/imgtools/` — package source; `cli.py` defines the Click CLI
- `tests/` — pytest suite; `test_cli.py` exercises the CLI via `CliRunner`
- `.github/workflows/` — CI (`ci.yml`)

## Commands

Run from repository root. Apply fixes before verification.

| Scope | Fix                                  | Verify                         |
| ----- | ------------------------------------ | ------------------------------ |
| Lint  | `ruff check --fix src tests`         | `ruff check src tests`         |
| Tests | —                                    | `pytest`                       |

## Testing

- **Suite:** pytest — `tests/`
- **Fixtures/mocks:** none; CLI tests use `click.testing.CliRunner`
- **Test database:** none
- **CI:** GitHub Actions (`.github/workflows/ci.yml`) on push and pull_request; runs
  `ruff check src tests` and `pytest` on Python 3.11 and 3.12

## Boundaries

### Always

- Read the documentation map and relevant linked docs before making edits.
- Follow explicit user instruction over repository documentation when they conflict.
- Use the exact commands and prerequisites from the docs.
- Run `ruff check src tests` and `pytest` before declaring work complete.
- Keep changes scoped and minimal.
- Preserve single sources of truth; update owning docs when behavior or interfaces change.

### Ask First

- Destructive, security-sensitive, or ambiguous changes. `resize` and `convert`
  overwrite input files in place.

### Never

- Commit secrets or local environment values.
- Hand-edit generated, vendored, or protected files.
- Commit `.venv/`, `__pycache__/`, or `dist/` artifacts.