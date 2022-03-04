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
const table = db.tables.create(
	'test',
	['a', 'b', 'c'],
	{ a: 300, b: 'someDefaultValue' }
);

table.insert([
	{ b: 'def', c: 'thing' },
	{ a: 700 }
]);

console.log(table.selectAll());
/*
[
	{ a: 300, b: 'def', c: 'thing' },
	{ a: 700, b: 'someDefaultValue', c: null }
]
*/
```
===

## insert

```js
table.insert({ a: 1, b: 'abc' });
```
if you want to insert multiple values do this:
```js
table.insert([
	{ a: 1, b: 'abc' },
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' },
]);
```
inserting multiple values at once is much faster than inserting one by one. see [benchmarks](../benchmarks.md)

## select

## update

## replace

## delete

## deleteAll