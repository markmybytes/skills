# csvtool

Filter and slice CSV files from the command line.

## Install

```sh
npm install -g csvtool
```

Requires Node.js 18+.

## Usage

### filter

Keep only rows where a column equals a value:

```sh
csvtool filter --column status --eq active data.csv
```

### head

Print the first N data rows (default: 10):

```sh
csvtool head data.csv
```

Change the row count with `-n` / `--rows`:

```sh
csvtool head -n 25 data.csv
```

## Commands

| Command | Description |
| ------- | ----------- |
| `filter` | Keep only rows where a column equals a value |
| `head` | Print the first N data rows |

### filter options

| Option | Description |
| ------ | ----------- |
| `--column <name>` | Column to match (required) |
| `--eq <value>` | Value to match (required) |

### head options

| Option | Description |
| ------ | ----------- |
| `-n, --rows <count>` | Row count (default: `10`) |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).