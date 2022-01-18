# <strong class="section-header">Command line (CLI)</strong>

The quantform CLI is a command-line interface tool that you use to create, develop and run quantform strategies directly from command shell.

# Commands

Here you can find a collection of build-in commands.

## initialize project

<code>qf init</code>

```console
$ qf init
```

Scaffolds a new project files in current directory.

## run live trading

<code>qf live</code>

```console
$ qf live -w -i 202101012210
```

Executes strategy in live trading mode.

| Arguments      |                    |
| -------------- | ------------------ |
| `-c, --config` | qf config file     |
| `-w, --watch`  | run in watch mode  |
| `-i, --id`     | session identifier |

## run paper trading

<code>qf paper</code>

```console
$ qf paper -w -i 202101012210
```

Executes strategy in paper e.g. simulation mode.

| Arguments      |                    |
| -------------- | ------------------ |
| `-c, --config` | qf config file     |
| `-w, --watch`  | run in watch mode  |
| `-i, --id`     | session identifier |

## run backtest

<code>qf backtest</code>

```console
$ qf backtest
```

Executes strategy in backtest mode for specified period.

| Arguments      |                          |
| -------------- | ------------------------ |
| `-c, --config` | qf config file           |
| `-w, --watch`  | run in watch mode        |
| `-f, --from`   | date from in unix format |
| `-t, --to`     | date to in unix format   |

## feed storage with data

<code>qf feed</code>

```console
$ qf feed 'binance:btc-usdt'
```

Fetches instrument historical data to storage.

| Arguments      |                                        |
| -------------- | -------------------------------------- |
| `<instrument>` | instrument to import in unified format |
| `-c, --config` | qf config file                         |
| `-f, --from`   | date from in unix format               |
| `-t, --to`     | date to in unix format                 |

## run user defined task

<code>qf task</code>

```console
$ qf task list-balances
```

Executes user defined task.

| Arguments      |                          |
| -------------- | ------------------------ |
| `<taskName>`   | name of the task to run  |
| `-c, --config` | qf config file           |
| `-w, --watch`  | run in watch mode        |
| `-p, --print`  | prints result to console |

# User defined tasks
