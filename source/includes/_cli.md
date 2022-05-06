# <strong class="section-header">Command line (CLI)</strong>

> To install the CLI globally using the `yarn` package manager:

The quantform CLI is a command-line interface tool that you use to create, develop and run quantform strategies directly from command shell.

# Commands

> Use help command to list commands:

```console
$ yarn run qf -h
```

Here you can find a collection of build-in commands.

## run live trading

<code>qf run</code>

> Execute live session in watch mode with fixed session id:

```console
$ qf run ./strat -w -i 202101012210
```

Executes strategy in live trading mode.

| Arguments |                    |
| --------- | ------------------ |
| `name`    | script file to run |

| Options         |                    |
| --------------- | ------------------ |
| `-i, --id <id>` | session identifier |
| `-w`            | run in watch mode  |

## run paper trading

<code>qf paper</code>

> Execute paper session in watch mode with fixed session id:

```console
$ qf dev ./strat -w -i 202101012210
```

Executes strategy in paper e.g. simulation mode.

| Arguments |                    |
| --------- | ------------------ |
| `name`    | script file to run |

| Options         |                    |
| --------------- | ------------------ |
| `-i, --id <id>` | session identifier |
| `-w`            | run in watch mode  |

## run backtest

<code>qf test</code>

> Execute backtest session:

```console
$ qf test ./strat
```

> Execute backtest session for custom period:

```console
$ qf test ./strat -f '01-01-2021' -t '01-01-2022'
```

Executes strategy in backtest mode for specified period.

| Arguments |                    |
| --------- | ------------------ |
| `name`    | script file to run |

| Options       |                          |
| ------------- | ------------------------ |
| `-f, --from`  | date from in unix format |
| `-t, --to`    | date to in unix format   |
| `-w, --watch` | run in watch mode        |

## feed storage with historical data

<code>qf pull</code>

> Download and save historical data of BTC-USDT on Binance:

```console
$ qf pull ./strat 'binance:btc-usdt'
```

> Download and save historical data for custom period:

```console
$ qf pull ./strat 'binance:btc-usdt' -f '01-01-2021' -t '01-01-2022'
```

Fetches instrument historical data to storage.

| Arguments    |                      |
| ------------ | -------------------- |
| `name`       | script file to run   |
| `instrument` | instrument to import |

| Options      |                          |
| ------------ | ------------------------ |
| `-f, --from` | date from in unix format |
| `-t, --to`   | date to in unix format   |
