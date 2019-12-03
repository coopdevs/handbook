## Problem
An admin can remove their own permissions

## GUI solution
If available, create a user with admin powers, then use that user to recover your permissions back.

## DB solution
We can imagine some scenarios where that's not possible. We want to make sure we know which switches to touch if the time comes.

### Summary

1. Identify your user. Do `select id,login from res_users where login='MY_LOGIN_NAME';`
2. Identify the module this permission comes from. You can look at the url to find something like `account_banking_pain_base`.
3. Identify the module category that module belongs to. Use its name like `select name,category_id from ir_module_module where name='account_banking_pain_base'`
4. Identify the permission you want back. It will be one of the id listed by `select id,name,category_id from res_groups where category_id=1 order by id;`, where `category_id` will be the one from the previous step.
5. Using the user id from `res_users` and the permission id from `res_groups`:
  1. Check if your user has this permission with `select * from res_groups_users_rel where gid=1 and uid=3`. If it's not, then:
  2. Create this membership: `insert into res_groups_users_rel(gid, uid) values(1,2);` using your values.

### Tables
The managing happens inside `base` module, but modules can create new permission groups for their own needs.

We list the interesting tables below.

#### res_users
```
odoo=> \dt res_users
                                            Table "public.res_users"
      Column       |            Type             | Collation | Nullable |                Default                
-------------------+-----------------------------+-----------+----------+---------------------------------------
 id                | integer                     |           | not null | nextval('res_users_id_seq'::regclass)
 active            | boolean                     |           |          | true
 login             | character varying           |           | not null | 
 password          | character varying           |           |          | NULL::character varying
 company_id        | integer                     |           | not null | 
 partner_id        | integer                     |           | not null | 
 signature         | text                        |           |          | 
 action_id         | integer                     |           |          | 
 share             | boolean                     |           |          | 
 create_uid        | integer                     |           |          | 
 create_date       | timestamp without time zone |           |          | 
 write_uid         | integer                     |           |          | 
 write_date        | timestamp without time zone |           |          | 
 password_crypt    | character varying           |           |          | 
 alias_id          | integer                     |           |          | 
 notification_type | character varying           |           | not null | 
 sale_team_id      | integer                     |           |          | 
 pos_security_pin  | character varying(32)       |           |          | 
Indexes:
    "res_users_pkey" PRIMARY KEY, btree (id)
    "res_users_login_key" UNIQUE CONSTRAINT, btree (login)
...
```
```
odoo=> select id,active,login,notification_type,company_id from res_users limit 2;
 id | active |  login  | notification_type | company_id 
----+--------+---------+-------------------+------------
  5 | t      | demo    | email             |          1
  3 | f      | default | email             |          1
(2 rows)
```

#### res_groups

Each row represents a permission group. If a user belongs to a group, then it has that permission.

```
odoo=> \d res_groups
                                         Table "public.res_groups"
   Column    |            Type             | Collation | Nullable |                Default                 
-------------+-----------------------------+-----------+----------+----------------------------------------
 id          | integer                     |           | not null | nextval('res_groups_id_seq'::regclass)
 name        | character varying           |           | not null | 
 comment     | text                        |           |          | 
 category_id | integer                     |           |          | 
 color       | integer                     |           |          | 
 share       | boolean                     |           |          | 
 is_portal   | boolean                     |           |          | 
 create_uid  | integer                     |           |          | 
 create_date | timestamp without time zone |           |          | 
 write_uid   | integer                     |           |          | 
 write_date  | timestamp without time zone |           |          | 
Indexes:
    "res_groups_pkey" PRIMARY KEY, btree (id)
    "res_groups_name_uniq" UNIQUE CONSTRAINT, btree (category_id, name)
    "res_groups_category_id_index" btree (category_id)
...
```

