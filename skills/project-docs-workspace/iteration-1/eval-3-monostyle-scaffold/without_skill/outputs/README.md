# notesync

Indexes a folder of Markdown notes into a JSON index.

## Usage

```sh
notesync -dir ./notes -out index.json
```

Flags:

| Flag   | Default      | Description                |
| ------ | ------------ | -------------------------- |
| `-dir` | `./notes`    | notes directory to index   |
| `-out` | `index.json` | output index file          |

Each note is recorded with its path, size, and modification time:

```json
[
  {
    "path": "notes/a.md",
    "size": 12,
    "modified": 1700000000
  }
]
```

## Build

```sh
go build -o bin/notesync .
```

## Development

See [CONTRIBUTING.md](CONTRIBUTING.md) for the contribution workflow and
[STYLE.md](STYLE.md) for coding conventions.

Common commands:

```sh
make test   # run all tests
make lint   # go vet + golangci-lint
make fmt    # gofmt -w .
```

## License

TODO: add license.