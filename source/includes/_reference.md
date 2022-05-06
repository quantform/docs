# <strong class="section-header">Reference</strong>

Here you can find a definition of a unified trading interface that you are going to interact with while developing strategies.

# Session

Session is a root object of quantform system. Provides an access to all unified trading components.

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

A method that pipes a [Instrument](#instrument) updates specified by selector. This method get updated when commission property was patched.

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

<code>trade(selector: InstrumentSelector): Observable&lt;Trade&gt;</code>

A method that pipes [Trades](#trade) ocurred on market.

> Print last executed trade on market:

```typescript
session
  .trade(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("last executed rate: ", it.rate)));
```

| Parameters                                                 |                                   |
| ---------------------------------------------------------- | --------------------------------- |
| `selector`<span class="arg-type">InstrumentSelector</span> | selector of instrument to specify |

### Returns

Returns a observable of [Trade](#trade).

## get orderbook

<code>orderbook(selector: InstrumentSelector): Observable&lt;Orderbook&gt;</code>

A method that pipes a snapshot of [Orderbook](#orderbook).

> Calculate and print a current spread:

```typescript
session
  .orderbook(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("spread: ", it.bestAskRate - it.bestBidRate)));
```

| Parameters                                                 |                                   |
| ---------------------------------------------------------- | --------------------------------- |
| `selector`<span class="arg-type">InstrumentSelector</span> | selector of instrument to specify |

### Returns

Returns a observable of [Orderbook](#orderbook).

## get position

<code>position(selector: InstrumentSelector): Observable&lt;Position&gt;</code>

A method that pipes a opened [Position](#position) on derivative market.

> Print unrealized profit for last updated position:

```typescript
session
  .position(instrumentOf("binance:btc-usdt"))
  .pipe(
    tap((it) => console.log("unrealized profit: ", it.estimatedUnrealizedPnL))
  );
```

| Parameters                                                 |                                   |
| ---------------------------------------------------------- | --------------------------------- |
| `selector`<span class="arg-type">InstrumentSelector</span> | selector of instrument to specify |

### Returns

Returns a observable of [Position](#position).

## get balance

<code>balance(selector: AssetSelector): Observable&lt;Balance&gt;</code>

A method that pipes [Balance](#balance) changes.

> Print current free balance:

```typescript
session
  .balance(assetOf("binance:usdt"))
  .pipe(tap((it) => console.log("current available balance: ", it.free)));
```

| Parameters                                            |                              |
| ----------------------------------------------------- | ---------------------------- |
| `selector`<span class="arg-type">AssetSelector</span> | selector of asset to specify |

### Returns

Returns a observable of [Balance](#balance).

## get history

<code>history(selector: InstrumentSelector, timeframe: number, length: number): Observable&lt;Candle&gt;</code>

A method that pipes a historical sequence of candles in specified timeframe.

> Download and print latest 21 candles in 15 min timeframe for BTC-USDT on Binance:

```typescript
session
  .history(instrumentOf("binance:btc-usdt"), Timeframe.M15, 21)
  .pipe(tap((it) => console.log(it.timestamp, it.close)));
```

| Parameters                                                 |                                                      |
| ---------------------------------------------------------- | ---------------------------------------------------- |
| `selector`<span class="arg-type">InstrumentSelector</span> | selector of instrument to specify                    |
| `timeframe`<span class="arg-type">number</span>            | timeframe of candles (must be supported by exchange) |
| `length`<span class="arg-type">number</span>               | number of candles back                               |

### Returns

Returns a observable of Candle.

## get order

<code>order(selector: InstrumentSelector): Observable&lt;Order&gt;</code>

A method that tracks an [Order](#order) changes.

> Print new state of order:

```typescript
session
  .order(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("order updated: ", it.state)));
```

| Parameters                                                 |                                   |
| ---------------------------------------------------------- | --------------------------------- |
| `selector`<span class="arg-type">InstrumentSelector</span> | selector of instrument to specify |

### Returns

Returns a observable of [Order](#order).

## get orders

<code>orders(selector: InstrumentSelector): Observable&lt;Order[]&gt;</code>

A method that tracks an collection of [Orders](#order) filtered by [Order States](#order-state).

> Print number of pending orders:

```typescript
session
  .orders(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => console.log("number of pending orders: ", it.length)));
```

| Parameters                                                 |                                   |
| ---------------------------------------------------------- | --------------------------------- |
| `selector`<span class="arg-type">InstrumentSelector</span> | selector of instrument to specify |

### Returns

Returns am observable collection of [Order](#order).

## open new order

<code>open(order: Order): Observable&lt;Order&gt;</code>

A method that opens a new [Order](#order).

> Opens a new buy order 0.1 of BTC on BTC-USDT at Binance market:

```typescript
session.open(Order.market(instrumentOf("binance:btc-usdt"), 0.1));
```

| Parameters                                          |               |
| --------------------------------------------------- | ------------- |
| order`<span class="arg-type">[Order](#order)</span> | order to open |

### Returns

Returns a promise.

## cancel pending order

<code>cancel(order: Order): Observable&lt;Order&gt;</code>

A method that cancels a pending [Order](#order).

> Opens a pending order:

```typescript
session.cancel(order);
```

| Parameters                                           |                 |
| ---------------------------------------------------- | --------------- |
| `order`<span class="arg-type">[Order](#order)</span> | order to cancel |

### Returns

Returns a promise.

## save measurements

<code>useMeasure&lt;T&gt;({ kind })</code>

A method that can store and retrieve object between multiple same sessions. You can use editor to render and debug stored measurements.

> Query an object from storage:

```typescript
const [order$] = session.useMeasure<Order>({ kind: "last-executed-order" });

order$.pipe(tap((it) => console.log(it)));
```

> Save a new object to storage:

```typescript
const [order$, saveOrder] = session.useMeasure<Order>({
  kind: "last-executed-order",
});

// save filled orders to storage
session.order(instrumentOf("binance:btc-usdt")).pipe(
  filter((it) => it.state == "FILLED"),
  map((it) => saveOrder(it))
);

// print last filled order stored in storage
order$.pipe(tap((it) => console.log(it)));
```

| Parameters                                           |                 |
| ---------------------------------------------------- | --------------- |
| `order`<span class="arg-type">[Order](#order)</span> | order to cancel |

### Returns

Returns a tuple of stored object and a function to save new objects.

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
const order = Order.market(instrument, instrument.base.floor(0.123456789));
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

| Properties                                                         |                                                         |
| ------------------------------------------------------------------ | ------------------------------------------------------- |
| `timestamp`<span class="arg-type">number</span>                    | last update time                                        |
| `base`<span class="arg-type">[Asset](#asset)</span>                | base asset which you going to buy or sell               |
| `quote`<span class="arg-type">[Asset](#asset)</span>               | quoted asset                                            |
| `cross`<span class="arg-type">[Asset](#asset)</span>               | represents collateral asset                             |
| `leverage`<span class="arg-type">number</span>                     | current leverage, `undefined` for non-leveraged markets |
| `commision`<span class="arg-type">[Commission](#commission)</span> | specifies trading fee rules                             |

### Commission

| Properties                                      |                |
| ----------------------------------------------- | -------------- |
| `makerRate`<span class="arg-type">number</span> | maker fee rate |
| `takerRate`<span class="arg-type">number</span> | taker fee rate |

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

Represents account balance.

> Subscribe to balance changes:

```typescript
session
  .balance(assetOf("binance:btc"))
  .pipe(tap((it) => `current free balance of BTC: ${it.free}`));
```

| Properties                                                      |                                                       |
| --------------------------------------------------------------- | ----------------------------------------------------- |
| `timestamp`<span class="arg-type">number</span>                 | last update time                                      |
| `asset`<span class="arg-type">[Asset](#asset)</span>            | related asset                                         |
| `free`<span class="arg-type">number</span>                      | available quantity to trade                           |
| `freezed`<span class="arg-type">number</span>                   | locked quantity in pending orders                     |
| `total`<span class="arg-type">number</span>                     | the sum of free and freezed quantities                |
| `position`<span class="arg-type">[Position[]](#position)</span> | collection of opened positions backed by this balance |

# Order

Represents an market or limit order on the market.

| Properties                                                          |                                                    |
| ------------------------------------------------------------------- | -------------------------------------------------- |
| `id`<span class="arg-type">string</span>                            | order unique identifier                            |
| `externalId`<span class="arg-type">string</span>                    | order id on specific market                        |
| `timestamp`<span class="arg-type">number</span>                     | last update time                                   |
| `instrument`<span class="arg-type">[Instrument](#instrument)</span> | related instrument                                 |
| `state`<span class="arg-type">[OrderState](#order-state)</span>     | current order state                                |
| `quantity`<span class="arg-type">number</span>                      | quantity of the order                              |
| `quantityExecuted`<span class="arg-type">number</span>              | executed quantity of the order (only limit orders) |
| `rate`<span class="arg-type">number</span>                          | limit rate (only limit orders)                     |
| `averageExecutionRate`<span class="arg-type">number</span>          | order execution rate                               |
| `stopRate`<span class="arg-type">number</span>                      | stop rate (only stop loss orders)                  |
| `createdAt`<span class="arg-type">number</span>                     | creation date                                      |

### Order Type

Please notice, the supported order types depends on the type of the market you are going to trade.

| Enumeration |                             |
| ----------- | --------------------------- |
| `MARKET`    | execute order immediately   |
| `LIMIT`     | add liquidity to the market |

### Order State

The order flow depends on the type of the order.

| Enumeration |                       |
| ----------- | --------------------- |
| `NEW`       | not sent to market    |
| `PENDING`   | for limit rate orders |
| `FILLED`    | execution completed   |
| `CANCELING` | cancel in progress    |
| `CANCELED`  | cancel completed      |
| `REJECTED`  | invalid parameters    |

# Position

Represents a position opened on derivative market.

> Subscribe to position changes:

```typescript
session
  .position(instrumentOf("binance:btc-usdt"))
  .pipe(tap((it) => `position changed: ${it.estimatedUnrealizedPnL}`));
```

| Properties                                                          |                            |
| ------------------------------------------------------------------- | -------------------------- |
| `id`<span class="arg-type">string</span>                            | position unique identifier |
| `timestamp`<span class="arg-type">number</span>                     | last update time           |
| `instrument`<span class="arg-type">[Instrument](#instrument)</span> | related instrument         |
| `averageExecutionRate`<span class="arg-type">number</span>          | position entry rate        |
| `size`<span class="arg-type">number</span>                          | size of the position       |
| `leverage`<span class="arg-type">number</span>                      | leverage of position       |
| `estimatedUnrealizedPnL`<span class="arg-type">number</span>        | current unrealized profit  |