```
odoo=> select id,name,category_id,comment from res_groups where comment != '' order by id;
 id |           name           | category_id |                                                   comment                                                    
----+--------------------------+-------------+--------------------------------------------------------------------------------------------------------------
 10 | Portal                   |          63 | Portal members have specific access rights (such as record rules and restricted menus).                     +
    |                          |             |                 They usually do not belong to the usual Odoo groups.
 11 | Public                   |          63 | Public users have specific access rights (such as record rules and restricted menus).                       +
    |                          |             |                 They usually do not belong to the usual Odoo groups.
 14 | Officer                  |           5 | the user will be able to approve document created by employees.
 15 | Manager                  |           5 | the user will have an access to the human resources configuration as well as statistic reports.
 22 | User: Own Documents Only |          44 | the user will have access to his own data in the sales application.
 23 | User: All Documents      |          44 | the user will have access to all records of everyone in the sales application.
 24 | Manager                  |          44 | the user will have an access to the sales configuration as well as statistic reports.
 49 | Tax display B2B          |           1 | Show line subtotals without taxes (B2B)
 50 | Tax display B2C          |           1 | Show line subtotals with taxes included (B2C)
 69 | Manual Attendance        |          54 | The user will gain access to the human resources attendance menu, enabling him to manage his own attendance.
 70 | Enable PIN use           |           1 | The user will have to enter his PIN to check in and out manually at the company screen.
(11 rows)
```

#### res_groups_users_rel

There's a relation table between groups and users. Each row represents a membership relation. Only two columns with non-unique values, so each user can belong to more than one group, and each group can have more than one user; what one would expect.

```
odoo=> \d res_groups_users_rel
        Table "public.res_groups_users_rel"
 Column |  Type   | Collation | Nullable | Default
--------+---------+-----------+----------+---------
 gid    | integer |           | not null |
 uid    | integer |           | not null |
```

#### ir_module_category

But how can we tell apart groups with same name and no comment? Id's and date-times depend on module install order and there's no other hint... except category_id. 

From `res_groups`:
```
odoo=> select * from res_groups where name = 'Manager' and comment != '';
 id |  name   |                                             comment                                             | category_id | color | share | is_portal | create_uid |        create_date         | write_uid |         write_date         
----+---------+-------------------------------------------------------------------------------------------------+-------------+-------+-------+-----------+------------+----------------------------+-----------+----------------------------
 15 | Manager | the user will have an access to the human resources configuration as well as statistic reports. |           5 |       | f     | f         |          1 | 2019-07-18 10:19:03.915946 |         1 | 2019-07-18 10:19:03.915946
 24 | Manager | the user will have an access to the sales configuration as well as statistic reports.           |          44 |       | f     | f         |          1 | 2019-07-18 10:19:35.603947 |         1 | 2019-07-18 10:19:35.603947
(2 rows)
```
... follow the track:

```
odoo=> select id,name,description from ir_module_category where id=5 or id=44;
 id |   name    |                         description                          
----+-----------+--------------------------------------------------------------
 44 | Sales     | Helps you handle your quotations, sale orders and invoicing.
  5 | Employees | Helps you manage your employees.
(2 rows)
```

#### ir_module_module

Finally, each module has exactly one module_category that relates to. Module categories group modules.

For instance:

```
odoo=> select id,name,category_id from ir_module_module order by name limit 10;
 id  |                 name                 | category_id 
-----+--------------------------------------+-------------
  91 | account                              |          11
  17 | account_analytic_default             |          11
 297 | account_asset                        |          11
 271 | account_banking_mandate              |          26
  26 | account_banking_pain_base            |           1
 128 | account_banking_sepa_credit_transfer |          26
 176 | account_banking_sepa_direct_debit    |          26
 312 | account_bank_statement_import        |          11
 258 | account_budget                       |          11
  86 | account_cancel                       |          11
(10 rows)
```
These modules are divided in 3 categories:
```
odoo=> select id,name from ir_module_category where id=1 or id=11 or id=26;
 id |        name        
----+--------------------
 11 | Accounting
 26 | Banking addons
  1 | Technical Settings
(3 rows)
```