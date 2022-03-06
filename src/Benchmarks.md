---
order: -2
icon: cpu
---

## Benchmarking

To run the benchmark yourself, run the following commands:

```
git clone https://github.com/Fabricio-191/simple.db
cd simple.db
npm i
npm run bench
```

Everything is measured in operations per second (ops/s). 🔸 means writes/reads every time (no cache use)

## Simple databases

#### SQLite

Action | Simple.SQLite | Simple.SQLite 🔸  | quick.db 🔸  
-------|---------------|-------------------|------------
set    | 237           | 14.59             | 2.49
get    | 5125          | -                 | 6.58 
delete | 713           | 16.27             | 9.12

#### JSON

Action | Simple.JSON | Simple.JSON 🔸  | megadb
-------|-------------|-----------------|--------
set    |     3618    |  0.16           |  0.11 🔸   
get    |     5097    | -               |  1985
delete |     4522    |  0.53           |  2210

<!--
```mermaid
gantt
	title SQLite
    dateFormat x
    axisFormat %s

    section Simple.SQLite
    get    : 0, 5125s
    set    : 0, 237s
    delete : 0, 713s

    section Simple.SQLite 🔸
    get    : 0, 14.59s
    delete : 0, 16.27s

    section quick.db 🔸
    get    : 0, 2.49s
    set    : 0, 6.58s
    delete : 0, 9.12s
```

```mermaid
gantt
	title JSON
    dateFormat x
    axisFormat %s

    section Simple.JSON
    get    :0, 3618s
    set    :0, 5097s
    delete :0, 4522s

    section Simple.JSON 🔸
    get    :0, 0.16s
    delete :0, 0.53s

    section megadb
    get 🔸 :0, 0.11s 
    set    :0, 1985s
    delete :0, 2210s
```
-->

==- Raw results
```
simple.JSON
set    x 3618 ops/sec ±8.01% (77 runs sampled)
get    x 5097 ops/sec ±0.64% (92 runs sampled)
delete x 4522 ops/sec ±0.92% (90 runs sampled)

simple.JSON (writing every time)
set    x 0.16 ops/sec ±14.63% (5 runs sampled)
delete x 0.53 ops/sec ±30.07% (6 runs sampled)

megadb
set (writes every time) x 0.11 ops/sec ±25.79% (5 runs sampled)
get (uses cache)        x 1985 ops/sec ±0.42% (94 runs sampled)
delete (uses cache)     x 2210 ops/sec ±0.40% (94 runs sampled)

simple.SQLite
set    x  237 ops/sec ±0.61% (85 runs sampled)
get    x 5125 ops/sec ±0.46% (92 runs sampled)
delete x  713 ops/sec ±1.75% (90 runs sampled)

simple.SQLite (writing every time)
set    x 14.59 ops/sec ±0.94% (40 runs sampled)
delete x 16.27 ops/sec ±0.47% (44 runs sampled)

quick.db (writes/reads every time)
set    x 2.49 ops/sec ±2.86% (11 runs sampled)
get    x 6.58 ops/sec ±1.55% (21 runs sampled)
delete x 9.12 ops/sec ±1.10% (27 runs sampled)
```
===

## SQLite database

### Conditions

100 rows    | SELECT             | SELECT IN TRANSACTION |
------------|--------------------|-----------------------|
JS          | 2648               | 3983                  |
SQL         | 4911               | 9550                  |
ALL         | 2663               | 2990                  |

100000 rows | SELECT             | SELECT IN TRANSACTION |
------------|--------------------|-----------------------|
JS          | 11.23              | 12.29                 | 
SQL         | 29.63              | 30.24                 |
ALL         | 6.28               | 6.57                  |

### Insert

INSERT      | many (transaction) | each                  |
------------|--------------------|-----------------------|
10 rows     | 6.96               | 0.77                  |
100 rows    | 7.45               | 0.07                  |

<!--
```mermaid
gantt
	title 100 rows
    dateFormat x
    axisFormat %s

    section SELECT
    JS    :0, 2648s
    SQL   :0, 4911s
    ALL   :0, 2663s

    section SELECT IN TRANSACTION
    JS    :0, 3983s
    SQL   :0, 9550s
    ALL   :0, 2990s
```

```mermaid
%%{init: {
	'logLevel': 'debug',
	'gantt': { 'barHeight':25 } 
} }%%
gantt
	title 100000 rows
    dateFormat X
    axisFormat %s

    section SELECT
    JS    :0.0, 11.23
    SQL   :0.0, 29.63
    ALL   :0.0, 6.28

    section SELECT IN TRANSACTION
    JS    :0.0, 12.29
    SQL   :0.0, 30.24
    ALL   :0.0, 6.57
```
-->

==- Raw results
```
with 100 rows:

SELECT
JS  x 2648 ops/sec ±18.51% (80 runs sampled)
SQL x 4911 ops/sec ±7.76% (74 runs sampled)
ALL x 2663 ops/sec ±1.10% (86 runs sampled)

SELECT IN TRANSACTION
JS  x 3983 ops/sec ±7.27% (88 runs sampled)
SQL x 9550 ops/sec ±1.96% (87 runs sampled)
ALL x 2990 ops/sec ±4.24% (85 runs sampled)


with 100000 rows:

SELECT
JS  x 11.23 ops/sec ±3.75% (32 runs sampled)
SQL x 29.63 ops/sec ±0.69% (52 runs sampled)
ALL x  6.28 ops/sec ±6.57% (20 runs sampled)

SELECT IN TRANSACTION
JS  x 12.29 ops/sec ±0.90% (35 runs sampled)
SQL x 30.24 ops/sec ±0.64% (53 runs sampled)
ALL x  6.57 ops/sec ±1.63% (21 runs sampled)


inserting 10 rows:
many (transaction) x 6.96 ops/sec ±24.65% (23 runs sampled)
each               x 0.77 ops/sec ±9.62% (6 runs sampled)

inserting 100 rows
many (transaction) x 7.45 ops/sec ±7.59% (21 runs sampled)
each               x 0.07 ops/sec ±2.88% (5 runs sampled)
```
===