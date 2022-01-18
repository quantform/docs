# <strong class="section-header">Reference</strong>

# Session

Session is a root object of quantform system. Provides an access to unified trading components.

| Parameters                                      |                                 |
| ----------------------------------------------- | ------------------------------- |
| `timestamp`<span class="arg-type">number</span> | last update time                |
| `statement`<span class="arg-type">any</span>    | collection of statement records |

## get list of instruments

<code>instruments(): Observable&lt;Instrument[]&gt;</code>

> Print every instrument one by one:

```typescript
session.instruments().pipe(tap((it) => it.forEach(console.log)));
```

A method that pipes a collection of tradeable instruments. This method get updated in case of Adapter initialization.

### Returns

Returns a observable collection of [Instrument](#instrument).

## get instrument

<code>instrument(selector: InstrumentSelector): Observable&lt;Instrument&gt;</code>

A method that pipes a [instrument](#instrument) updates specified by selector. This method get updated when commission property was patched.

> Track the rate of market maker fee for BTC-USDT on Binance:

```typescript
session
  .instrument(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("maker fee: ", it.commission.makerRate)));
```

| Parameters                                                 |                                   |
| ---------------------------------------------------------- | --------------------------------- |
| `selector`<span class="arg-type">InstrumentSelector</span> | selector of instrument to specify |

### Returns

Returns a observable of [Instrument](#instrument).

## get trade

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

| Properties                                      |                           |
| ----------------------------------------------- | ------------------------- |
| `timestamp`<span class="arg-type">number</span> | last update time          |
| `name`<span class="arg-type">string</span>      | name of asset             |
| `exchange`<span class="arg-type">string</span>  | name of exchange          |
| `scale`<span class="arg-type">number</span>     | defines numeric precision |
| `tickSize`<span class="arg-type">number</span>  | minimum price movement    |

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

| Properties                                           |                                                         |
| ---------------------------------------------------- | ------------------------------------------------------- |
| `timestamp`<span class="arg-type">number</span>      | last update time                                        |
| `base`<span class="arg-type">[Asset](#asset)</span>  | base asset which you going to buy or sell               |
| `quote`<span class="arg-type">[Asset](#asset)</span> | quoted asset                                            |
| `cross`<span class="arg-type">[Asset](#asset)</span> | represents collateral asset                             |
| `leverage`<span class="arg-type">number</span>       | current leverage, `undefined` for non-leveraged markets |
| `commision`<span class="arg-type">Commission</span>  | specifies trading fee rules                             |

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

| Properties                                                          |                             |
| ------------------------------------------------------------------- | --------------------------- |
| `timestamp`<span class="arg-type">number</span>                     | time when execution ocurred |
| `instrument`<span class="arg-type">[Instrument](#instrument)</span> | related instrument          |
| `rate`<span class="arg-type">number</span>                          | execution price             |
| `quantity`<span class="arg-type">number</span>                      | execution size              |

# Orderbook

Provides an access to pending buy and sell orders on the specific market.

> Subscribe to orderbook changes:

```typescript
session
  .orderbook(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => `current best buy offer: ${it.bestBidRate}`));
```

| Properties                                                          |                             |
| ------------------------------------------------------------------- | --------------------------- |
| `timestamp`<span class="arg-type">number</span>                     | last update time            |
| `instrument`<span class="arg-type">[Instrument](#instrument)</span> | related instrument          |
| `bestAskRate`<span class="arg-type">number</span>                   | best sell price             |
| `bestAskQuantity`<span class="arg-type">number</span>               | quantity of best sell price |
| `bestBidRate`<span class="arg-type">number</span>                   | best buy rate               |
| `bestBidQuantity`<span class="arg-type">number</span>               | quantity of best buy price  |
| `midRate`<span class="arg-type">number</span>                       | middle price of bid and ask |

<aside class="warning">Right now you can access only L1 orderbook data.</aside>

# Balance

# Order

# Position
