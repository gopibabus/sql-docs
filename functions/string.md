## ⚡String Functions

### ✳LENGTH()

?> The **LENGTH()** function returns the length of a string (in bytes).

```sql
SELECT LENGTH("SQL Tutorial");#12
```

### ✳UPPER() & LOWER()

?> The **UPPER()** function converts a string to upper-case. The **LOWER()** function converts a string to lower-case.

```sql
SELECT UPPER('GopiBabu');#GOPIBABU

SELECT LOWER('GopiBabu');#gopibabu
```

### ✳LTRIM(), RTRIM() & TRIM()

?> The **LTRIM()** function removes leading spaces from a string. The **RTRIM()** function removes trailing spaces from a string. The **TRIM()** function removes the space character OR other specified characters from the start or end of a string.

```sql
SELECT LTRIM('     SQL Tutorial');#SQL Tutorial

SELECT RTRIM('SQL Tutorial     ');#SQL Tutorial

SELECT TRIM('     SQL Tutorial!     ');#SQL Tutorial
```

### ✳LEFT(), RIGHT()

?> The **LEFT()** function extracts a number of characters from a string (starting from left). The **RIGHT()** function extracts a number of characters from a string (starting from right).

```sql
SELECT LEFT('SQL Tutorial', 3);#SQL

SELECT RIGHT('SQL Tutorial', 3);#ial
```

### ✳SUBSTRING()

?> The **SUBSTRING()** function extracts some characters from a string.

```sql
SELECT SUBSTRING('SQL Tutorial', 1, 3);#SQL

SELECT SUBSTRING('SQL Tutorial', 5);#Tutorial
```

### ✳LOCATE()

?> The **LOCATE()** function returns the position of the first occurrence of a substring in a string. If the substring is not found within the original string, this function returns 0. This function performs a case-insensitive search.

```sql
SELECT LOCATE("3", "W3Schools.com");#2
```

### ✳REPLACE()

?> The **REPLACE()** function replaces all occurrences of a substring within a string, with a new substring. This function performs a case-sensitive replacement.

```sql
SELECT REPLACE("SQL Tutorial", "SQL", "HTML");#HTML Tutorial
```

### ✳CONCAT()

?> The **CONCAT()** function adds two or more expressions together.

```sql
SELECT CONCAT("SQL ", "Tutorial ", "is ", "fun!");#SQL Tutorial is fun
```
