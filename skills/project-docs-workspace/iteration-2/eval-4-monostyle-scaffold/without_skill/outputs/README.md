# notesync

`notesync` indexes a folder of Markdown notes into a JSON index. Each entry records the file path, size, and last-modified time, so external tools can watch or sync the collection.

## Install

```sh
go install github.com/user/notesync@latest
```

Or build locally:

```sh
make build
# binary at bin/notesync
```

## Usage

```sh
notesync [flags]
```

Flags:

| Flag   | Default      | Description                          |
| ------ | ------------ | ------------------------------------ |
| `-dir` | `./notes`    | Notes directory to index             |
| `-out` | `index.json` | Output index file                    |

Example:

```sh
notesync -dir ~/notes -out notes-index.json
# indexed 42 notes -> notes-index.json
```

## Index format

JSON array of entries, one per Markdown file found under `-dir`:

```json
[
  {
    "path": "notes/a.md",
    "size": 12,
    "modified": 1700000000
  }
]
```

Only regular files ending in `.md` are included.

## Development

```sh
make build   # compile
make test    # run tests
make lint    # go vet + golangci-lint
make fmt     # gofmt
```

See [CONTRIBUTING.md](CONTRIBUTING.md) and [STYLE.md](STYLE.md).