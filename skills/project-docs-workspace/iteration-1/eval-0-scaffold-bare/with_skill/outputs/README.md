# imgtools

Small image conversion and resizing CLI.

## Scope

imgtools is a command-line tool for batch image manipulation: resizing images to a fixed
WIDTH x HEIGHT and converting image files between formats. It operates on one or more files
in place. Aimed at developers and script users who need image processing from the terminal.

## Stack

| Layer     | Technology             |
| --------- | ---------------------- |
| Language  | Python >= 3.11         |
| CLI       | Click >= 8.1           |
| Imaging   | Pillow >= 10.0         |
| Lint      | Ruff (line-length 100) |

## Prerequisites

- Python 3.11 or newer

## Quick Start

```sh
python -m pip install -e .
imgtools --help
```

## Documentation

- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents

## Development

| Command                                                       | Purpose                             |
| ------------------------------------------------------------- | ----------------------------------- |
| `python -m pip install -e . ruff pytest`                      | Install the package and dev tools   |
| `imgtools resize --width 800 --height 600 photo.jpg`          | Resize images to 800 x 600          |
| `imgtools convert --format webp photo.png`                    | Convert images to another format    |
| `imgtools convert --format webp --help`                       | Show a command's usage              |

Both commands accept multiple files: `imgtools resize --width 100 --height 100 a.jpg b.jpg`.

## Testing and Quality

```sh
ruff check src tests
pytest
```

- Tests live in `tests/test_cli.py` and exercise the CLI through `click.testing.CliRunner`.
- No test fixtures or services are required.
- CI (`.github/workflows/ci.yml`) runs the same lint and test commands on Python 3.11
  and 3.12 for every push and pull request.

## Important Notes

- `resize` and `convert` overwrite the input files in place; keep backups of originals.
- `convert` keeps the original filename and forces the requested format on save.