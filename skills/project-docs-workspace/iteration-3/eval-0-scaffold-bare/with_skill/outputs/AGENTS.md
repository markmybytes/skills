# AGENTS.md

Batch image resize and format conversion CLI built with Click and Pillow.

## Documentation map

| Doc                                | Read when           | Purpose / ownership        |
| ---------------------------------- | ------------------- | -------------------------- |
| [README.md](README.md)             | Starting work       | Human onboarding           |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution work   | Branches, commits, and PRs |
| [STYLE.md](STYLE.md)               | Before code changes | Coding conventions         |

## Overview

- **Entry:** `imgtools.cli:cli` (installed as the `imgtools` console script)
- **Run:** `imgtools resize --width W --height H <paths>` or `imgtools convert --format <fmt> <paths>`
- **Processes:** none — batch CLI, no daemons or services

## Tech Stack

- **Language/runtime:** Python 3.11+ (installed with pip; no lockfile)
- **CLI/library:** Click 8.1+ and Pillow 10.0+ (declared in `pyproject.toml`)
- **Tooling:** ruff (lint, line length 100), pytest (tests)

## Repository Map

- `src/imgtools/` — package: `cli.py` (`resize`/`convert` commands), `__init__.py` (version)
- `tests/` — pytest suite; `test_cli.py` exercises the CLI via Click's `CliRunner`
- `.github/workflows/` — CI; `ci.yml` runs ruff and pytest on Python 3.11 and 3.12

Follow [STYLE.md](STYLE.md) for code placement and architecture rules. Do not
duplicate those rules in this map.

## Commands

Run from repository root. Apply fixes before verification.

| Scope   | Fix                           | Verify                   |
| ------- | ----------------------------- | ------------------------ |
| Lint    | `ruff check --fix src tests`  | `ruff check src tests`   |
| Tests   | `pytest`                      | `pytest`                 |

- Both commands need an environment with the package installed editable: `pip install -e .`
- `resize` requires `--width` and `--height`; both commands overwrite source files in place.

## Testing

- **Suite:** pytest — `tests/test_cli.py`
- **Fixtures/mocks:** none required; uses Click's `CliRunner`
- **Test database:** none
- **CI:** GitHub Actions (`.github/workflows/ci.yml`) on push and pull requests, matrix Python 3.11/3.12: `ruff check src tests`, then `pytest`.

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