---
label: Database
icon: container
order: 3
---

## Creating a database

```js
const Database = require('simplest.db').SQLite;
const db = new SQLite({
	path: './database.sqlite',
});
```
The only option is `path` which is the path to the database file. any other options are sent to [better-sqlite3](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/api.md#new-databasepath-options) database constructor.

## tables

This object is a table manager

### create

Creates a table. if a table with that name already exists it will throw an error.

```js
// db.tables.create(tableName, columns, defaultValues);
const table = db.tables.create('test', ['a', 'b', 'c']);
```

==- Default values
By default all default values are set to `null`.

```js
const table = db.tables.create('tableName', ['a', 'b', 'c'], { b: 'abc', c: 300 });

table.insert({ b: 'def' }); // { a: null, b: 'def', c: 300 }
```
===

==- Columns (advanced)
The columns while creating a table and while adding a column are [column's-def](https://www.sqlite.org/syntax/column-def.html) from SQLite, by default the type is BLOB. And default values are overriden by default values provided while creating the table.

```js
db.tables.create(
	'test', [
		'a INTEGER PRIMARY KEY',
		'b TEXT UNIQUE',
		'c NOT NULL'
	],
	{ c: 'defaultValue' }
);
```
===

### delete

Deletes a table if it exists
```js
db.tables.delete('tableName');
```

### get

Gives the `Table` object for a table in the database.

## transactions

Transactions are very useful if you want to do multiple operations in a short time.
They put the database in memory, and will only write it on disk when the transaction ends.

see [benchmarks](benchmarks.md).

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

### begin

`db.transactions.begin()` starts a transaction.

### commit

`db.transactions.commit()` ends a transaction.

### rollback

Rollbacks a transaction. if a savepoint is provided it will rollback to that savepoint.

`db.transactions.rollback();`
`db.transactions.rollback('savepointName');`

### savepoint

Creates a savepoint with the specified name.

`db.transactions.savepoint('savepointName');`

### delete savepoint

Deletes a savepoint.

`db.transactions.deleteSavepoint('savepointName');`

## optimize

`db.optimize();`

A tiny function that executes a pragma and a SQL that optimizes the database.

This is intended to be used only in big databases after a lot of operations. you can read about it [here](https://www.sqlite.org/lang_vacuum.html)

## Closing the database

Closes the database connection. if you are going to close the process deliberately, you should call this function.
or if you want to move/rename/delete the database file. while the process is running.
it will call the `optimize` method, as it's recommended in SQLite docs.

```js
db.close();
```

## Interacting directly with the database

These methods are for interating directly with the database. (like the title barely says) these are intended for people that knows SQLite.

### pragma

Runs a [pragma](https://www.sqlite.org/pragma.html) in the database

```js
db.pragma('cache_size = 32000');
```

### prepare

Prepares a SQLite statement. see [better-sqlite3](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/api.md#preparestring---statement)

This is intended for people that knows SQLite, and want to do a query that is not supported by the library.

```js
const data = db.prepare(`SELECT * FROM albums INNER JOIN artists
ON artists.ArtistId = albums.ArtistId`).all();
```