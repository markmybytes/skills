# imgtools

Small command-line tool for batch image conversion and resizing.

Built with [Click](https://click.palletsprojects.com/) and [Pillow](https://python-pillow.org/).
Requires Python 3.11+.

## Installation

From a checkout of this repository:

```console
pip install -e .
```

Or straight from the source tree with `uv`:

```console
uv pip install -e .
```

This installs the `imgtools` command on your `PATH`.

## Usage

```
imgtools [OPTIONS] COMMAND [ARGS]...
```

Two commands are available: `resize` and `convert`. Both accept one or more
image paths and modify the files **in place**.

### resize

Resize images to a fixed width and height:

```console
imgtools resize --width 800 --height 600 photo1.png photo2.png
```

Options:

- `--width` (required) — target width in pixels.
- `--height` (required) — target height in pixels.

### convert

Convert images to another format:

```console
imgtools convert --format webp logo.png icon.png
```

Options:

- `--format` (default `png`) — target format. Any format supported by Pillow
  can be used; the value is uppercased internally (e.g. `jpg`, `webp`).

## Development

Set up a virtual environment and install with dev dependencies:

```console
python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # macOS / Linux
pip install -e . ruff pytest
```

Run the checks:

```console
ruff check src tests
pytest
```

Continuous integration (`.github/workflows/ci.yml`) runs `ruff` and `pytest`
on Python 3.11 and 3.12 for every push and pull request.

## Project layout

```
src/imgtools/cli.py        Click command-line entry point
src/imgtools/__init__.py   Package metadata (__version__)
tests/test_cli.py          CLI smoke tests
```

## License

No license declared.