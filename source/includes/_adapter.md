# <strong class="section-header">Adapter</strong>

# Binance

> To install package, use following command:

```console
$ yarn add @quantform/binance
```

Exposes an access to [binance.com](https://binance.com) spot market.

## Configuration

> Provide following environment variables:

```env
QF_BINANCE_APIKEY=your_api_key
QF_BINANCE_APISECRET=your_api_secret
```

```typescript
// and create a new instance with parameter less constructor.
const adapter = new BinanceAdapter();
```

> or configure adapter in code:

```typescript
const adapter = new BinanceAdapter({
  key: "your_api_key",
  secret: "your_api_secret",
});
```

There are to ways to configure adapter:

- define environment variables in your system or in local .env file
- provide sensitive data in adapter constructor (not recommended, due security violation)

<aside class="warning">Remember to keep api credentials secure, never store them in a code or provide to third-parties.</aside>

# Binance Futures

> To install package, use following command:

```console
$ yarn add @quantform/binancefuture
```

Exposes an access to [binance.com](https://binance.com) futures market.

## Configuration

> Provide following environment variables:

```env
QF_BINANCEFUTURE_APIKEY=your_api_key
QF_BINANCEFUTURE_APISECRET=your_api_secret
```

```typescript
// and create a new instance with parameter less constructor.
const adapter = new BinanceFutureAdapter();
```

> or configure adapter in code:

```typescript
const adapter = new BinanceFutureAdapter({
  key: "your_api_key",
  secret: "your_api_secret",
});
```

There are to ways to configure adapter:

- define environment variables in your system or in local .env file
- provide sensitive data in adapter constructor (not recommended, due security violation)

<aside class="warning">Remember to keep api credentials secure, never store them in a code or provide to third-parties.</aside>
