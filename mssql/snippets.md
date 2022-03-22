# Microsoft SQL Server Docs

* How to create a database?

```sql
create database TEST;
```

* How to modify the database name?

```sql
-- Using Alter Statement
alter database TEST modify name=TEST1;

-- Using System Stored Procedure
sp_renameDB 'TEST', 'TEST1'
```

* How to delete a database?

>NOTE: MSSql won't allow us to delete database, if it is in use.

```sql
drop database TEST;
```

* How to create a table?

```sql
use Company
Go

create table tableEmployees(
EmployeeID int Primary Key Not Null,
EmployeeName varchar(50) Not Null,
Phone int Not Null UNIQUE,
Age int Not Null CHECK(Age >= 18),
DepID int Foreign Key references tableDepartments(DepID) Not Null
);
```

* How to rename a table?

> We should use MSSql Stored procedure to rename an existing table.

```sql
use Company
Go

EXEC sp_rename 'tableEmployess', 'Employees'
```

* How to delete a table?

```sql
use Company
Go

Drop table tableDepartments;
```

* Data Types

```txt
Int

Bit

Decimal

Datetime

Char

Varchar

Binary
```

* Constraints

```txt
Not Null

Default [value]

Primary Key

Foreign Key

UNIQUE

CHECK
```

* How to add Constraint?

```sql
-- Adding Foreign Key Constraint
Alter Table tblPerson add constraint tblPerson_GenderID_FK
Foreign Key (GenderID) references tblGender (ID)

-- Adding Default Constraint
Alter Table tblPerson
add constraint df_tblPerson_GenderID
Default 3 for GenderID;

-- Adding Unique Constraint
Alter table tableEmployees
Add Constraint UQ_Phone
UNIQUE(Phone)

-- Adding Check Constraint
Alter table tableEmployees
Add Constraint CK_EmployeeAge
CHECK(Age > 0 AND Age < 150)
```

* How to delete a Constraint?

```sql
Alter table tableEmployees
Drop Constraint U_EmployeeAge
```

* How to get Last Generated Identity Column Value?

```sql
-- Last Generated value in a Same Session & Same Scope
Select SCOPE_IDENTITY();

-- Last Generated value in a Same Session & Any Scope
Select @@IDENTITY;

-- Last Generated value in a Any Session & Any Scope
Select INDENT_CURRENT('tableName')
```

How to create a Trigger?

```sql
Create Trigger trForInsert on TestTable1 for Insert
as
Begin
	Insert Into TestTable2 values ('YYY')
End
```

* How to insert data?

```sql
use Company
Go

insert into tableEmployees
	(EmployeeID, EmployeeName, Phone, Age, DepID)
	values(3, 'Dattu', 4111, 26, 2), (4, 'Vamsi', 4589, 27, 2);
```

* What is Cascading Referential Integrity?

> What database should do with the records in the Foreign Relationship Table.

1. **No Action**: Won't allow us to delete row, if we have foreign table Relationship.
2. **Cascade**: Delete rows associated with it in foreign table.
3. **Set NULL**: Set rows value to NULL in foreign Relationship table.
4. **Set Default**: Set rows value to Default Value in foreign Relationship table.

* How to update data in a table?

```sql
use Company
Go

update tableEmployees
set EmployeeName = 'Gopibabu', Phone = 4580
where EmployeeID = 1;
```

* How to delete data in a table?

```sql
use Company
Go

delete tableEmployees
where EmployeeID = 4;
```

* What is Fully Qualified Table Name?

> Table Name with Database Name and Schema name. The benefit of using Fully Qualified table Name is we can use Table Name from any database Context.

```sql
Select * From [AppName].[Customer].[Person]

-- AppName: Database Name
-- Customer: Schema
-- Person: Table Name
```

* How to retrieve data from database?

```sql
# Retrieve all data from the table
select * from tableEmployees;

# Retrieve specific columns
select Age, EmployeeName as Name from tableEmployees;

select employee.Age, employee.EmployeeName as Name
from tableEmployees as employee;

* Retrieve data based on some condition
select *
from Person.Person
where FirstName='Terri'

select *
from Person.Person
where BusinessEntityID > 2

select *
from Person.Person
where FirstName='Terri' AND BusinessEntityID = 2

select *
from Person.Person
where FirstName='Terri' AND BusinessEntityID between 1 and 5

select *
from Person.Person
where FirstName='Terri' AND BusinessEntityID IN (1, 2, 5)
```

