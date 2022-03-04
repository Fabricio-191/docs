---
order: 3
---

## Creating a database

```js
const SQLiteDatabase = require('@fabricio-191/simple.db').SQLite;

const db = new SQLiteDatabase({
	path: './database.sqlite',
});
```

## Creating a table

```js
const table = db.tables.create('test', ['a', 'b', 'c']);

table.insert({ a: 1, b: 'abc' });
table.update(row => row.a === 1, { b: 'def' });
```

## Transactions

## Optimize

## Closing the database

## Pragma

## Run