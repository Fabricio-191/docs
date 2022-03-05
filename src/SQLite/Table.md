---
order: 2
---

# Creating a table

```js
// db.tables.create(tableName, columns, defaultValues);
const table = db.tables.create('test', ['a', 'b', 'c']);
```

==- Default values
By default all default values are set to `null`.

```js
const table = db.tables.create(
	'test',
	['a', 'b', 'c'],
	{ a: 300, b: 'abc' }
);

table.insert({ b: 'def', c: 'thing' }); // { a: 300, b: 'def', c: 'thing' }
table.insert({ a: 400 });               // { a: 400, b: 'abc', c: null }
```
===

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

## Data

Data must be a object where the keys are the column names and the values are the values to be used.

The only acepted values are `null`, numbers, bigint, strings and buffers. for booleans you should use `1` and `0`.
If a value is not provided or it's undefined, the default value will be used.

```js
const data = {
	a: 1,
	b: 123,
	c: 'def'
};

table.insert(data);
```

## Conditions

Conditions must be strings or functions, using SQL syntax is faster than using functions.

I recommend use SQL syntax for simple conditions and using functions for complex conditions or things that need external data.

### Using a function

Similar to `Array.prototype.filter()` and `Array.prototype.find()` where the provided value is a row of Data from the table.
You can use external variables and functions.

Example:

```js
table.insert([
	{ a: 1, b: 'abc' },
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' },
]);

console.log(table.select(row => row.a > 1));
/*
[
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' }
]
*/

table.select(row => Math.abs(row.a) > 1);
table.select(row => Math.abs(row.a) > 1 && row.b === 'def');
```

!!!
You cannot use async functions in conditions. As them returns a promise, will be seen always as a truthy value.
!!!

### Using a SQL syntax

You can see the oficial SQLite syntax [here](https://www.sqlite.org/syntax/expr.html).
Or you can use these examples

```js
table.select('a > 1');
table.select('abs(a) > 1');
table.select('abs(a) AND b = "def"');
```

==- More examples
To see what functions (`abs`, `round`, `length`, `random`, etc.) you can use see [this](https://www.sqlite.org/lang_corefunc.html).

Here i will put some simple examples so people that does not know SQLite can understand how to use them.

```js
*   /   %
+   -
>   <   <=   >=
==  !=

NOT
AND
OR
```

```sql
a == b AND c <= 500;
a > b OR b != "13";
(a - 3) % 2 == 0;
```
===

# Methods

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

```js
console.log(table.select()); 
/*
[
	{ a: 1, b: 'abc' },
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' }
]
*/

console.log(table.select('a > 1'));
/*
[
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' }
]
*/

console.log(table.select(row => row.a > 1));
/*
[
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' }
]
*/
```

Using direct SQL is much faster than using a function.

## get

Same as select but instead of returning an array of rows, returns the first row.

```js
console.log(table.select()); 
/*
[
	{ a: 1, b: 'abc' },
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' }
]
*/

console.log(table.get('a > 1')); // { a: 2, b: 'def' }
```

## update

## replace

## delete

## deleteAll