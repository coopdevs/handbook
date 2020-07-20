### List objects owned by user

```sql
select nsp.nspname as object_schema,
       cls.relname as object_name, 
       rol.rolname as owner, 
       case cls.relkind
         when 'r' then 'TABLE'
         when 'm' then 'MATERIALIZED_VIEW'
         when 'i' then 'INDEX'
         when 'S' then 'SEQUENCE'
         when 'v' then 'VIEW'
         when 'c' then 'TYPE'
         else cls.relkind::text
       end as object_type
from pg_class cls
  join pg_roles rol on rol.oid = cls.relowner
  join pg_namespace nsp on nsp.oid = cls.relnamespace
where nsp.nspname not in ('information_schema', 'pg_catalog')
  and nsp.nspname not like 'pg_toast%'
  and rol.rolname = current_user  --- remove this if you want to see all objects
order by nsp.nspname, cls.relname;
```

Note that the owner most of the times will be the user who created the object. 

source: https://dba.stackexchange.com/questions/30061/how-do-i-list-all-tables-in-all-schemas-owned-by-the-current-user-in-postgresql

### List permissions on an object

```sql
\dp <object_name>
```

Example:

```sql
=# \dp otrs_contract
                                         Access privileges
 Schema |     Name      | Type |          Access privileges          | Column privileges | Policies
--------+---------------+------+-------------------------------------+-------------------+----------
 public | otrs_contract | view | administrator=arwdDxt/administrator+|                   |
        |               |      | otrs=r/administrator                |                   |
```

### Find column in database

The following query returns the column matching the specified pattern along with its table

```sql
select c.relname, a.attname
from pg_class as c
    inner join pg_attribute as a on a.attrelid = c.oid
where a.attname like '%operation%' and c.relkind = 'r';
```
```
               relname                |          attname          
--------------------------------------+---------------------------
 ar_account_operation                 | operation_date
 ar_accounting_writing_acc_operations | account_operation_id
 ar_matching_amount                   | account_operation_id
 ar_other_transaction                 | operation_date
```

### Check how much space tables and indexes are taking up

```sql
SELECT
   relname AS table_name,
   pg_size_pretty(pg_total_relation_size(relid)) AS total,
   pg_size_pretty(pg_relation_size(relid)) AS internal,
   pg_size_pretty(pg_table_size(relid) - pg_relation_size(relid)) AS external,
   pg_size_pretty(pg_indexes_size(relid)) AS indexes
   FROM pg_catalog.pg_statio_user_tables
   ORDER BY pg_total_relation_size(relid) DESC;
```

If you want to see details about each specific index:

```sql
SELECT
  schema_name, rel_name, table_size,
  pg_size_pretty(table_size) AS size
FROM (
  SELECT
    nspname AS schema_name,
    relname AS rel_name,
    pg_table_size(pg_class.oid) AS table_size
  FROM pg_class, pg_namespace
  WHERE pg_class.relnamespace = pg_namespace.oid
) _
WHERE schema_name NOT LIKE 'pg_%'
      AND schema_name != 'information_schema'
ORDER BY table_size DESC;
```

## Finding missing index

```sql
SELECT
  relname,
  seq_scan - idx_scan AS too_much_seq,
  CASE
    WHEN seq_scan - coalesce(idx_scan, 0) > 0 THEN 'Missing Index ?'
    ELSE 'OK'
  END,
  pg_size_pretty(pg_relation_size(relname::regclass)) AS rel_size, 
  seq_scan, idx_scan
FROM pg_stat_all_tables
WHERE schemaname = 'public' AND pg_relation_size(relname::regclass) > 80000 
ORDER BY too_much_seq DESC;
```

you'll get something like

```
                 relname                 | too_much_seq |      case       | rel_size | seq_scan | idx_scan 
-----------------------------------------+--------------+-----------------+----------+----------+----------
 decidim_action_logs                     |            7 | Missing Index ? | 1128 kB  |       13 |        6
 decidim_accountability_timeline_entries |            3 | Missing Index ? | 80 kB    |        3 |        0
 decidim_attachments                     |         -110 | OK              | 112 kB   |        9 |      119
 decidim_comments_comments               |         -355 | OK              | 472 kB   |       16 |      371
 decidim_accountability_results          |         -682 | OK              | 152 kB   |       29 |      711
```
source: https://salayhin.wordpress.com/2018/01/02/finding-missing-index-in-postgresql/ plus `pg_size_pretty` taken from https://www.tutorialdba.com/2017/11/find-missing-indexes-of-schema.html.