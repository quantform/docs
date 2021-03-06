# Introduction

The quantform is a framework designed to automate trading on traditional, crypto centralized/decentralized markets. The main goal of this project is to allow express your strategy in declarative/reactive way and provide a solution to trade at the same time across multiple exchanges and instruments.

The framework is bases on Node.js with minimum dependencies to meet requirements. Before start you should know TypeScript and should be familiar with RxJS solution.

Please notice, this project is still in development process.

## List of features

Here is a list of general features:

- Ability to execute strategies in paper mode, backtest mode and live mode.
- Provide an access to user account and market data via streams.
- Manage local store of orderbook, trades, balances and orders.
- Dedicated command line tools with ability to execute user defined tasks.
- Storage obligated to persist strategy state between multiple sessions.
- Editor app designed for rendering measurements to debug and analyze strategy execution on the fly.
- Standard library of basic technical analysis indicators.

## What this project is not

The general purpose of quantform is to automate your long-term and short-term investments. It's not a high frequency trading solution, instead this project is focused to provide simple and useful tools with acceptable performance aspect.

# Packages

All packages are distributed on npmjs.com:

- <a href="https://www.npmjs.com/package/create-quantform-app"><img src="https://img.shields.io/npm/v/create-quantform-app.svg?logo=npm&logoColor=fff&label=create-quantform-app&color=03D1EB&style=flat-square" alt="create-quantform-app on npm" /></a>
- <a href="https://www.npmjs.com/package/@quantform/core"><img src="https://img.shields.io/npm/v/@quantform/core.svg?logo=npm&logoColor=fff&label=@quantform/core&color=03D1EB&style=flat-square" alt="quantform/core on npm" /></a>
- <a href="https://www.npmjs.com/package/@quantform/sqlite"><img src="https://img.shields.io/npm/v/@quantform/sqlite.svg?logo=npm&logoColor=fff&label=@quantform/sqlite&color=03D1EB&style=flat-square" alt="quantform/sqlite on npm" /></a>
- <a href="https://www.npmjs.com/package/@quantform/binance"><img src="https://img.shields.io/npm/v/@quantform/binance.svg?logo=npm&logoColor=fff&label=@quantform/binance&color=03D1EB&style=flat-square" alt="quantform/binance on npm" /></a>
- <a href="https://www.npmjs.com/package/@quantform/binance-future"><img src="https://img.shields.io/npm/v/@quantform/binance-future.svg?logo=npm&logoColor=fff&label=@quantform/binance-future&color=03D1EB&style=flat-square" alt="quantform/binance-future on npm" /></a>
- <a href="https://www.npmjs.com/package/@quantform/studio"><img src="https://img.shields.io/npm/v/@quantform/studio.svg?logo=npm&logoColor=fff&label=@quantform/studio&color=03D1EB&style=flat-square" alt="quantform/studio on npm" /></a>
