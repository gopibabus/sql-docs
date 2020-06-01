# 🔥Views

 <img alt="functions" width="300px" src="https://image.flaticon.com/icons/svg/2658/2658219.svg" />

?> In SQL, a **view** is a _virtual table_ based on the result-set of an SQL statement. A view contains rows and columns, just like a real table. The fields in a view are fields from one or more real tables in the database. You can add SQL functions, WHERE, and JOIN statements to a view and present the data as if the data were coming from one single table.

### ✳Creating a view

```sql
CREATE VIEW sales_by_client AS
SELECT
	c.client_id,
    c.name,
    SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING(client_id)
GROUP BY client_id, name;

-- We can select data from a view

SELECT *
FROM sales_by_client
ORDER BY total_sales DESC;

-- One more query

SELECT *
FROM sales_by_client
JOIN clients USING(client_id);
```

### ✳Altering and Dropping a view

?> A **view** can be updated with the **CREATE** OR **REPLACE VIEW** command.

?> A view is deleted with the **DROP VIEW** command.

```sql
DROP VIEW sales_by_client;
```

```sql
CREATE OR REPLACE VIEW sales_by_client AS
SELECT
	c.client_id,
    c.name,
    SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING(client_id)
GROUP BY client_id, name;
```

### ✳Updatable views

In MySQL, views are not only query-able but also updatable. It means that you can use the INSERT or UPDATE statement to insert or update rows of the base table through the updatable view. In addition, you can use DELETE statement to remove rows of the underlying table through the view.

However, to create an updatable view, the SELECT statement that defines the view must not contain any of the following elements:

- Aggregate functions such as MIN, MAX, SUM, AVG, and COUNT.
- DISTINCT
- GROUP BY clause.
- HAVING clause.
- UNION or UNION ALL clause.
- Left join or outer join.
- Subquery in the SELECT clause or in the WHERE clause that refers to the table appeared in the FROM clause.
- Reference to non-updatable view in the FROM clause.
- Reference only to literal values.
- Multiple references to any column of the base table.

```sql
CREATE OR REPLACE VIEW sales_by_client AS
SELECT
	c.client_id,
    c.name
FROM clients c
JOIN invoices i USING(client_id);
```

### ✳The View WITH CHECK OPTION Clause

?>The **WITH CHECK OPTION** clause can be given for an **updatable view** to prevent inserts to rows for which the WHERE clause in the select_statement is not true. It also prevents updates to rows for which the WHERE clause is true but the update would cause it to be not true.

?> In simple words, it prevents visible rows from being updated to nonvisible rows.

```sql
CREATE OR REPLACE VIEW sales_by_client AS
SELECT
	c.client_id,
    c.name
FROM clients c
JOIN invoices i USING(client_id)
WITH CHECK OPTION;
```

### ✳Benefits of Views

- Simplify Queries
- Reduce the impact of changes
- Restrict access to the data.
