---
order: 2
---

## Creating a table

```js
const table = db.tables.create('test', ['a', 'b', 'c']);
```

## insert

```js
table.insert({ a: 1, b: 'abc' });
```