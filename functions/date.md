# 🔥Date

 <img alt="Date functions" width="200px" src="https://image.flaticon.com/icons/png/512/2972/2972498.png" />

## ⚡Date Functions

### ✳NOW(), CURDATE(), CURTIME()

?> The **NOW()** function returns the current date and time. The **CURDATE()** function returns the current date. The **CURTIME()** function returns the current time.

```sql
SELECT NOW();

SELECT CURDATE();

SELECT CURTIME();
```

### ✳YEAR(), MONTH(), DAY(),

?> The **YEAR()** function returns the year part for a given date. The **MONTH()** function returns the month part for a given date. The **DAY()** function returns the day of the month for a given date.

```sql
SELECT YEAR("2020-06-15");#2020

SELECT MONTH("2017-06-15");#06

SELECT DAY("2017-06-15");#15
```

### ✳HOUR(), MINUTE(), SECOND()

?> The **HOUR()** function returns the hour part for a given date. The **MINUTE()** function returns the minute part of a time/datetime. The **SECOND()** function returns the seconds part of a time/datetime (from 0 to 59).

```sql
SELECT HOUR("2017-06-20 09:34:00");#9

SELECT MINUTE("2017-06-20 09:34:00");#34

SELECT SECOND("2017-06-20 09:34:00.000023");#0
```

### ✳DAYNAME(), MONTHNAME()

?>The **DAYNAME()** function returns the weekday name for a given date. The **MONTHNAME()** function returns the name of the month for a given date.

```sql
SELECT DAYNAME("2017-06-15");#Thursday

SELECT MONTHNAME("2017-06-15");#June
```

### ✳EXTRACT()

?>The **EXTRACT()** function extracts a part from a given date.

```sql
SELECT EXTRACT(YEAR FROM "2017-06-20 09:34:00.0000235");#2017

SELECT EXTRACT(MONTH FROM "2017-06-20 09:34:00.000023");#6

SELECT EXTRACT(DAY FROM "2017-06-20 09:34:00.000023");#20

SELECT EXTRACT(HOUR FROM "2017-06-20 09:34:00.000023");#9

SELECT EXTRACT(MINUTE FROM "2017-06-20 09:34:00.000023");#34

SELECT EXTRACT(SECOND FROM "2017-06-20 09:34:00.000023");#0
```

## ⚡Formatting Dates and Times

### ✳DATE_FORMAT()

?> The **DATE_FORMAT()** function formats a date as specified.

```sql
SELECT DATE_FORMAT("2017-06-15", "%Y");#2017

SELECT DATE_FORMAT("2017-06-15", "%y");#17

SELECT DATE_FORMAT("2017-06-15", "%m %y");#06 17

SELECT DATE_FORMAT("2017-06-15", "%M %Y");#June 17

SELECT DATE_FORMAT("2017-06-15", "%M %d %Y");#June 15 17
```

> [For different DATE_FORMAT() parameter values](https://www.w3schools.com/sql/func_mysql_date_format.asp)

### ✳TIME_FORMAT()

?> The **TIME_FORMAT()** function formats a time by a specified format.

```sql
SELECT TIME_FORMAT("19:30:10", "%H:%i:%s %p");#19:30:10 PM
```

> [For different TIME_FORMAT() parameter values](https://www.w3schools.com/sql/func_mysql_time_format.asp)

## ⚡Calculating Dates & Times

### ✳DATE_ADD(), DATE_SUB()

?> The **DATE_ADD()** function adds a time/date interval to a date and then returns the date. The **DATE_SUB()** function subtracts a time/date interval from a date and then returns the date.

```sql
SELECT DATE_ADD("2017-06-15", INTERVAL 10 DAY);#2017-06-25

SELECT DATE_ADD("2017-06-15", INTERVAL 1 YEAR);#2018-06-25

SELECT DATE_SUB("2017-06-15", INTERVAL 1 YEAR);#2016-06-25
```

### ✳DATEDIFF()

?> The **DATEDIFF()** function returns the number of days between two date values.

```sql
SELECT DATEDIFF("2017-06-25", "2017-06-15");#10
```

### ✳TIME_TO_SEC()

?> The **TIME_TO_SEC()** function converts a time value into seconds.

```sql
SELECT TIME_TO_SEC("19:40:10") - TIME_TO_SEC("19:30:10");#600
```
