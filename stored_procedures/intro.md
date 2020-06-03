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

### ✳Parameters

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	SELECT * FROM clients c
    WHERE c.state = state;
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
```

```sql
DROP PROCEDURE IF EXISTS get_invoices_by_client;

DELIMITER $$
CREATE PROCEDURE get_invoices_by_client(client_id INT)
BEGIN
	SELECT * FROM invoices i
    WHERE i.client_id = client_id;
END$$
DELIMITER ;

CALL get_invoices_by_client(1);
```

### ✳Parameters with default values

?> Version 1

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	IF state IS NULL THEN
		SET state = 'CA';
	END IF;

	SELECT * FROM clients c
    WHERE c.state = state;
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
CALL get_clients_by_state(NULL);
```

?> Version 2

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	IF state IS NULL THEN
		SELECT * FROM clients;
	ELSE
		SELECT * FROM clients c
		WHERE c.state = state;
	END IF;
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
CALL get_clients_by_state(NULL);
```

?> Version3

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	SELECT * FROM clients c
	WHERE c.state = IFNULL(state, c.state);
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
CALL get_clients_by_state(NULL);
```

```sql
DROP PROCEDURE IF EXISTS get_payments;

DELIMITER $$
CREATE PROCEDURE get_payments(client_id INT, payment_method_id TINYINT)
BEGIN
	SELECT * FROM payments p
	WHERE
		p.client_id = IFNULL(client_id, p.client_id)
		AND
		p.payment_method = IFNULL(payment_method_id, p.payment_method);
END$$
DELIMITER ;

CALL get_payments(5, 2);
CALL get_payments(NULL, NULL);
```

### ✳Parameter Validation

?> Before Validation

```sql
DROP PROCEDURE IF EXISTS make_payment;

DELIMITER $$
CREATE PROCEDURE make_payment
	(
		invoice_id INT,
        payment_amount DECIMAL(9, 2),
        payment_date DATE
	)
BEGIN
	UPDATE invoices i
	SET
		i.payment_total = payment_amount,
		i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;
END$$
DELIMITER ;

CALL make_payment(2, 100, '2019-01-01');
```

?> After adding Validation

```sql
DROP PROCEDURE IF EXISTS make_payment;

DELIMITER $$
CREATE PROCEDURE make_payment
	(
		invoice_id INT,
        payment_amount DECIMAL(9, 2),
        payment_date DATE
	)
BEGIN
	IF payment_amount <= 0 THEN
		SIGNAL SQLSTATE '22003'
			SET MESSAGE_TEXT = 'Invalid payment amount';
	END IF;

	UPDATE invoices i
	SET
		i.payment_total = payment_amount,
		i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;
END$$
DELIMITER ;

CALL make_payment(2, -100, '2019-01-01');#Throws Error code 22003
```

> [🌐 SQLSTATE common error codes](https://www.ibm.com/support/knowledgecenter/en/SSEPEK_11.0.0/codes/src/tpc/db2z_sqlstatevalues.html)

### ✳Output Parameters

```sql
DROP PROCEDURE IF EXISTS get_unpaid_invoices_for_client;

DELIMITER $$
CREATE PROCEDURE get_unpaid_invoices_for_client
	(
		client_id INT,
        OUT invoices_count INT,
        OUT invoices_total DECIMAL(9, 2)
	)
BEGIN
	SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id
		AND payment_total = 0;
END$$
DELIMITER ;

SET @invoices_count = 0;
SET @invoices_total = 0;
CALL get_unpaid_invoices_for_client(3, @invoices_count, @invoices_total);
SELECT @invoices_count, @invoices_total;
```

!> Better to avoid output parameters, because it will cause so much confusion and it is hard to use.

### ✳Variables

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

### ✳Functions

?> A **function/stored function** is a special kind stored program that returns a single value. Typically, you use stored functions to encapsulate common formulas or business rules that are reusable among SQL statements or stored programs.

?> Different from a stored procedure, you can use a stored function in SQL statements wherever an expression is used. This helps improve the readability and maintainability of the procedural code.

```sql
DELIMITER $$
CREATE FUNCTION get_risk_factor_for_client(client_id INT)
RETURNS INTEGER
READS SQL DATA
MODIFIES SQL DATA
BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;

    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id;

    SET risk_factor = invoices_total / invoices_count * 5;
	RETURN IFNULL(risk_factor, 0);
END$$
DELIMITER ;

DROP FUNCTION IF EXISTS get_risk_factor_for_client;

-- How to use functions

SELECT
	client_id,
    name,
    get_risk_factor_for_client(client_id) AS risk_factor
FROM clients;
```
