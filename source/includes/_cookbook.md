# <strong class="section-header">Cookbook</strong>

Here you can find a collection of snippets how to work with quantform.

# Setup a development environment

This section describes how to setup development a environment. Before start please, visit and download Node.js installer from official node [homepage](https://nodejs.org/en/download/).

Use terminal to install quantform CLI globally:

<code>$ yarn global add @quantform/cli</code>

Create new folder and initialize project files:

<code>qf init</code>

Right now, you should have a working project to start research and developer new strategies.

# Swing trading

This chapter describes a swing trading strategy based on bollinger bands. In simple words we are going to use bands to determine entry and exit points for a trade. This strategy is optimized to spot market, so we are able to trade only for long direction. Please notice, it's a simple example of bands trading strategy. You should not trade it for a real money, because you will lost your money for sure.

## determine market direction

```typescript
const direction$ = session.trade(instrumentOf("binance:btc-usdt")).pipe(
  candle(Timeframe.M15, (it) => it.rate), // build 15 min candles
  sma(length, (it) => it.close), // calculate SMA for specified length
  map(([, sma]) => sma)
);
```

Let's create a new RxJS pipe operator, with will be supposed to determine market direction. We are going to define a function that for a number of Candles

## enter market position

## exit market position
