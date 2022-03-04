---
order: 2
---

## Creating a table

```js
// db.tables.create(tableName, columns, defaultValues);
const table = db.tables.create('test', ['a', 'b', 'c']);
```

==- Columns (advanced)
The columns while creating a table are [column's-def](https://www.sqlite.org/syntax/column-def.html) from SQLite by default the type is BLOB. And default values are overriden by default values provided while creating the table.

```js
db.tables.create(
	'test', [
		'a INTEGER PRIMARY KEY',
		'b UNIQUE',
		'c NOT NULL'
	],
	{ c: 'defaultValue' }
);
```
===

==- Default values
By default all default values are set to `null`.

```js
const table = db.tables.create('test', ['a', 'b', 'c'], { a: 123, b: 'abc' });

table.insert([
	{ b: 'def' },
	{ a: 456 }
]);

console.log(table.selectAll());
/*
[
	{ a: 123, b: 'def', c: null },
	{ a: 456, b: 'abc', c: null }
]
*/
```
===

## insert

```js
table.insert({ a: 1, b: 'abc' });
```

## select

## update

## replace

## delete

## deleteAll