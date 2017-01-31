#SQL Intro
> SQL: standard language for accessing and manipulating databases
> Structures Query Language
> an American National Standards Institute(ANSI) standard

## SQL can...
>- execute queries in DB
>- retrieve data from DB
>- insert/update/delete records
>- create new DB, new tables in DB, stored procedures, and views in DB
>- set permissions on tables, procedures, and views


>> view: how information is portrayed in DB

###Using SQL requires
> - RDBMS db program (Access, MySQL..)
> - usage of a server-side scripting languague, like PHP or ASP
> - usage of SQL to get the data you want
> - usage of HTML/CSS

##RDBMS
**: Relational Database Management System**
> - basis for SQL and for all DB systems
> - its data is stored in tables
> 
>> table : a collection of related data entries and consists of columns and rows

#SQL Syntax
##Database Tables
> DB has often one or more tables
> 
> table is identified by a name (ex: "Orders", "Categories")

###SELECT * FROM table_name (all the records in a table):
`SELECT * FROM table_name;`
>NOT case sensitive `select` = `SELECT`

>`*` : selects "all"

#####other SQL commands
>
SELECT|extracts data
---|---
UPDATE| update data
DELETE| delete
INSERT INTO|insert data into DB
CREATE DATABASE|creates DB
ALTER DATABASE|modifies DB
CREATE TABLE|create table
ALTER TABLE|modifies table
DROP TALBE|delete table
CREATE INDEX|create index(search key)
DROP INDEX|delete index

###Select Columns of a table:
`SELECT column_name1, column_name2 FROM table_name;`
>extracts certain columns from a table

###SELECT DISTINCT data from columns of a table
>does not show repeated data

`SELECT DISTINCT column_name1, column_name2 FROM table_name;`

###SELECT _ FROM _ WHERE col_name _operator_ # or ' ';

operator|description
---|---
=|equal
< >|not equal
>|greater than
<|less than
>=|
><=|
BETWEEN|between an inclusive range
LIKE|search for pattern
IN|specify muliple possible values for a column

```
SELECT column_names(*) FROM table_name 
WHERE column_name = (other operators) # (or 'text'); 
```

>extracts data that meets specific field from columns selected of a table
####

###SELECT _ FROM _ WHERE col_name = # or ' '
```SQL
SELECT City, PostalCode FROM [Customers]
WHERE Country = 'Germany';
```
###SELECT WHERE ~ AND/OR 
> and: satisfies both conditions
> 
> or: satisfies either one or the other

```SQL
SELECT Unit FROM [Products]
where price = 18 and ProductName IN ('Chais','Chang')

SELECT LastName, FirstName FROM [Employees]
where EmployeeID >3 or FirstName = 'Nancy';

```
###SELECT _ FROM _ ORDER BY_ ASC/DESC
>sort the result-set by one or more columns

 ```sql
 SELECT column_name FROM table_name
 ORDER BY column_name ASC(DESC);
 ```

```sql
SELECT * 						#select all data 
FROM [OrderDetails]			#from orderdetail_table
ORDER BY ProductID ASC, Quantity DESC		#sort by productID ascending, quanity descending
```

###INSERT INTO table_name ( col _name ) VALUES ()

```sql
INSERT INTO table_name
VALUES (value1,value2,value3,...);
```

```sql
INSERT INTO [Categories] (CategoryID, CategoryName, Description)
VALUES (35, 'Detergents', 'liquids detergents, soaps');
```
###UPDATE tb\_name SET col\_name# = value WHERE some_col = value
>change existing records in table
>
>MUST USE WHERE, otherwise change all data in corresponding columns

```sql
UPDATE table_name
SET col1 = val1, col2 = val2, ...
WHERE column_name = value 			#indicates specific col and data to update certain row 
```

```sql
UPDATE Customers
SET ContactName = 'Young Cho'
WHERE CustomerID = 95 
```

###DELETE FROM _ WHERE col\_value = some\_value
```sql
DELETE FROM table_name
WHERE column_name = text/#_value;
```

```sql
DELETE FROM [Employees]
WHERE FirstName = 'Nancy';
```
!!!
`Delete From tb_name` and no WHERE statement => deletes all table data

##SQL INJECTION
> inject SQL commands into an SQL statements via web page input
> 
> can alter SQL statement and compromise the security of web app

