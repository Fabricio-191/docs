---
label: Simple
order: 1
icon: container
---

<!--
database
	|--get*
	|--set*
	|--delete*
	|--clear*
	|
	|--keys
	|--values
	|--entries
	|--data
	|
	|--save
	|--array
	|    |--push*
	|    |--extract*
	|    |--splice*
	|    |--sort*
	|    |--includes
	|    |--find
	|    |--findIndex
	|    |--filter
	|    |--map
	|    |--some
	|    |--every
	|    |--reduce
	|    |--random
	|
	|--number
	|    |--add*
	|    |--subtract*
-->

Really simple to use key-value databases.

```js
const Database = require('simplest.db/simple/json');
// or for sqlite
const Database = require('simplest.db/simple/SQLite');

const db = new Database({
    path: './test.json' // or .sqlite for sqlite
});
```

## Options

Options need to be an object like this
```js
{ 
    path: './somepath.db', 
	saveTimeout: 200,
	name: 'users', // SQLite only
	check: true,   // JSON only
}
```

* `path` is where is going to be the file with the database. By default it's `./simple-db.json` for JSON and `./simple-db.sqlite` for SQLite

* `saveTimeout` when you make a change in the database, it will await more changes and when this timeout ends it will save the changes in the database. If you set it to 0 the database will be writen every time. By default `100` for SQLite and `500` for JSON

* `check` (json only) is whenether to check or not if the value was stored correctly, `false` by default

* `name` (sqlite only) it's the table name in the SQLite database, `simple_db` by default. for those who dont know SQLite, it's like a "division" in the database. you can have multiple tables in the same database.

==- Example
```js
const Database = require('simplest.db/simple/SQLite');

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
===

<!--
==- Cache
```js
const db = new Database({
    path: './test.json'
});

let obj = { num: 1 };

db.set('abc', obj);

obj.num += 30;

console.log(db.get('abc')); // { num: 31 }
```
===
-->

==- Keys

Keys in the database are stringified "property accessors" (normal way to access any property in JavaScript but in string)

```js
'ABC'
'foo.bar'
'foo.bar.baz'
`things[0].${something}.${something2}`
```

It can also recieve array of strings as keys

```js
['foo', 'bar'] // same as 'foo.bar'
['foo', 'bar', 'baz'] // same as 'foo.bar.baz'
['things', '0', something, something2] // same as `things[0].${something}.${something2}`
```

===

## Methods

### set

Sets a value in the database if the key point to a unexistent object it will create it.

```js
db.set('foo', 'bar');
db.set('arr.thing', [4, 5, 6]);

/*
{
	foo: 'bar',
	arr: {
		thing: [4, 5, 6]
	}
}
*/
```

### get

Gets a value from the database.

```js
/*
{
	foo: 'bar',
	arr: {
		thing: [4, 5, 6]
	}
}
*/

db.get('foo'); // 'bar'
db.get('arr'); // { thing: [4, 5, 6] }
db.get('arr.thing'); // [4, 5, 6]
db.get('arr.thing[1]'); // 5
```

### delete

Deletes a value from the database

```js
/*
{
	foo: 'bar',
	arr: {
		thing: [4, 5, 6]
	}
}
*/

db.delete('foo');
db.delete('arr.thing');

// { arr: {} }
```

### has

Returns `true` if there it's a value at the provided key in the database or `false` if not.

```js
// { arr: {} }

db.has('foo'); // false
db.has('arr.thing'); // false
db.has('arr'); // true
```

### clear

Deletes all the data in the database

### save

This method will save the data in the database at the path you set in the options. Ignoring the saveTimeout.

### keys/values/entries

```js
console.log(db.entries)
/*
[
    {
        key: 'ABC',
        value: null
    }, 
    {
        key: 'foo',
        value: {
            bar: [123, 460, 800]
        }
    }
]
*/

console.log(db.keys)
//['ABC', 'foo']

console.log(db.values)
/*
[
    null, 
    {
        bar: [123, 460, 800]
    }
]
*/
```