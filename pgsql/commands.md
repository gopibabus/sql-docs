# 🔥Essential PostgreSQL Commands

?> In postgres we call **database** as **Schema**.

### ❇How to create Schema in Postgres?

```sql
CREATE SCHEMA information;
```

### ❇How to use Schema?

```sql
SET search_path TO information;
```

> There is no difference in syntax for creating tables.

> There is no difference in syntax for truncating tables.

### ❇How to rename a table?

```sql
ALTER TABLE table_name RENAME TO new_table_name;
```

### ❇How to change a column in a table?

```sql
ALTER TABLE table_name ALTER COLUMN standard TYPE character;
```

> There is no difference in syntax for dropping tables.

> There is no difference in syntax in inserting data into table.

> There is no difference in syntax in updating data into table.

> There is no difference in syntax in deleting data from table.

> There is no difference in syntax in retrieving(SELECT) data from table.

> There is no difference in syntax of using **UNION** operator.

> There is no difference in syntax of using **LIKE** operator.

> There is no difference in syntax of using **DISTINCT** keyword.

> There is no difference in syntax of using **COUNT()** function.

> There is no difference in syntax of using **WHERE** and **GROUP BY** keywords.

> There is no difference in syntax of using **OFFSET** and **LIMIT** keywords.

> There is no difference in syntax of using **LEFT JOIN**, **RIGHT JOIN**, **JOIN** keywords.

> In PostgreSQL we replace **FULL OUTER JOIN** with **FULL JOIN**.

> There is no difference in syntax of creating **VIEWS**.

> In PostgreSQL we call **Stored Procedures** as **Procedures**. Syntax is similar for creating stored procedures.

> Syntax is similar for creating **TRIGGERS**.

> There is no difference in syntax of using **GRANT** and **REVOKE** keywords.

> Syntax is similar for creating **TRANSACTIONS**.

?> 💡 Main advantange of using **PostgreSQL** over **MySQL** is postgreSQL **supports extensions** like UUID etc.,. And we can create our own extensions.