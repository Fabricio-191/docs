---
label: simplest.db
icon: container
---

## About

This module really simplifies the creation and use of databases.
But sacrifices eficciency for simplicity.

It's thinked for begginers or for projects where speed isn't vital. 

==- Sad reality
Using JSON as a database is not a good idea. and using SQLite tables with two rows to store JSON is also not the best idea.

This module was actually made as a better alternative to quick.db and megadb.  

megadb: 
* It's slow, see [this](./Benchmarks.md#json)
* has a high change of going corrupt as it writes the changes on the disk on every call of the `establecer` (`set`) method.
* it uses cache on the delete method but not in the set method, which means poor design.

quick.db:
* It's slow, see [this](./Benchmarks.md#sqlite)
* You cannot set a custom database file, it's always `./json.sqlite`
===

## Installation

To install the package, run the following command:
```sh
npm i simplest.db
```

!!!
if you have any problem in the installation related to `better-sqlite3`, try running the command as root/administrator or see [this](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/troubleshooting.md)
!!!

if you don't want to use the SQLite database, you should do 

```sh
npm i simplest.db --no-optional
```

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
