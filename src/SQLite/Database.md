---
label: Database
icon: database
order: 3
---

## Creating a database

```js
const SQLiteDB = require('simplest.db/SQLite');
const db = new SQLiteDB({
	path: './database.sqlite',
});
```
The only option is `path` which is the path to the database file. any other options are sent to [better-sqlite3](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/api.md#new-databasepath-options) database constructor.

## Tables

### Create

### Delete

### Get

## Transactions

Transactions are very useful if you want to do multiple operations in a short time. see [benchmarks](../benchmarks.md/#sqlite-database).

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

### Begin

`db.transtions.begin()` starts a transaction. puts the database in memory instead of disk. and the changes during the transaction are applied only to the database in memory.

### Commit

`db.transtions.commit()` ends a transaction. writes the database in memory to the database in the disk.

### Rollback

### Savepoint

### Delete savepoint

## Optimize

A tiny function that executes a pragma and a SQL that optimizes the database.

This is intended to be used only in big databases after a lot of operations. you can read about it [here](https://www.sqlite.org/lang_vacuum.html)

```js
db.optimize();
```

## Closing the database

Closes the database connection. if you are going to close the process deliberately, you should call this function. it will allow you to move/rename/delete the database file.  
it will call the `optimize` method, as it's recommended in SQLite docs.

```js
db.close();
```

## Interating directly with the database

These methods are for interating directly with the database. (like the title barely says) these are intended for people that knows SQLite.

### Pragma

Runs a [pragma](https://www.sqlite.org/pragma.html) in the database

```js
db.pragma('cache_size = 32000');
```

### Prepare

Prepares a SQLite statement. see [better-sqlite3](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/api.md#preparestring---statement)

This is intended for people that knows SQLite, and want to do a query that is not supported by the library.

```js
const data = db.prepare(`SELECT * FROM albums INNER JOIN artists
ON artists.ArtistId = albums.ArtistId`).all();
```