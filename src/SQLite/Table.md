---
icon: columns
order: 2
---

# Table

```js
// db.tables.create(tableName, columns, defaultValues);
const table = db.tables.create('test', ['a', 'b', 'c']);
```

==- Default values
By default all default values are set to `null`.

```js
const table = db.tables.create('test', ['a', 'b', 'c'], { b: 'abc', c: 300 });

table.insert({ b: 'def' }); // { a: null, b: 'def', c: 300 }
```
===

==- Columns (advanced)
The columns while creating a table and while adding a column are [column's-def](https://www.sqlite.org/syntax/column-def.html) from SQLite, by default the type is BLOB. And default values are overriden by default values provided while creating the table.

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

The only acepted values are `null`, numbers, bigints, strings and buffers. for booleans you should use `1` and `0`.  
If a value is not provided or it's undefined, the default value will be used.

```js
const data = {
	a: 1,
	b: 123,
	c: 'def'
};

table.insert(data);
```

## Condition

Conditions must be strings or functions, using SQL syntax is faster than using functions. see [benchmarks](../benchmarks/#conditions)

I recommend use SQL syntax for simple conditions and for large amounts of data and using functions for complex conditions or things that need external data.

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
```

!!!
You cannot use async functions in conditions. As them returns a promise, will be seen always as a truthy value.
!!!

### Using a SQL syntax

You can see the oficial SQLite syntax [here](https://www.sqlite.org/syntax/expr.html).
Or you can use these examples

```js
table.select('a > 1');
table.select('abs(a) <= 1');
table.select('(a % 2) == 0 AND b == "def"');
```

==- More examples
To see what functions (`abs`, `round`, `length`, `random`, etc.) you can use see [this](https://www.sqlite.org/lang_corefunc.html).

Here i will put some simple examples so people that does not know SQLite can understand how to use them.

```js
*   /   %
+   -
>   <   <=   >=
==  !=

NOT // !
AND // &&
OR  // ||
```

```js
table.select("a == 'abc'");
table.select("a == 'abc' AND b == `don't`");
table.select('b <= 500');
table.select('b > c OR a != "13"');
table.select('(a - 3) % 2 == 0');
```
===

## Methods

### insert

see [data](#data)

```js
// table.insert(data);
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
inserting multiple values at once is much faster than inserting one by one. see [benchmarks](../benchmarks/#insert)

### select

returns all the rows that satisfy the condition, see [condition](#condition)

```js
// table.select(condition);
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

### get

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

### delete

Deletes all rows that satisfy the condition. see [condition](#condition)

For security if you dont provide a condition to the `delete` method, it will throw an error. if you want to delete all rows use `table.deleteAll()`

```js
// table.delete(condition);
console.log(table.select()); 
/*
[
	{ a: 1, b: 'abc' },
	{ a: 2, b: 'def' },
	{ a: 3, b: 'ghi' }
]
*/

table.delete('a < 2 OR b == "ghi"');

console.log(table.select()); 
// [{ a: 2, b: 'def' }]
```

### update

Updates all the rows that satisfy the condition with the given data, see [condition](#condition) and [data](#data)

```js
// table.update(condition, data);

console.log(table.select()); 
/*
[
	{ a: 1, b: 'aaa' },
	{ a: 2, b: 'bbb' },
	{ a: 3, b: 'ccc' }
]
*/

table.update(row => row.a === 1, { b: 'ddd' });
table.update(row => row.a === 3, { a: 4, b: 'eee' });

console.log(table.select()); 
/*
[
	{ a: 1, b: 'ddd' },
	{ a: 2, b: 'bbb' },
	{ a: 4, b: 'eee' }
]
*/
```


### replace

The replace method will only work with an `UNIQUE` column or a `PRIMARY KEY`, see [Columns (advanced)](#table)

This method will replace the row where the `UNIQUE` or `PRIMARY KEY` is equal to the given value. If there is no row with the given value, it will insert the data.

```js
const table = db.tables.create('test', ['a PRIMARY KEY', 'b']);

table.insert([
	{ a: 1, b: 'a' },
	{ a: 2, b: 'b' },
	{ a: 3, b: 'c' },
]);

table.replace({ a: 4, b: 'd' });
// as none value coincides it will insert the data
/*
[
  { a: 1, b: 'a' },
  { a: 2, b: 'b' },
  { a: 3, b: 'c' },
  { a: 4, b: 'd' }
]
*/

table.replace({ a: 2, b: 'e' });
/*
[
  { a: 1, b: 'a' },
  { a: 3, b: 'c' },
  { a: 4, b: 'd' },
  { a: 2, b: 'e' }
]
*/
```

## Columns

### add

A method to add a column to the table. you can also set the default value for the column.
if no default value is provided it's gonna be `null`.

The `fill` parameter indicates whenether to fill the new column with the default value or not. (default: `true`)

```js
const table = db.tables.create('test', ['a', 'b', 'c']);
table.insert({ a: 1, b: 2, c: 3 });

// table.columns.add(column, defaultValue, fill)
table.columns.add('d')
console.log(table.get()); // { a: 1, b: 2, c: 3, d: null }

table.columns.add('e', 'default');
console.log(table.get()); // { a: 1, b: 2, c: 3, d: null, e: 'default' }

table.columns.add('f', 'default', false);
console.log(table.get()); // { a: 1, b: 2, c: 3, d: null, e: 'default', f: null }
```

### delete

```js
const table = db.tables.create('test', ['a', 'b', 'c']);
table.insert({ a: 1, b: 2, c: 3 });

// table.columns.delete(columnName)
table.columns.delete('c')

console.log(table.get()); // { a: 1, b: 2 }
```

### rename

```js
const table = db.tables.create('test', ['a', 'b', 'c']);
table.insert({ a: 1, b: 2, c: 3 });

// table.columns.delete(columnName)
table.columns.rename('c', 'd');
console.log(table.get()); // { a: 1, b: 2, d: 3 }
```
