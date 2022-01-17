---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - typescript

toc_footers:
  - quantform © 2017 - 2022

includes:
  - cli
  - reference
  - adapter
  - storage
  - editor

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

> To authorize, use this code:

```typescript
session
  .instrument(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("maker fee: ", it.commission.makerRate)));
```

> Make sure to replace `meowmeowmeow` with your API key.

The quantform is a framework designed for exchanging assets (e.g. stocks, currencies, cryptocurrencies) between traditional, centralized and decentralized markets in automatic/computer way.
The framework is based od Node.js with minimum dependecies to met requirements.

Please consider, we are still in development process.

In general the quantform is exchange neutral solution with means you can simply trade at the same time a cross multiple exchanges and instruments.
The second difference is that you express your strategy in a devlarative (reactive) way. Here you can read more about reactive programing.

## What this project is not

The general purpose of quantform is to automate your long-term and short-term investments. It's not a high frequency trading solution, instead this project is focused to provide simple and useful tools with acceptable performance aspect.

# Prerequisites

Read how to setup development environment.

## Install Node.js

Please, visit and download oficiall node.js installer from homepage:

{% embed url="https://nodejs.org/en/download/" %}

## Install quantform command line interface:

Install the CLI globally using the `npm` package manager:

```
$ npm i @quantform/cli -G
```

## Scaffolding a project files:

If you have installed quantform [CLI](../reference/cli.md), you can use a `init` command to generate a project files:

```
$ qf init
```
