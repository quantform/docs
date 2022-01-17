# <strong class="section-header">Reference</strong>

# Session

Session is a root object of quantform system. Provides an access to unified trading components.

### `instruments()`

Subscribes for collection of tradable [instruments](instrument.md).

```typescript
session.instruments().pipe(tap((it) => console.log));
```

### `instrument(selector)`

Subscribes for [instrument](instrument.md) specified by provided selector.

```typescript
session
  .instrument(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("maker fee: ", it.commission.makerRate)));
```

### `trade(selector)`

Subscribes for executed [trades](trade.md) on exchange.

```typescript
session
  .trade(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("last executed rate: ", it.rate)));
```

### `orderbook(selector)`

Subscribes for specified [orderbook](orderbook.md).

```typescript
session
  .orderbook(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("spread: ", it.bestAskRate - it.bestBidRate)));
```

### `position(selector)`

Subscribes for specified [position](position.md) in derivative market.

```typescript
session
  .position(instrumentOf("binance:btc-usdt"))
  .pipe(
    tap((it) => console.log("unrealized profit: ", it.estimatedUnrealizedPnL))
  );
```

### `balance(selector)`

Subscribes for specified [balance](balance.md) changes.

```typescript
session
  .balance(assetOf("binance:usdt"))
  .pipe(tap((it) => console.log("current available balance: ", it.free)));
```

### `history(selector, timeframe, length)`

Pipes a historical sequence of candles in specified timeframe.

```typescript
session
  .history(instrumentOf("binance:btc-usdt"), Timeframe.M15, 21)
  .pipe(tap((it) => console.log(it.timestamp, it.close)));
```

### `open(...orders)`

Opens a sequence of new [orders](order.md).

```typescript
session.open(Order.buyMarket(instrumentOf("binance:btc-usdt"), 0.1));
```

### `cancel(order)`

Cancels peending [order](order.md).

```typescript
session.cancel(order);
```

### `order(selector)`

Subscribes for [order](order.md) changes.

```typescript
session
  .order(instrumentOf("binane:btc-usdt"))
  .pipe(tap((it) => console.log("order updated: ", it.state)));
```

## Paper trading

## Backtesting

## Live trading

# Asset

Represents a security that you can trade or hold in your wallet. For example, you can combine two trading assets to create a trading instrument.

> Create an asset selector based on unified string notation:

```typescript
// returns a selector related to Binance spot market and USDT asset.
const selector = assetOf("binance:usdt");
```

> Subscribe to asset updates and print number of decimals:

```typescript
session
  .asset(assetOf("binance:usdt"))
  .pipe(tap((it) => console.log(`number of decimals: `, it.scale)));
```

> Round an order quantity to exchange decimal places:

```typescript
const order = Order.buyMarket(instrument, instrument.base.floor(0.123456789));
```

| Member      | Description                 |
| ----------- | --------------------------- |
| `timestamp` | _last update time_          |
| `name`      | _name of asset_             |
| `exchange`  | _name of exchange_          |
| `scale`     | _defines numeric precision_ |
| `tickSize`  | _minimum price movement_    |

| Method          | Description                                   |
| --------------- | --------------------------------------------- |
| `toString()`    | _returns unified asset format_                |
| `fixed(number)` | _trims a number to the asset precision_       |
| `floor(number)` | _rounds down a number to the asset precision_ |
| `ceil(number)`  | _rounds up a number to the asset precision_   |

# Instrument

Represents a trading market which is made up by two trading assets (base and
quoted).

> Create a instrument selector based on unified string notation:

```typescript
const selector = instrumentOf("binance:btc-usdt");
```

> Subscribe to BTC-USDT instrument on Binance to print current maker fees:

```typescript
session
  .instrument(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("maker fee: ", it.commission.makerRate)));
```

> Print all tradeable instruments with are quoted to USDT:

```typescript
session.instruments().pipe(filter((it) => it.quote.name == "usdt"));
```

| Member      | Description                                                            |
| ----------- | ---------------------------------------------------------------------- |
| `timestamp` | _last update time_                                                     |
| `base`      | _base_ [asset.md](asset.md "mention") _which you going to buy or sell_ |
| `quote`     | _quoted_ [asset.md](asset.md "mention")                                |
| `cross`     | _represents collateral_ [asset.md](asset.md "mention")                 |
| `leverage`  | _current leverage, `null` for non-leveraged markets_                   |
| `commision` | _specifies trading fee rules_                                          |

| Method       | Description                |
| ------------ | -------------------------- |
| `toString()` | _returns unified selector_ |

# Trade

Simple trade or ticker executed on the market, it's a match of buyer and
seller of the same asset.

> Subscribe to instrument ticker:

```typescript
session
  .trade(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log(`Last price: ${it.rate}`)));
```

> Build a five minute candle based on market trades:

```typescript
session.trade(instrumentOf('binance:btc-usdt'))
  .pipe(
    candle(Timeframe.M5, it => it.rate),
    tap(it => console.log($`{new Date(it.timestamp)} close price is: ${it.close}`)
  );
```

> Calculate current ratio of two instruments (pair) for every occurred trade:

```typescript
combineLatest([
  session.trade(instrumentOf("binance:btc-usdt")),
  session.trade(instrumentOf("binance:eth-usdt")),
]).pipe(
  map(([btc, eth]) => btc.rate / eth.rate),
  tap((it) => `current rate is: ${it}`)
);
```

| Member      | Description                   |
| ----------- | ----------------------------- |
| `timestamp` | _time when execution occured_ |
| `rate`      | _execution price_             |
| `quantity`  | _execution size_              |

# Orderbook

Provides an access to pending buy and sell orders on the specific market.

> Subscribe to orderbook changes:

```typescript
session
  .orderbook(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => `current best buy offer: ${it.bestBidRate}`));
```

| Member            | Description                                        |
| ----------------- | -------------------------------------------------- |
| `timestamp`       | _last update time_                                 |
| `instrument`      | _related_ [instrument.md](instrument.md "mention") |
| `bestAskRate`     | best sell price                                    |
| `bestAskQuantity` | _best sell quantity_                               |
| `bestBidRate`     | _best buy rate_                                    |
| `bestBidQuantity` | _best buy quantity_                                |
| `midRate`         | _middle price of bid and ask_                      |

<aside class="warning">Right now you can access only L1 orderbook data.</aside>

# Balance

# Order

# Position
