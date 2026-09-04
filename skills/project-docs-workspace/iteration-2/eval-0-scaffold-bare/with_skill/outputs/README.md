<!-- Developer-facing README template. Keep it practical; omit user-marketing sections. -->

# imgtools

Small image conversion and resizing CLI built with Click and Pillow.

## Scope

Batch-resize and convert image files from the command line. The commands operate
in place on the files you pass, overwriting the originals.

## Stack

| Layer    | Technology    |
| -------- | ------------- |
| Language | Python >= 3.11 |
| CLI      | Click >= 8.1  |
| Imaging  | Pillow >= 10.0 |

## Prerequisites

- Python 3.11 or newer
- pip

## Quick Start

```sh
pip install -e .
imgtools resize photo.png --width 800 --height 600
imgtools convert logo.png --format jpeg
```

## Documentation

- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents

## Development

| Command                | Purpose                                            |
| ---------------------- | -------------------------------------------------- |
| `pip install -e .`     | Install the package and `imgtools` CLI (editable)  |
| `pytest`               | Run the test suite                                 |
| `ruff check src tests` | Lint source and tests                              |

## Testing and Quality

```sh
pip install -e . ruff pytest
ruff check src tests
pytest
```

CI (`.github/workflows/ci.yml`) runs lint and tests on push and pull request
for Python 3.11 and 3.12.

## Important Notes

- `resize` and `convert` overwrite the original files in place — keep a backup
  before running them.
- Ruff is configured with `line-length = 100` in `pyproject.toml`.