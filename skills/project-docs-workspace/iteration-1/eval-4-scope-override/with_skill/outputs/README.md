# csvtool

Filter and slice CSV files from the command line.

## Install

```sh
npm install -g csvtool
```

Requires Node.js 18+.

## Usage

Filter rows where a column equals a value:

```sh
csvtool filter --column status --eq active data.csv
```

Print the first 10 data rows:

```sh
csvtool head data.csv
```

## Commands

| Command | Description |
| ------- | ----------- |
| `filter` | Keep rows where a column equals a value; requires `--column <name>` and `--eq <value>` |
| `head` | Print the first N data rows; `-n, --rows <count>` defaults to 10 |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).