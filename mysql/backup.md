# 🔥MySQL Backup & Restore

## ⚡️Mysqldump Backup

?> Typical way of exported a single database

```bash
mysqldump some_database > some_database.sql

# Or with user auth
mysqldump -u some_user -p some_database > some_database.sql

# Or with gzip compression
mysqldump some_database | gzip > some_database.sql.gz

# Or with the "pv" tool, which let's us know how much data is
# flowing between our pipes - useful for knowing if the msyqldump
# has stalled
mysqldump some_database | pv | gzip > some_database.sql.gz
# 102kB 0:01:23 [1.38MB/s] [  <=>
```

?> Assuming you are using InnoDB tables, your mysqldump should look something like this:

```bash
mysqldump --single-transaction --skip-lock-tables some_database > some_database.sql
```

?> Dump multiple databases:

```bash
mysqldump --single-transaction --skip-lock-tables --databases db1 db2 db3 \
    > db1_db2_and_db3.sql
```

?> Dump specific tables from a single database:

```bash
mysqldump --single-transaction --skip-lock-tables some_database table_one table_two table_three \
    > some_database_only_three_tables.sql
```

?> Dump the entire database

```bash
mysqldump --single-transaction --skip-lock-tables --flush-privileges --all-databases > entire_database_server.sql
```

## ⚡️Mysqldump Restore

?> Restore a single Database

```bash
mysql -u username -p [database_name] < databasename.sql
```

?> Restore all databases

```bash
mysql -u username -p < alldatabases.sql
```

[🌍 Reference](https://serversforhackers.com/c/mysqldump-with-modern-mysql)

[XtraBackup Software](https://www.percona.com/software/mysql-database/percona-xtrabackup)

[⏯ Video Reference](https://www.youtube.com/watch?v=vIIRuDtG1yI)
