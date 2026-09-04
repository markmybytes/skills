# imgtools

Small image conversion and resizing CLI built on [Click](https://click.palletsprojects.com/) and [Pillow](https://python-pillow.org/).

## Requirements

- Python >= 3.11

## Installation

From the repository root:

```sh
pip install -e .
```

This installs the `imgtools` command and its dependencies (`click`, `pillow`).

## Usage

`imgtools` provides a `resize` and a `convert` command. Both accept one or more image paths and **modify the files in place** — the original files are overwritten. Make a copy first if you need to keep the originals.

### Resize images

Resize images to a fixed width and height. Both `--width` and `--height` are required.

```sh
imgtools resize --width 800 --height 600 photo.jpg banner.png
```

### Convert images

Convert images to another format. Format is given with `--format` (defaults to `png`).

```sh
imgtools convert --format jpeg image.png
imgtools convert --format webp *.png
```

### Help

```sh
imgtools --help
imgtools resize --help
imgtools convert --help
```

## Development

Set up a development environment:

```sh
python -m venv .venv
.venv\Scripts\activate   # on Windows
pip install -e . ruff pytest
```

Run the checks:

```sh
ruff check src tests   # lint
pytest                 # tests
```

CI (`.github/workflows/ci.yml`) runs both on Python 3.11 and 3.12.

## Project layout

```
imgtools/
├── src/imgtools/      # package source
│   ├── __init__.py    # package metadata (__version__)
│   └── cli.py         # Click CLI entry point
├── tests/             # pytest test suite
├── .github/workflows/ # CI configuration
└── pyproject.toml     # project metadata and build config
```

## Versioning

Current version: 0.2.1 (see `pyproject.toml` and `src/imgtools/__init__.py`).