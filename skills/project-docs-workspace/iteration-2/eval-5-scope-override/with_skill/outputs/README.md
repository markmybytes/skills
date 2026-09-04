# csvtool

Filter and slice CSV files from the command line.

## Install

```sh
npm install -g csvtool
```

Requires Node.js 18+.

## Usage

Keep rows where a column equals a value:

```sh
csvtool filter --column status --eq active data.csv
```

Print the first 10 data rows:

```sh
csvtool head data.csv
```

Use `--rows` to print a different count:

```sh
csvtool head --rows 5 data.csv
```

## Options

| Command | Option | Description |
| ------- | ------ | ----------- |
| `filter` | `--column <name>` | Column to match (required) |
| `filter` | `--eq <value>` | Value to match (required) |
| `head` | `-n, --rows <count>` | Number of data rows to print (default: 10) |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).