###Injection based on 1=1 is always true
> wrong input을 방지하는것이 없을때...항상 참인 값을 입력하여 정보에 접근 할 수 있다. 

ex) 
userid: `105 or 1=1`

- server result: `SELECT * FROM Users WHERE USERID = 105 or 1=1`

>this allows one to access all user data

###Parameters for Protection
> To protect from injection attacks, set SQL parameters
> >parameter: values added to an SQL query at execution time

```sql
txtSQL = "INSERT INTO Customers (CustomerName,Address,City) Values(@0,@1,@2)";
```

###SELECT TOP _  percent _ FROM 
>selects top #% records(can only retrieve from certain cols) from a table

```sql
SELECT TOP number | percent col_name
FROM table_name
```
```sql
SELECT top 20 PERCENT OrderID FROM OrderDetails;
```

> MySQL code is slighly different

```MySQL
SELECT col_name
FROM ta_name
LIMIT number;


SELECT *
FROM Customers
Limit 5;
```

###SELECT _ FROM _ WHERE _ LIKE pattern;
> selects records that meets certain pattern
> 
> can select records that has a certain letter or contains certain word...

```sql
SELECT col_name(s)
FROM tb_name
WHERE col_name LIKE pattern;
```
```sql
SELECT * FROM [Employees]
where LastName like '%o%'		#to select records that has a letter 'o' in its lastname
```

####SELECT wildcard using underscore
>wildcard character is used to substitue for any other character in a string

wildcard|description
---|---
%| subs for zero or more chars
_| subs for a single char
[chars]| set and ranges of chars to match
[^char] or [!char]| matches only a char not in brackets

```sql
SELECT * FROM Customers
WHERE City LIKE '_ortland';			#selects a city that ends with'ortland'

SELECT * FROM Customers
WHERE City LIKE '[a-c]%';			#selects a city starting with 'a','b','c'
```

####SELECT _ FROM _ WHERE cols IN (values...)
> selects data based on multiple values

```sql
SELECT col_name(s)
FROM tb_name
WHERE col_name IN (value1,value2,...);
```

```sql
SELECT * FROM [Categories]
where CategoryName In ('Beverages', 'Condiments');

#selects records which categoryname has 'beverages' or 'condiments'
```
####SELECT _ FROM _ WHERE cols BETWEEN val1 AND val2
> selects data that is in a range
```sql
SELECT col_name(s)
FROM tb_name
WHERE col_name BETWEEN val1 AND val2;
```

```sql
SELECT * FROM [Customers]
where CustomerID between 40 and 90			# selects 40~90 customer id
```
###Aliases 
> gives a table or a column in a table a temporary name

```sql
SELECT col_name(s)
FROM tb_name AS alias_name;
```

```sql
SELECT OrderID As Orders FROM [Orders]

#below codes create an alias name "Address" that has data combined from four cols

SELECT CustomerName, Address+', '+City+', '+PostalCode+', '+Country AS Address
FROM Customers;

SELECT CustomerName, CONCAT(Address,', ',City,', ',PostalCode,', ',Country) AS Address
FROM Customers;

```

###SQL JOINS
> combine rows from two or more tables

>simple inner join returns all rows where there is at least one match in **both** tables

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID=Customers.CustomerID;
```

>LEFT JOIN: returns all rows from left table, and the matched rows from right

```sql
SELECT col_name(s)
FROM table1
LEFT JOIN table2
ON tb1.col_name=tb2.col_name;

SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

>RIGHT JOIN: returns all rows from the right and matched rows form left

```sql
SELECT col_name(s)
FROM table1
RIGHT JOIN table2
ON tb1.col_name=tb2.col_name;

SELECT Orders.OrderID, Employees.FirstName
FROM Orders
RIGHT JOIN Employees
ON Orders.EmployeeID=Employees.EmployeeID
ORDER BY Orders.OrderID;
```

>FULL JOIN: returns all rows when there is a match in **one of tables**

