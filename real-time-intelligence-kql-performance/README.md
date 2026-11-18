# KQL Performance in Real-Time Intelligence

**One bad query, four steps, 26 times faster – measured on 100 million rows in a Microsoft Fabric Eventhouse.**

📖 **Blog article:** [One bad query, four steps, 26 times faster: KQL performance in Real-Time Intelligence](https://renefuerstenberg.de/microsoftfabric/kql-performance-real-time-intelligence-26-times-faster/)

This folder contains everything you need to rebuild the test from the article: a notebook that
generates the test data and a KQL script with all queries and the measurement.

## Results

Median of all runs, F64 trial capacity, 100 million events.

| Step | Duration | CPU time | Memory | Factor |
|---|---|---|---|---|
| Q0 – starting point | 4.93 s | 7.42 s | 72 MB | 1× |
| Q1 – time filter first | 0.73 s | 0.83 s | 72 MB | 6.8× |
| Q2 – filters before the join | 0.55 s | 0.69 s | 72 MB | 9.0× |
| Q3 – lookup + broadcast join | 0.67 s | 0.61 s | 39 MB | 7.4× |
| Q3b – lookup, no hint | 0.56 s | 0.63 s | 88 MB | 8.8× |
| Q4 – update policy | **0.19 s** | **0.09 s** | 29 MB | **26×** |

## Files

| File | Content |
|---|---|
| `generate_perf_demo_data.ipynb` | Fabric notebook: creates the tables and generates the test data in your Eventhouse |
| `kql_performance_queries.kql` | Queries Q0–Q4 (plus Q3b without the broadcast hint), the update policy for Q4, the measurement query and cleanup |

## Test data

| Table | Rows | Content |
|---|---|---|
| `ShopEvents` | 100 million | Web shop clickstream over 90 days (Aug 18 – Nov 15, 2026): page views, searches, cart, checkout, purchases, errors |
| `Users` | 1 million | Customers with a segment (New, Regular, Loyal, VIP) |
| `Products` | 1,000 | Products in 10 categories |
| `Purchases` | ~2.5 million | Created in step Q4 by an update policy |

The data is synthetic and generated with KQL (`range`, `rand()`), so no external source or license is involved.

## How to run it

1. **Create a separate Eventhouse** for the test (e.g. `eh_perf`). All databases in an Eventhouse share
   the same compute, so the test won't slow down your other workloads.
2. **Copy the Query URI** of the KQL database (database → *Database details* → *Query URI*).
3. **Import the notebook** into your workspace (*Import → Notebook → From this computer*).
4. In the **parameter cell**, set `KUSTO_URI` and `DATABASE`. Keep the defaults for the full test
   (90 days, 10 chunks of 10 million rows). For a quick trial run, use e.g. `DAYS = 2`,
   `CHUNKS = 2`, `ROWS_PER_CHUNK = 100_000`.
5. **Run all.** The full load takes about 40 minutes on an F64. The notebook loads the chunks one
   after another (an Eventhouse accepts only one `.set-or-append` at a time) and can simply be run
   again after an interruption – complete chunks are skipped.
6. Open a **KQL queryset** on the database, paste `kql_performance_queries.kql` and run every query
   at least three times. Then run the measurement query at the end of the script.

The test window is fixed (`Timestamp >= datetime(2026-11-09)`), so results don't depend on the day
you run the queries. If you change `START_DATE` or `DAYS` in the notebook, adjust this date to the
last 7 days of your data.

## Cleanup

When you're done, drop the demo tables to free the storage – either with the last cell of the
notebook or with the commands at the end of the KQL script.

## Requirements

- Microsoft Fabric capacity (the test was measured on an F64 trial; smaller capacities work, just slower)
- A Fabric notebook (Python or PySpark) – the notebook installs `azure-kusto-data` itself