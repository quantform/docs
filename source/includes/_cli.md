# <strong class="section-header">Command line (CLI)</strong>

The quantform CLI is a command-line interface tool that you use to create, develop and run quantform strategies directly from command shell.

# Commands

## `qf init`

Scaffolds a new project files in current directory.

```
$ qf init
```

## `qf live`

Executes strategy in live trading mode.

| Argument       | Description          |
| -------------- | -------------------- |
| `-c, --config` | _qf config file_     |
| `-w, --watch`  | _run in watch mode_  |
| `-i, --id`     | _session identifier_ |

```
$ qf live -w -i 202101012210
```

## `qf paper`

Executes strategy in paper e.g. simulation mode.

| Argument       | Description          |
| -------------- | -------------------- |
| `-c, --config` | _qf config file_     |
| `-w, --watch`  | _run in watch mode_  |
| `-i, --id`     | _session identifier_ |

```
qf paper -w -i 202101012210
```

## `qf backtest`

Executes strategy in backtesting mode for specified period.

| Argument       | Description                |
| -------------- | -------------------------- |
| `-c, --config` | _qf config file_           |
| `-w, --watch`  | _run in watch mode_        |
| `-f, --from`   | _date from in unix format_ |
| `-t, --to`     | _date to in unix format_   |

```
qf backtest
```

## `qf feed`

Fetches instrument historical data to storage.

|                |                                          |
| -------------- | ---------------------------------------- |
| `<instrument>` | _instrument to import in unified format_ |
| `-c, --config` | _qf config file_                         |
| `-f, --from`   | _date from in unix format_               |
| `-t, --to`     | _date to in unix format_                 |

```
qf feed 'binance:btc-usdt'
```

## `qf task`

Executes user defined task.

|                |                            |
| -------------- | -------------------------- |
| `<taskName>`   | _name of the task to run_  |
| `-c, --config` | _qf config file_           |
| `-w, --watch`  | _run in watch mode_        |
| `-p, --print`  | _prints result to console_ |

```
qf task list-balances
```

# User defined tasks
