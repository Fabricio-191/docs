---
label: simplest.db
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

## About

This module really simplifies the creation and use of databases.
But sacrifices eficciency for simplicity.

It's thinked for begginers or for projects where speed isn't vital. 

## Installation

To install the package, run the following command:
```sh
npm i simplest.db
```

!!!
if you have any problem in the installation related to `better-sqlite3` see [this](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/troubleshooting.md)
!!!

if you don't want to use the SQLite database, you should do 

```sh
npm i simplest.db --no-optional
```
