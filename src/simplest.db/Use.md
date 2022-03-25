---
icon: arrow-right
---

```js
const Database = require('simplest.db').JS0N; // with zero 
// or for sqlite
const Database = require('simplest.db').SQLite;

const db = new Database(options);
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

* `name` (sqlite only) `simple_db` by default. See how to use it [here](./extras.md#Multiple_SQLite_databases_in_the_same_file)

==- Keys
Keys in the database are like "property accessors" (normal way to access any property in JavaScript) but in string. And the only separator accepted is a dot `.` 

```js
'ABC'
'foo.bar'
`foo.bar.${something}` // same as foo.bar[something]
`things.0.bar.${something}` // same as things[0].bar[something]
```

It can also recieve array of strings as keys

```js
['foo', 'bar'] // same as 'foo.bar'
['foo', 'bar', 'baz'] // same as 'foo.bar.baz'
['things', '0', something, something2] // same as `things[0][something][something2]`
```
===

## Methods

### set

Sets a value in the database.

```js
db.set('foo', 'bar');
db.set('thing.arr', [4, 5, 6]);

/*
{
	foo: 'bar',
	thing: {
		arr: [4, 5, 6]
	}
}
*/
```

if the key points to an object that it does not exists it will set it as an empty object `{}`

==- Example
```js
db.set('a.b.c', 'foo');
/*
{
	foo: 'bar',
	thing: {
		arr: [4, 5, 6]
	},
	a: {
		b: {
			c: 'foo'
		}
	}
}
*/
```
===

### get

Gets a value from the database.

```js
/*
{
	foo: 'bar',
	thing: {
		arr: [4, 5, 6]
	}
}
*/

db.get('foo'); // 'bar'
db.get('arr'); // { thing: [4, 5, 6] }
db.get('thing.arr'); // [4, 5, 6]
db.get('thing.arr.1'); // 5
db.get('thing.a.b.c'); // undefined
```

### delete

Deletes a value from the database

```js
/*
{
	foo: 'bar',
	thing: {
		arr: [4, 5, 6]
	}
}
*/

db.delete('foo');
db.delete('thing.arr');

// { thing: {} }
```

### has

Returns `true` if there it's a value at the provided key in the database or `false` if not.

```js
// { thing: {} }

db.has('foo'); // false
db.has('thing.arr'); // false
db.has('thing'); // true
```

### clear

Deletes all the data in the database

### save

This method will save the data in the database at the path you set in the options. Ignoring the saveTimeout.

### keys/values/entries

```js
/*
{
	foo: 'bar',
	thing: {
		arr: [4, 5, 6]
	}
}
*/

console.log(db.keys)
//['foo', 'thing']

console.log(db.values)
/*
[
    'bar', 
    {
		arr: [4, 5, 6]
	}
]
*/

console.log(db.entries)
/*
[
    {
        key: 'foo',
        value: 'bar'
    }, 
    {
        key: 'thing',
        value: {
			arr: [4, 5, 6]
		}
    }
]
*/
```