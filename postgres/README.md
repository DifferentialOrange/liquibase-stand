# Perform PostgreSQL migration with Liquibase

Start a DB instance and perform migration when it's ready.

```bash
docker compose up
```

```
liquibase-1   | UPDATE SUMMARY
liquibase-1   | Run:                          1
liquibase-1   | Previously run:               0
liquibase-1   | Filtered out:                 0
liquibase-1   | -------------------------------
liquibase-1   | Total change sets:            1
liquibase-1   | 
liquibase-1   | Liquibase: Update has been successful. Rows affected: 1
liquibase-1   | Liquibase command 'update' was executed successfully.
liquibase-1 exited with code 0
```

Prepare credentials for `pgsql`.

```bash
export PGPASSFILE=`pwd`/.pgpass
```

Connect to DB and introspect schema.

```bash
PGDATABASE=mydb psql -U myuser --host=localhost
```

```sql
select * from pg_catalog.pg_tables where schemaname='public';
```
```
 schemaname |       tablename       | tableowner | tablespace | hasindexes | hasrules | hastriggers | rowsecurity 
------------+-----------------------+------------+------------+------------+----------+-------------+-------------
 public     | databasechangelog     | myuser     |            | f          | f        | f           | f
 public     | databasechangeloglock | myuser     |            | t          | f        | f           | f
 public     | test_table            | myuser     |            | t          | f        | f           | f
(3 rows)
```
