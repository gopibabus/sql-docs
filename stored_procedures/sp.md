# 🔥Stored Procedures

<img alt="stored procedures" width="700px" src="/assets/images/stored_procedures.png" />

?> A **stored procedure** is a prepared SQL code that you can save, so the code can be reused over and over again. So if you have an SQL query that you write over and over again, save it as a stored procedure, and then just call it to execute it. You can also pass parameters to a stored procedure, so that the stored procedure can act based on the parameter value(s) that is passed.

Benefits of Stored Procedures:

1. Store and Organize SQL.
2. Faster Execution.
3. Data Security

### ✳Creating a Stored Procedure

```sql
DELIMITER $$# We are changinf delimiter to $$
CREATE PROCEDURE get_clients()
BEGIN
	SELECT * FROM clients;
END$$
DELIMITER ; #Restoring delimiter to ;

-- call stored procedure

CALL get_clients();
```

### ✳Create Stored Procedure-Workbench

> [Reference](https://www.mysqltutorial.org/getting-started-with-mysql-stored-procedures.aspx/)

### ✳Dropping Stored Procedures

```sql
DROP PROCEDURE IF EXISTS get_clients;
```

```sql
DROP PROCEDURE IF EXISTS get_clients;

DELIMITER $$# We are changinf delimiter to $$
CREATE PROCEDURE get_clients()
BEGIN
	SELECT * FROM clients;
END$$
DELIMITER ; #Restoring delimiter to ;
```
