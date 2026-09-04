<a id="readme-top"></a>

<div align="center">

  <h3 align="center">imgtools</h3>

  <p align="center">
    Batch image resize and format conversion from the command line.
  </p>
</div>

<!-- ABOUT THE PROJECT -->

## About the Project

`imgtools` is a small command-line tool for batch image processing. It resizes
one or more images to a given width and height, or converts them to another
format, with a single command. It is built with Click and Pillow and runs on
Python 3.11+.

> Note: both commands operate on the source files in place. `resize` and
> `convert` overwrite the original files, and `convert` keeps the original
> filename — only the image data is re-encoded, so the file extension will not
> match the new format. Copy files first if you need to keep the originals.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->

## Usage Examples

### Resize images

Resize one or more images to `WIDTH x HEIGHT`:

```sh
imgtools resize --width 800 --height 600 photo.jpg
imgtools resize --width 1920 --height 1080 photo.jpg banner.png
```

### Convert images to another format

Convert one or more images to a format (default `png`):

```sh
imgtools convert --format webp photo.png
imgtools convert banner.jpg
```

The format name is case-insensitive: `--format JPEG` and `--format jpeg` both
work.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## Getting Started

### Prerequisites

- Python 3.11 or newer
- Pillow and Click (installed automatically with the package)

### Installation / Setup

Install from the repository root:

```sh
pip install -e .
```

This installs the `imgtools` command.

### Development

```sh
pip install -e .
pytest
ruff check src tests
```

### Documentation

- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents
- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
- [STYLE.md](STYLE.md) — Coding conventions

<p align="right">(<a href="#readme-top">back to top</a>)</p>