# til

Today I learnt

## mysql

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