* Retrieve data by restricting and sorting

> GROUP BY should be used along with Aggregate functions like SUM, AVG etc.,

> WHERE should not be used with Aggregate Functions but we can use HAVING cluase with Aggregate Functions.

```sql
# Retreive unique PersonType and FirstName Records in the table
select Distinct PersonType from Person.Person;
select Distinct PersonType, FirstName from Person.Person;

# Retrieve top 20 records in the table
select TOP 20 * from Person.Person;

# Order Records
select FirstName, Middlename, LastName
from Person.Person
order by FirstName ASC

select Shelf, sum(Quantity) as Quantity
from Production.ProductInventory
group by Shelf
order by Shelf;

select Shelf, sum(Quantity) as Quantity
from Production.ProductInventory
Where Shelf IN ('A', 'B')
group by Shelf
Having Shelf = 'A'
order by Shelf
```

* Wild Card Operations

```sql
# Firstname starting with letter 'ar'
select * from Person.Person where FirstName Like 'ar%'

# Firstname end with letter 's'
select * from Person.Person where FirstName Like '%s'

# Firstname having 'su' letters
select * from Person.Person where FirstName Like '%su%'

# Firstname having end with 'aron' and having length 5
select * from Person.Person where FirstName Like '_aron'

# Firstname having end with 'aron' and having length 6
select * from Person.Person where FirstName Like '__aron'

# Firstname start with letters "a", "b" or "c"
select * from Person.Person where FirstName Like '[abc]%'

# Firstname start with letters starting from "a" to "c"
select * from Person.Person where FirstName Like '[a-c]%'

# Firstname not start with letters starting from "a" to "c"
select * from Person.Person where FirstName Not Like '[a-c]%'
```

* Case Function

```sql
use Company

Go

Select DepID, DepartmentName,
	Case
		When DepartmentName='IT' Then 'Information technology'
		When DepartmentName='HR' Then 'Human Resources'
		When DepartmentName='FI' Then 'Financial Institution'
		Else 'Other Departments'
	END As 'Long Name'
From tableDepartments;

Select DepID, DepartmentName,
	Case DepartmentName
		When 'IT' Then 'Information technology'
		When 'HR' Then 'Human Resources'
		When 'FI' Then 'Financial Institution'
		Else 'Other Departments'
	END As 'Long Name'
From tableDepartments;
```

* Conversion Functions

```sql
select CONVERT(int,5.45);
select CONVERT(varchar, 5.45);
select convert(datetime, '2021-01-07');

select CAST(5.45 as int);
select CAST(5.45 as varchar);
select CAST('2021-01-07' as datetime)

select FirstName, LastName, CAST(ModifiedDate as varchar) as ModifiedDate from Person.Person;
```

* Retrieving Data from Multiple Tables

> CROSS JOIN produces the Cartesian Product of the 2 tables involved in the JOIN. 10 rows in table 1 and 4 rows in table 2 produces 40 rows in CROSS JOIN result.

```sql
# Inner Join
 select EmployeeID, EmployeeName, DepartmentName
 from tableEmployees te
 Join tableDepartments td
 On te.DepID=td.DepID;

# Left Join
select Name, SalesOrderDetailID
from Production.Product
Left Join Sales.SalesOrderDetail
on Product.ProductID = SalesOrderDetail.ProductID;

# Right Join
select Name, SalesOrderDetailID
from Production.Product
Right Join Sales.SalesOrderDetail
on Product.ProductID = SalesOrderDetail.ProductID;

# Cross Join
select Name, SalesOrderDetailID
from Production.Product
Cross Join Sales.SalesOrderDetail;

# Full Join
select Name, SalesOrderDetailID
from Production.Product
Full Join Sales.SalesOrderDetail
on Product.ProductID = SalesOrderDetail.ProductID;

# Self Join
select P1.Name, P2.Name, P1.ListPrice
From Production.Product P1
Join Production.Product P2
On P1.ListPrice = P2.ListPrice
	And P1.ListPrice <> 0
	And P1.Name <> P2.Name
Order By ListPrice;
```

* Set Operations

> Important difference between UNION and JOIN is UNION combines rows where as JOIN combines columns.

