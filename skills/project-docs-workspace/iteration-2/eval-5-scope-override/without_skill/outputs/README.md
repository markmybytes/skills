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

Print the first 10 rows:

```sh
csvtool head data.csv
```

## Commands

| Command | Description |
| ------- | ----------- |
| `filter` | Keep only rows where a column equals a value |
| `head` | Print the first N data rows |

### `filter`

| Option | Description |
| ------ | ----------- |
| `--column <name>` | Column to match (required) |
| `--eq <value>` | Value to match (required) |

### `head`

| Option | Description |
| ------ | ----------- |
| `-n, --rows <count>` | Row count (default: 10) |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).