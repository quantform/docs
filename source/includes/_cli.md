# <strong class="section-header">Command line (CLI)</strong>

The quantform CLI is a command-line interface tool that you use to create, develop and run quantform strategies directly from command shell.

# Commands

Here you can find a collection of build-in commands.

## initialize a project

> Create project files in current empty directory:

<code>qf init</code>

```console
$ qf init
```

Scaffolds a new project files in current directory.

## run live trading

<code>qf live</code>

> Execute live session in watch mode with fixed session id:

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

> Execute paper session in watch mode with fixed session id:

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

> Execute backtest session:

```console
$ qf backtest
```

> Execute backtest session for custom period:

```console
$ qf backtest -f '01-01-2021' -t '01-01-2022'
```

Executes strategy in backtest mode for specified period.

| Arguments      |                          |
| -------------- | ------------------------ |
| `-c, --config` | qf config file           |
| `-w, --watch`  | run in watch mode        |
| `-f, --from`   | date from in unix format |
| `-t, --to`     | date to in unix format   |

## feed storage with historical data

<code>qf feed</code>

> Download and save historical data of BTC-USDT on Binance:

```console
$ qf feed 'binance:btc-usdt'
```

> Download and save historical data for custom period:

```console
$ qf feed 'binance:btc-usdt' -f '01-01-2021' -t '01-01-2022'
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

> run user defined task named 'list-balances':

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

TODO:
