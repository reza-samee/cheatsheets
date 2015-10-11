Magic words:
```bash
psql -U postgres
```
If run with `-E` flag, it will describe the underlaying queries of the `\` commands (cool for learning!).

Most `\d` commands support additional param of `__schema__.name__` and accept wildcards like `*.*`

- `\q`: Quit/Exit
- `\c __database__`: Connect to a database
- `\d __table__`: Show table definition including triggers
- `\dt *.*`: List tables from all schemas (if `*.*` is omitted will only show SEARCH_PATH ones)
- `\l`: List databases
- `\dn`: List schemas
- `\df`: List functions
- `\dv`: List views
- `\df+ __function` : Show function SQL code. `\x` before pretty-formats it

Casting:
- `CAST (column AS type)` or `column::type`
- `'__table_name__'::regclass::oid`: Get oid having a table name

SQL queries:
- `SELECT * FROM pg_proc WHERE proname='__procedurename__'`: List procedure/function
- `SELECT * FROM pg_views WHERE viewname='__viewname__';`: List view (including the definition)
- `SELECT pg_size_pretty(pg_total_relation_size('__table_name__'));`: Show DB table space in use
- `SELECT pg_size_pretty(pg_database_size('__database_name__'));`: Show DB space in use
- `show statement_timeout;`: Show current user's statement timeout
- `SELECT pid, datname, waiting, state, query FROM pg_stat_activity WHERE datname='__database_name__';`: Show queries being executed at a certain DB. Can also display query time, etc.
- `SELECT * FROM pg_indexes WHERE tablename='__table_name__' AND schemaname='__schema_name__';`: Show table indexes
- Get all indexes from all tables of a schema:
```sql
select
   t.relname as table_name,
   i.relname as index_name,
   a.attname as column_name
from
   pg_class t,
   pg_class i,
   pg_index ix,
   pg_attribute a,
    pg_namespace n
where
   t.oid = ix.indrelid
   and i.oid = ix.indexrelid
   and a.attrelid = t.oid
   and a.attnum = ANY(ix.indkey)
   and t.relnamespace = n.oid
    and n.nspname = 'kartones'
order by
   t.relname,
   i.relname
```