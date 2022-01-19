---
title: quantform - documentation

toc_footers:
  - quantform © 2017 - 2022

includes:
  - cookbook
  - cli
  - reference
  - adapter
  - storage
  - editor

code_clipboard: true

meta:
  - name: description
    content: Documentation for quantform
---

# Introduction

The quantform is a framework designed to automate trading on traditional, crypto centralized/decentralized markets. The main goal of this project is to allow express your strategy in declarative/reactive way and provide a solution to trade at the same time across multiple exchanges and instruments.

The framework is bases on Node.js with minimum dependencies to meet requirements. Before start you should know TypeScript and should be familiar with RxJS solution.

Please notice, this project is still in development process.

## What this project is not

The general purpose of quantform is to automate your long-term and short-term investments. It's not a high frequency trading solution, instead this project is focused to provide simple and useful tools with acceptable performance aspect.

## List of features

Here is a list of general features:

- Ability to execute strategies in paper mode, backtest mode and live mode.
- Provide an access to user account and market data via streams.
- Manage local store of orderbook, trades, balances and orders.
- Dedicated command line tools with ability to execute user defined tasks.
- Storage obligated to persist strategy state between multiple sessions.
- Editor app designed for rendering measurements to debug and analyze strategy execution on the fly.
- Standard library of basic technical analysis indicators.