```sql
Use AdventureWorks2019
Go

# UNION: Remove Duplicates
Select CurrencyCode From Sales.CountryRegionCurrency
Union
Select CurrencyCode From Sales.Currency

# UNION ALL: Allow Duplicates
Select CurrencyCode From Sales.CountryRegionCurrency
Union All
Select CurrencyCode From Sales.Currency

# INTERSECT: Include records involve both Male and Female (null values are included)
Select JobTitle
From HumanResources.Employee
Where Gender = 'M'
Intersect
Select JobTitle
From HumanResources.Employee
Where Gender = 'F'

# EXCEPT: Only JobTitles held by Males
Select JobTitle
From HumanResources.Employee
Where Gender = 'M'
Except
Select JobTitle
From HumanResources.Employee
Where Gender = 'F'
```

* Sub Queries

```sql
Use AdventureWorks2019
Go

# Single Row Sub Query
Select COUNT(*) as Count From Sales.SalesOrderDetail Where ProductID = (
	Select ProductID From Production.Product Where Name = 'Cable Lock'
)

# Multiple Row Sub Query
Select * From Production.Product Where ProductID IN (
	Select ProductID From Sales.SalesOrderDetail
)

# Correlated Sub Queries
Select * From Production.Product as P Where Not Exists (
	Select ProductID From Sales.SalesOrderDetail as S
	Where P.ProductID = S.ProductID
)

# SubQuery with insert
Insert Into  Person.StateProvince2
	Select	StateProvinceCode,
			CountryRegionCode,
			IsOnlyStateProvinceFlag,
			Name,
			TerritoryID,
			ModifiedDate
	From Person.StateProvince

# SubQuery with Update
Update Person.StateProvinceTest
Set TerritoryID = 99
Where CountryRegionCode IN (
	Select CountryRegionCode From Person.StateProvince
	Where CountryRegionCode = 'CA'
)

# SubQuery with delete
Delete From Person.StateProvince
Where CountryRegionCode IN (
	Select CountryRegionCode From Person.StateProvince
	Where CountryRegionCode = 'CA'
)
```

* DateTime Functions

```sql
-- Current DateTime of Machine running SQL Server
Select GETDATE()

-- Check Value is Date value or not
Select ISDATE(GETDATE())

-- Get Day of the Month
Select DAY('08-12-2021') as Day, MONTH('08-12-2021') as Month, YEAR('08-12-2021') as Year

-- Get Part of the Date and Returns Integer
Select DATEPART(YEAR, '2021-08-14 14:40:54.270')
Select DATEPART(QUARTER, '2021-08-14 14:40:54.270')
Select DATEPART(MONTH, '2021-08-14 14:40:54.270')
Select DATEPART(DAYOFYEAR, '2021-08-14 14:40:54.270')
Select DATEPART(DAY, '2021-08-14 14:40:54.270')
Select DATEPART(WEEK, '2021-08-14 14:40:54.270')
Select DATEPART(WEEKDAY, '2021-08-14 14:40:54.270')
Select DATEPART(HOUR, '2021-08-14 14:40:54.270')
Select DATEPART(MINUTE, '2021-08-14 14:40:54.270')
Select DATEPART(SECOND, '2021-08-14 14:40:54.270')
Select DATEPART(MILLISECOND, '2021-08-14 14:40:54.270')

-- Get Part of Date or Time and returns String
Select DATENAME(MONTH, '2021-08-14 14:40:54.270')

-- Add Specified Date Time Interval to the Date
Select DATEADD(YEAR, 1, '2021-08-14 14:40:54.270')
Select DATEADD(MONTH, -1, '2021-08-14 14:40:54.270')

-- Get Difference between 2 Dates
Select  DATEDIFF(YEAR, '1992-09-21', '2021-09-21')

-- Convert Date Time into Different Formats
Select	CONVERT(varchar, GETDATE(), 1) as 'Format-1',
		CONVERT(varchar, GETDATE(), 2) as 'Format-2',
		CONVERT(varchar, GETDATE(), 3) as 'Format-3',
		CONVERT(varchar, GETDATE(), 4) as 'Format-4',
		CONVERT(varchar, GETDATE(), 5) as 'Format-5',
		CONVERT(varchar, GETDATE(), 6) as 'Format-6',
		CONVERT(varchar, GETDATE(), 7) as 'Format-7',
		CONVERT(varchar, GETDATE(), 10) as 'Format-10';

Select	CONVERT(varchar, GETDATE(), 8) as 'Format-8',
		CONVERT(varchar, GETDATE(), 14) as 'Format-14',
		CONVERT(varchar, GETDATE(), 24) as 'Format-24',
		CONVERT(varchar, GETDATE(), 108) as 'Format-108';
```

