---
icon: eye-closed
visibility: hidden
---

## Multiple db's in the same file

This is only for SQLite, the `name` option specifies the table in the database file.

```js
const Database = require('simplest.db').SQLite;

const db = new Database({
    path: './simple-db.sqlite',
	name: 'users',
});

const db2 = new Database({
    path: './simple-db.sqlite',
	name: 'servers',
});

// db and db2 are in the same file but they do not share values
```

## Cache

The cache is an intermediate layer between the database and the user. because writing the data on disk directly is really slow. see the [benchmarks](Benchmarks.md).

This layers improves performance but it may lead to weird behavior if you don't know about it.

```js
const db = new Database({
    path: './test.json'
});

let obj = { num: 1 };

db.set('abc', obj);

obj.num += 30;

console.log(db.get('abc')); // { num: 31 }
```