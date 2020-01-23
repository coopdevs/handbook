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