* String Functions

```sql
Select	LEN('This is Gopi') as Length,
		LEFT('This is Gopi', 3) as Left3Chars,
		TRIM('    This is Gopi  ') as TrimSpaces,
		LTRIM('    This is Gopi  ') as LeftTrimSpaces,
		RTRIM('    This is Gopi  ') as RightTrimSpaces,
		LOWER('This is Gopi') as LowerCase,
		UPPER('This is Gopi') as UpperCase,
		REVERSE('This is Gopi') as ReverseString,
		REPLACE('This is Gopi', 'Gopi', 'Harika') as ReplaceString,
		SUBSTRING('This is Gopi', 8, 5) as SubStrings
```

* Math Functions

```sql
Select ABS(-202) as AbsoluteValue;
Select AVG(ListPrice) From Production.Product;
Select COUNT(*) From Production.Product;
Select CEILING(15.75) as CeilValue;
Select FLOOR(15.75) as CeilValue;
Select MAX(ListPrice) From Production.Product;
Select MIN(ListPrice) From Production.Product;
Select POWER(2, 3) as PowerResult;
Select RAND()*100 as RandomNumber;
Select ROUND(123.321234, 3) as RoundedNumber;
Select SQRT(16) as SquareRootOf16;
Select SQUARE(4) as SquareOf4;
Select SUM(ListPrice) From Production.Product;
```

* Transactions

```sql
Begin Tran

Update Person.Person
Set FirstName = 'Tom'
Where BusinessEntityID = 2
Save Tran SaveFirstName

Update Person.Person
Set MiddleName = 'Doe'
Where BusinessEntityID = 2
Save Tran SaveMiddleName

Update Person.Person
Set LastName = 'Harper'
Where BusinessEntityID = 2
Save Tran SaveLastName

Rollback Tran SaveMiddleName
Commit
```

* Schema

> **Schema** is the logical Collection of Tables, Where Permission to Users are applied in single place(on Schema) instead of applying User Permissions on individual tables in the Database.

```sql

-- Create Schema
Create Schema TestSchema
Authorization guest;

-- Create table In Schema
Create Table TestSchema.Departments
(
	ID int Not Null,
	DepratmentName varchar Not Null
)

-- Change Schema Name of the Existing Table
Alter Schema NewSchema
Transfer TestSchema.Departments;

-- Delete Schema
Drop Schema TestSchema;
```

* Views

> Views are Virtual Tables created by user on the result Set of a SQL Statement.

```sql
-- Create View
Create View PersonWithEmailAddress As
Select FirstName, LastName, EmailAddress
From Person.Person As P
Join Person.EmailAddress As E
On P.BusinessEntityID = E.BusinessEntityID

-- Retrieving Data From View
Select * From PersonWithEmailAddress

-- Deleting the View
Drop View PersonWithEmailAddress
```

* Sequence

> Sequence is an Object in SQL Server that is used to generate number Sequence

```sql
-- Create Sequence
Create Sequence SequenceObject
Start With 1
Increment By 1
MinValue 100
MaxValue 2000

-- Next Sequence Value
Select Next Value for SequenceObject;

-- Get Current Sequence Value
Select current_value From sys.sequences
Where name = 'SequenceObject'

-- Restart Sequence Value
Alter Sequence SequenceObject
Restart With 1;

-- Create Decrement Sequence
Create Sequence DecrementSequenceObject
As INT
Start With 100
Increment By -1;

-- Using Sequence Values in Insert Statements
Insert Into HumanResources.Departments
Values(
	Next Value For SequenceObject, 'S'
)

-- Delete Sequence
Drop Sequence DecrementSequenceObject
```

* User Management

1. **Login**: Allows users to use simple Credentials like username and password for accessing SQL Server
2. **Windows Authentication**: Allows users to use Windows Credentials to login to SQL Server.
3. **SQL Authentication**: Create Users using SSMS.

```sql
-- Create Login
Create Login NewLogin With Password = '123456'

-- Rename Login
Alter Login NewLogin with Name = haarikakota
Alter Login haarikakota with Password = 'gopihari595'

-- Delete Login
Drop Login haarikakota;

-- Create User
Create User gopibabus  For  Login gopibabus
```

* Stored Procedure

> **System Defined Stored Procedures** are Procedures that are developed by MSSQL Server team as utilities and they will start with **sp_** and what ever user creates, those are called **Used Defined Stored Procedures**.

