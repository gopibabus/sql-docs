# 🔥Essential MySQL Functions

 <img alt="functions" width="200px" src="https://image.flaticon.com/icons/png/512/2628/2628251.png" />

## ⚡Numeric Functions

### ✳ROUND()

?> The **ROUND()** function rounds a number to a specified number of decimal places.

```sql
SELECT ROUND(5.73);#6
SELECT ROUND(5.7342, 2);#5.73
```

### ✳TRUNCATE()

?> The **TRUNCATE()** function truncates a number to the specified number of decimal places.

```sql
SELECT TRUNCATE(135.375, 2);#135.37
```

### ✳CEILING()

?> The **CEILING()** function returns the smallest integer value that is larger than or equal to a number.

```sql
SELECT CEILING(25.75);#26
```

### ✳FLOOR()

?> The **FLOOR()** function returns the largest integer value that is smaller than or equal to a number.

```sql
SELECT FLOOR(25.75);#25
```

### ✳ABS()

?> The **ABS()** function returns the absolute value of a number.

```sql
SELECT Abs(-243.5);#243.5
```

### ✳RAND()

?> The **RAND()** function returns a random number between 0 (inclusive) and 1 (exclusive).

```sql
SELECT RAND();
```
