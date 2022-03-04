---
order: 3
---
# Use

## Quick example

```js
const Database = require('simplest.db');
const db = new Database.simple.JSON({
    path: './test.json'
})

db.set('ABC', null)
db.set('foo.bar', [123, 456])

console.log(db.data);
/*
{
    ABC: null,
    foo: {
        bar: [123, 456]
    }
}
*/

db.get('foo.bar[0]') //123
db.numbers.add('foo.bar[1]', 4) //460
db.array.push('foo.bar', 800) //[123, 460, 800]

console.log(db.data);
/*
{
    ABC: null,
    foo: {
        bar: [123, 460, 800]
    }
}
*/

db.delete('foo.bar')

/*
{
    ABC: null
    foo: {}
}
*/

db.clear()

// {}
```

## Options

Options need to be an object like this
```js
{ 
    path: './somepath.db', 
    check: false,
	saveTimeout: 200,
}
```

* `path` is where is going to be the file with the database. By default it's `./simple-db.json` for JSON and `./simple-db.sqlite` for SQLite

* `name` is only required if the database type is `SQLite` (if you know SQL: it's the table name), `simple_db` by default.

* `check` is whenether to check or not if the value was stored correctly, `false` by default

## cache

```js
let db = new Database({
    path: './test.json',
    cacheType: 2
});

let obj = {
    num: 1
}

db.set('abc', obj);

obj.num += 30;

console.log(db.get('abc')); // { num: 31 }
```