```sql
-- Creating Stored Procedure

Create Procedure spGetEmployees
As
Begin
	Select Name, Gender From tblEmployee
End

Create Proc spGetEmployees
As
Begin
	Select Name, Gender From tblEmployee
End

-- Execute Stored Procedure

spGetEmployees

EXEC spGetEmployees

EXECUTE spGetEmployees

-- Creating Stored Procedure with Parameters
Create Procedure spEmployeesByGenderAndDepartment
@Gender nvarchar(20)
@DepartmentId int
As
Begin
	Select Name, Gender, DepartmentId
	From tblEmployee
	Where Gender = @Gender And DepartmentId = @DepartmentId
End

-- Executing Stored Procedure with Parameters
spEmployeesByGenderAndDepartment 'Male', 1

-- Change Stored Procedure
Alter Procedure spGetEmployees
As
Begin
	Select Name, Gender From tblEmployee Order By Name
End

-- Delete Stored Procedure
Drop Procedure spGetEmployees

-- Encrypt Stored Procedure
Alter Procedure spGetEmployees
With Encryption
As
Begin
	Select Name, Gender From tblEmployee
End

-- Stored Procedure with Output Parameters

Create Procedure spEmployeeCountByGender
@Gender nvarchar(20)
@EmployeeCount int output
As
Begin
	Select @EmployeeCount = COUNT(Id)
	From tblEmployee
	Where Gender = @Gender
End

-- Execute Stored Procedure with Output Parameters
Declare @EmployeeTotal int
Execute spEmployeeCountByGender 'Male', @EmployeeTotal Output
Print @EmployeeTotal
if(@EmployeeTotal is null)
	Print '@EmployeeTotal is null'
Else
	Print '@EmployeeTotal is not null'

-- Stored Procedure with return value
Create Procedure spEmployeeCount
as
Begin
	return (Select Count(ID) from Employees)
End

-- Execute Stored Procedure with return value
-- We can use Stored Procedure with return value to return only one integer value.
Declare @EmpCount int
Execute @EmpCount = spEmployeeCount
Select @EmpCount

-- Stored Procedure with Optional Parameters
Create Procedure spSearchEmployee
	@Name nvarchar(50) = NULL,
	@Email nvarchar(50) = NULL,
	@Gender nvarchar(20) = NULL,
	@Age int = NULL
As
Begin
	Select * From tblEmployee
	Where
		(Name = @Name OR @Name IS NULL) AND
		(Gender = @Gender OR @Gender IS NULL) AND
		(Email = @Email OR @Email IS NULL) AND
		(Age = @Age OR @Age IS NULL)
End
```

* System Defined Stored Procedure

```sql
-- Get Information of Database Object
sp_help [procedure or table or view name]

-- Get Text of Stored Procedure
sp_helptext [procedure name]

-- Get table names on which Stored Procedure depends on
sp_depends [procedure name]
```

* Benefits of using Stored Procedures

1. Execution Plan retention and reusability
2. Reduces Network Traffic
3. Code reusability and better maintainability
4. Better Security
5. Avoids SQL Injection Attack

* Error Handling using @@@Error System Function

> @@Error returns a non-zero value, if there is an error. If there is no error it returns zero indicating SQL statement encountered no errors.

> @@Error is cleared and reset on each statement execution.

```sql
-- First Method
Insert Into tblProduct values (2, 'Samsung', 1500, 100)
if(@@Error <> 0)
	Print 'Error Occured'
Else
	Print 'No Errors'
Select * From tblProduct;
if(@@Error <> 0)
	Print 'Error Occured'
Else
	Print 'No Errors'

-- Second Method
Declare @Error int
Insert Into tblProduct values (2, 'Samsung', 1500, 100)
Set @Error = @@Error
Select * From tblProduct;
Set @Error = @@Error

if(@@Error <> 0)
	Print 'Error Occured'
Else
	Print 'No Errors'
```

* Error Handling using Try .. Catch

```sql
Begin Try
	Insert Into tblProduct values (2, 'Samsung', 1500, 100)
	Set @Error = @@Error
	Select * From tblProduct;
End Try
Begin Catch
	Select
			ERROR_NUMBER() as ErrorNumber,
			ERROR_MESSAGE() as ErrorMessage,
			ERROR_PROCEDURE as ErrorProcedure,
			ERROR_STATE() as ErrorState,
			ERROR_SEVERITY() as ErrorSeverity,
			ERROR_LINE() as ErrorLine
End Catch
```
