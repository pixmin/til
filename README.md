# til

Today I learnt

## Architecture

> A good architecture maximises the number of decisions NOT made.

Uncle Bod Martin - https://www.youtube.com/watch?v=NeXQEJNWO5w

All peripherals stuff (UI, Database, Framework) should be written as plugins to the Use Cases

## Mysql

### Indexing

https://www.youtube.com/watch?v=HubezKbFL7E

>  If you want to learn more about database indexing, I really recommend [https://use-the-index-luke.com](https://use-the-index-luke.com/)/ The entire talk is basically the first few chapters from this site :)

Explain best:

- **Const / Eq_Ref** (B-Tree / Binary search) Can only be used if uniqueness is guaranteed
- **Ref / Range** B-Tree traversal to find starting point, then scans
- **Index** Full Index Scan (no filter)
- **ALL** (avoid at all cost) Full Table Scan

Even with an index on `create_at` Mysql will not use it because we are using a function: YEAR()

```mysql
SELECT SUM(total)
FROM orders
WHERE YEAR(created_at) = 2013
```

Explain will use **ALL** index, no possible keys, no key.

Let's try giving it the precise range:

```mysql
EXPLAIN SELECT SUM(total)
FROM orders
WHERE created_at BETWEEN '2013-01-01 00:00:00' AND '2013-12-31 23:59:59'
```

Now we have possible keys, but no key used.

```mysql
EXPLAIN SELECT SUM(total)
FROM orders FORCE INDEX (orders_created_at_idx)
WHERE created_at BETWEEN '2013-01-01 00:00:00' AND '2013-12-31 23:59:59'
```

`ALL` => `range`

Add index on 'total' > no need to force the index

Explain 'Extra' > `Using index`

`Using index ONLY` is the best way

### Inequality operators

Index stops at the first inequality operator

Ex: `BETWEEN`


#### Multi column indexes

Using from left to right => The column order in a index matters


### Tables size

```sql
SELECT
    TABLE_NAME AS `Table`,
    ROUND((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS `Size (MB)`
FROM
    information_schema.TABLES
WHERE
        TABLE_SCHEMA = "video_encoder"
ORDER BY
    (DATA_LENGTH + INDEX_LENGTH)
    DESC;
```

## ngork

To serve a site from a vagrant using ngrok, from the host:
```
ngrok http -host-header=rewrite local.domain.net:80
```

## salt

To list all managed minions:
```
salt-run manage.status
```

Or list accepted keys:
```
salt-key -L
```
