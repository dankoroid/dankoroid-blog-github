---
when writing SQL UPSERT query like
```
INSERT INTO user_item (user_id, item_id, quantity)
VALUES (:userId, :itemId, :quantity)
ON DUPLICATE KEY UPDATE quantity = quantity + :quantity
```
make sure that `ON DUPLICATE KEY` is actually done against such pair of columns that has defined unique constraint/index.

Otherwise it will not update existing row but just create a new one

---
PostgreSQL - in `array_agg` you can use ordering of elements (even by aggregated element itself)

This could be helpful both for comparing of arrays and just for a better result of query 

Example:
```
select p.id, count(c.id), array_agg(c.name order by c.name)
from parent p
join child c on c.parent_id = p.id
group by p.id;
```
[Manual](https://www.postgresql.org/docs/current/sql-expressions.html#SYNTAX-AGGREGATES)

---
Postgresql - how to cast from one type to other:

'123'::bigint

---
[PostgreSQL] SQL query to select boolean when all children of child match some condition:
```
SELECT
    case
        when
            not exists (
                select child.identifier
                from child_table ct
                where ct.parent_id = :given_id
                and ct.field <> 'SUCCESS'
            )
        then TRUE
        else FALSE
    end
```
It is crucial to use both NOT EXISTS and negating condition - to prevent cases of partial match

---
SQL - When needed to load parent together with some fields from any 1 child:
```
select
    parent.field_1
    (select child.field_1 from child where child.parent_id = parent.id limit 1) as child_field_1
from parent
```
Reasoning for such query at all - probably DB evolution, as some fields seem to be suited better in parent than in children

---
PostgreSQL - counting amount of rows in table:

1. Exact count (Slow for big tables).
  <br>With concurrent write operations, it may be outdated the moment you get it.
    ```
    SELECT count(*) AS exact_count FROM myschema.mytable;
    ```
2. Estimate (Extremely fast)
  <br>Typically, the estimate is very close. How close, depends on whether ANALYZE or VACUUM are run enough - where "enough" is defined by the level of write activity to your table
    ```
    SELECT reltuples AS estimate FROM pg_class where relname = 'mytable';
    ```
3. Safer estimate.
  <br>The above ignores the possibility of multiple tables with the same name in one database - in different schemas. To account for that:
    ```
    SELECT c.reltuples::bigint AS estimate
    FROM   pg_class c
    JOIN   pg_namespace n ON n.oid = c.relnamespace
    WHERE  c.relname = 'mytable'
    AND    n.nspname = 'myschema';
    ```
The cast to bigint formats the real number nicely, especially for big counts.

4. Better estimate
<br>Faster, simpler, safer, more elegant. See the manual on [Object Identifier Types](https://www.postgresql.org/docs/current/datatype-oid.html).
    ```
    SELECT reltuples::bigint AS estimate
    FROM   pg_class
    WHERE  oid = 'myschema.mytable'::regclass;
    ```
There is additional info on this approach for partitioned tables

[SO answer](https://stackoverflow.com/a/7945274)
