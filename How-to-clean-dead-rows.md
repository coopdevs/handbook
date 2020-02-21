Sometimes the auto-vacuum settings of your PostgreSQL server might not be able to keep the pace at which your app is marking rows as dead.

Here's how to manually deal with this, heavily inspired by https://www.cybertec-postgresql.com/en/reasons-why-vacuum-wont-remove-dead-rows/.

## Check tables with the most dead rows

First, check where are the dead rows within the database to see where to start cleaning.

```sql
openfoodnetwork-> FROM pg_stat_all_tables
openfoodnetwork-> ORDER BY n_dead_tup
openfoodnetwork->     / (n_live_tup
openfoodnetwork(>        * current_setting('autovacuum_vacuum_scale_factor')::float8
openfoodnetwork(>           + current_setting('autovacuum_vacuum_threshold')::float8)
openfoodnetwork->      DESC
openfoodnetwork-> LIMIT 10;
schemaname |        relname        | n_live_tup | n_dead_tup |        last_autovacuum
------------+-----------------------+------------+------------+-------------------------------
public     | delayed_jobs          |         45 |       1498 | 2020-02-15 10:43:16.303955+01
pg_catalog | pg_depend             |          0 |        215 |
pg_catalog | pg_trigger            |          0 |         60 |
public     | spree_states          |        265 |        107 | 2020-02-14 16:13:19.552129+01
public     | tag_rules             |         87 |         65 |
public     | spree_adjustments     |     374999 |      70631 | 2020-02-02 10:00:11.048836+01
public     | spree_users           |       6921 |       1327 | 2019-12-29 10:18:54.742794+01
pg_catalog | pg_statistic          |        987 |        218 | 2020-02-14 14:16:45.235952+01
public     | spree_inventory_units |     531520 |      90381 | 2019-07-23 02:37:40.569874+02
public     | spree_products_taxons |       8160 |       1428 |
(10 rows)
```

## VACUUM a table

Now, time to use the vacuum cleaner.

```sql
openfoodnetwork=> VACUUM (VERBOSE) spree_adjustments;
INFO:  vacuuming "public.spree_adjustments"
INFO:  scanned index "spree_adjustments_pkey" to remove 62940 row versions
DETAIL:  CPU 0.07s/0.08u sec elapsed 6.95 sec
INFO:  scanned index "index_adjustments_on_order_id" to remove 62940 row versions
DETAIL:  CPU 0.04s/0.03u sec elapsed 3.10 sec
INFO:  "spree_adjustments": removed 62940 row versions in 6683 pages
DETAIL:  CPU 0.18s/0.11u sec elapsed 0.83 sec
INFO:  index "spree_adjustments_pkey" now contains 376515 row versions in 6154 pages
DETAIL:  57288 index row versions were removed.
27 index pages have been deleted, 9 are currently reusable.
CPU 0.00s/0.00u sec elapsed 0.01 sec.
INFO:  index "index_adjustments_on_order_id" now contains 376515 row versions in 1998 pages
DETAIL:  4852 index row versions were removed.
2 index pages have been deleted, 1 are currently reusable.
CPU 0.00s/0.00u sec elapsed 0.01 sec.
INFO:  "spree_adjustments": found 19211 removable, 252786 nonremovable row versions in 8093 out of 10674 pages
DETAIL:  3784 dead row versions cannot be removed yet.
There were 567171 unused item pointers.
Skipped 0 pages due to buffer pins.
0 pages are entirely empty.
CPU 0.47s/0.33u sec elapsed 13.89 sec.
INFO:  vacuuming "pg_toast.pg_toast_28342"
INFO:  index "pg_toast_28342_index" now contains 0 row versions in 1 pages
DETAIL:  0 index row versions were removed.
0 index pages have been deleted, 0 are currently reusable.
CPU 0.00s/0.00u sec elapsed 0.00 sec.
INFO:  "pg_toast_28342": found 0 removable, 0 nonremovable row versions in 0 out of 0 pages
DETAIL:  0 dead row versions cannot be removed yet.
There were 0 unused item pointers.
Skipped 0 pages due to buffer pins.
0 pages are entirely empty.
CPU 0.00s/0.00u sec elapsed 0.00 sec.
VACUUM
```

There are a number of things you should be aware of form that output. Check out https://www.cybertec-postgresql.com/en/reasons-why-vacuum-wont-remove-dead-rows/ and the [PostgreSQL docs](https://www.postgresql.org/docs/current/sql-vacuum.html) to learn more. 
