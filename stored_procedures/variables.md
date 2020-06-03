# 🔥Variables

?> Mysql supports the concept of **User-defined variables**, which allows passing of a value from one statement to another. A user-defined variable in Mysql is written as @var*name where, var_name is the name of variable and can consist of alphanumeric characters, ., *, and \$.

?> A user-defined variable is **session specific** i.e variable defined by one client is not shared to other client and when the session ends these variables are automatically expired.

```sql
SET @invoices_total = 0;
```

?> **Local variable** can be used in stored procedure, function etc. It is used with the DECLARE keyword. No @ prefix like user-defined variable is needed.

?> The main **difference between local variables and user-defined variable**is that local variable is reinitialized with NULL value each time whenever stored procedure is called while session-specific variable or user-defined variable does not reinitialized with NULL. A user-defined variable set by one user can not be seen by other user.Whatever session variable for a given user is automatically destroyed when user exits.

```sql
DELIMITER $$
CREATE PROCEDURE get_risk_factor()
BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
	DECLARE invoices_total DECIMAL(9, 2);
	DECLARE invoices_count INT;

    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices;

    SET risk_factor = invoices_total / invoices_count * 5;
    SELECT risk_factor;
END$$
DELIMITER ;
```
