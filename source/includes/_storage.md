# <strong class="section-header">Storage</strong>

Provides persisted storage for session feed and measurement data.

# SQLite

> To install package, use following command:

```console
$ yarn add @quantform/sqlite
```

> Register providers in `qf.config.ts` file:

```typescript
run({
  feed: SQLiteFeed(),
  measurement: SQLiteMeasurement(),
});
```

Manages session and measurement data in local quantform working environment.

This kind of storage is supposed to store data in local project <code>.quantform</code> directory.
