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
The only option is `path` which is the path to the database file. any other options are sent to [better-sqlite3](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/api.md#new-databasepath-options) database constructor.

## Transactions

Transactions are very useful if you want to do multiple operations in a short time. see [benchmarks](../benchmarks.md)

for example, instead of doing this:
```js
table.update(row => row.a === 1, { a: 100 });
table.update(row => row.a === 2, { a: 200 });
table.update(row => row.a === 3, { a: 300 });
```

you should do:

```js
db.transactions.begin();

table.update(row => row.a === 1, { a: 100 });
table.update(row => row.a === 2, { a: 200 });
table.update(row => row.a === 3, { a: 300 });

db.transactions.commit();
```

## Optimize

## Closing the database

## Pragma

## Run