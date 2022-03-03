---
order: -2
icon: cpu
---

## Last results

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

## Benchmarking

To run the benchmark yourself, run the following commands:

```
git clone https://github.com/Fabricio-191/simple.db
npm i
npm run comp
```
