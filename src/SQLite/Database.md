---
order: 3
---

```js
const SQLiteDatabase = require('@fabricio-191/simple.db').SQLite;

const db = new SQLiteDatabase({
	path: './database.sqlite',
});

const table = db.tables.create('test', ['a', 'b', 'c']);

table.insert({ a: 1, b: 'abc' });
```