```sql
SELECT col_name(s)
FROM table1
FULL OUTER JOIN table2
ON tb1.col_name=tb2.col_name;

SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

###Select UNION
> combines the result of two or more select statements

```sql
SELECT col_name(s) FROM tb1
UNION (ALL  # allow duplicate values)
SELECT col_name(s) FROM tb2;
```

```sql
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
ORDER BY City;
```
###SELECT INO
>creates a copy (maybe for backup...) from one table into another

```sql
SELECT *
INTO newtable [IN externaldb]
FROM table1;
```

```sql
SELECT *
INTO OD_01 IN 'Backup.mdb'
FROM OrderDetails;
#copy from orderdetails to new table 'OD_01'
```

###INSERT into
>copy from one table to existing table

```sql
INSERT INTO table2
(column_name(s))
SELECT column_name(s)
FROM table1;
```
##CREATE DATABASE db_name; and TABLE
- `CREATE DATABASE dbname;` creates database

- creates a table : data_type: (varchar, integer, decimal, date, etc.)

	```sql
	CREATE TABLE table_name
	(
	column_name1 data_type(size),
	column_name2 data_type(size),
	column_name3 data_type(size),
	....
	);
	```
##CONSTRAINTS
> creates a table with constraints

	```sql
	CREATE TABLE table_name
	(
	column_name1 data_type(size) constraint_name,
	column_name2 data_type(size) constraint_name,
	column_name3 data_type(size) constraint_name,
	....
	);
	
	```

###NOT NULL
> does not accept Null values, must input something...
	
```sql
CREATE TABLE PersonsNotNull
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)

```
	
###UNIQUE
> uniquely identifies each record in a table (primary key also guantees for uniqueness but it automatically has a unique constraint defined on it)
	
```sql
CREATE TABLE Persons
(
P_Id int NOT NULL UNIQUE,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
###PRIMARY KEY
>- uniquely identifies each record in DB
>- must contain unique values
>- cant contain Null

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
PRIMARY KEY (P_Id)
)
```
>to add primary key, when altering a table...

```sql
ALTER TABLE Persons
ADD PRIMARY KEY (P_Id)
```

###FOREIGN KEY
> foreign key in one table points to a primary key in another

ex) P_Id, a primary key in "Persons" becomes a foreign key in a table 'Orders'

```sql
CREATE TABLE Orders
(
O_Id int NOT NULL PRIMARY KEY,
OrderNo int NOT NULL,
P_Id int FOREIGN KEY REFERENCES Persons(P_Id)
)
```
###CHECK
>limit the range of values that can be placed in a column

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL CHECK (P_Id>0),
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```

>when alterging:

```sql
ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
```
###DEFAULT
> insert a default value into a column
> 
> default values will be added to all new records if no other value is specified

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255) DEFAULT 'Sandnes'
)
```

>when altering:
	
```sql
ALTER TABLE Persons
ALTER COLUMN City SET DEFAULT 'SANDNES'
```

##CREATE Index
>create indexes that allow DB application to find data fast 
>
>users cannot see indexes

```sql
CREATE (UNIQUE) INDEX index_name
ON tb_name (col_name)
```

```sql
#create an index, 'PIndex' on combi of cols, last name and first mane
CREATE INDEX PIndex
ON Persons (LastName, FirstName)
```

###DROP INDEX, TABLE, DB
> delete an index in a table, delete a table, delete a DB

```sql
DROP INDEX index_name ON table_name		#Access
DROP INDEX table_name.index_name		#mySQL


DROP TABLE table_name

DROP DATABASE database_name

#does not delete whole table but data inside
TRUNCATE TABLE table_name

```
##Alter TABLE
>- ADD

```sql
ALTER TABLE table_name
ADD column_name datatype
```

>- DROP

```sql
ALTER TABLE table_name
DROP COLUMN column_name
```
>- Change data (alter...)

```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype

```
#SQL Functions

Function|what it does
---|---
AVG()|return average
COUNT()|count the number of rows
FIRST()|return first val
LAST()|return last val
MAX()|return max val
MIN()|return min val
SUM()|return the sum
UCASE()|convert to uppercase
LCASE()|convert to lowercase
MID()|extract characters from a text
LEN()|return length of text
ROUND()|rounds numeric field to number of decimals
NOW()|return current data and time
FORMAT()|format for display

###AVG()
>selecting col as average values

`SELECT AVG(column_name) FROM table_name`

>selecting values with where statement using where

```sql
SELECT ProductName, Price FROM Products
WHERE Price>(SELECT AVG(Price) FROM Products);
```

###MID()
>selecting certain length of texts 
> start: from 1

`SELECT MID(column_name,start,length) AS some_name FROM table_name;
`

> selects only 4 charcters of cities from Customer tb

```sql
SELECT MID(City,1,4) AS ShortCity
FROM Customers;
```
