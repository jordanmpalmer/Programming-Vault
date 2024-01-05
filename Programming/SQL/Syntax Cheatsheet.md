## Database

It is defined as a collection of interrelated data stored together to serve multiple applications.

## MySQL Elements

MySQL has certain elements that play an important role in querying a database.

### Literals

Literals refer to a fixed data value

```sql
17 #It is a numeric literal
"Harry" #It is a text literal
12.5 #It is a real literal
```

Copy

### Data Types

Data types are means to identify the type of data.

```sql
#Numeric

INT -- Integer data type
TINYINT
SMALLINT
MEDIUMINT
BIGINT

FLOAT(M,D) -- Floating point data type
DOUBLE(M,D) -- Double data type also stores decimal values
DECIMAL(M,D) -- Decimal data type
```

Copy

```sql
#Data and Time 

DATE -- Date data type (YYYY-MM-DD)
DATETIME -- It's a date and time combination (YYYY-MM-DD HH:MM:SS)
TIME -- It stores time (HH:MM:SS)
```

Copy

```sql
#String/Text 

CHAR(M) -- Character data type
VARCHAR(M) -- Variable character data type
BLOB or TEXT
```

Copy

### NULL Values

If a column has no value, then it is said to be NULL

### Comments

A comment is a text that is not executed.

```sql
/* This is a multi-line
comment in MySQL */

# It is a single-line commend

-- It is also a single-line comment
```

Copy

## MySQL Simple Calculations

You can perform simple calculations in MySQL, just by using the Select command, there's no need to select any particular database to perform these commands.

### Addition

It will add two numbers

```sql
Select 5+8;
```

Copy

### Subtraction

It will subtract the second number from first

```sql
Select 15-5;
```

Copy

### Multiplication

It will give the product of supplied numbers

```sql
Select 5*5;
```

Copy

### Division

It will divide the number

```sql
Select 24/4;
```

Copy

```sql
-- SQL is not a case-sensitive language
```

Copy

## Accessing Database

These commands allow one to check all the databases and tables

### Show command

It will show all the databases in the system

```sql
Show databases;
```

Copy

It will show all the tables in a selected database

```markup
show tables;
```

Copy

### Use command

It will start using the specified database i.e. now you can create tables in the selected database

```sql
use database_name;
```

Copy

## Creating tables

These commands allow you to create the table in MySQL

### Create table command

This query is used to create a table in the selected database

```sql
Create table <table-name>
(<column_name> <data_type>,
<column_name> <data_type>,
<column_name> <data_type>);
```

Copy

### Insert command

It will add data into the selected table

```sql
Insert into <table_name> [<column-list>]
Values (<value1>,<value2>...);
```

Copy

### Inserting NULL values

This query will add NULL value in the col3 of the selected table

```sql
Inset into <table-name> (col1, col2,col3) 
Values (val1,val2,NULL);
```

Copy

### Inserting Dates

It will add the following data into the selected column of the table

```sql
Insert into <table_name> (<col_name>) 
Values ('2021-12-10');
```

Copy

## Select Command

A select query is used to fetch the data from the database

### Selecting All Data

It will retrieve all the data of the selected table

```sql
Select * From <table_name>;
```

Copy

### Selecting Particular Rows

It will retrieve all the data of the row that will satisfy the condition

```sql
Select * from <table_name>
Where <condition_to_satisfy>;
```

Copy

### Selecting Particular Columns

It will retrieve data of selected columns that will satisfy the condition

```sql
Select column1, column2 from <table_name>
Where <condition_to_satisfy>;
```

Copy

### DISTINCT Keyword

It will retrieve only distinct data i.e. duplicate data rows will get eliminated

```sql
Select DISTINCT <column_name> from <table_name>;
```

Copy

### ALL Keyword

It will retrieve all the data of the selected column

```sql
Select ALL <column_name> from <table_name>;
```

Copy

### Column Aliases

It is used to give a temporary name to a table or a column in a table for the purpose of a particular query

```sql
Select <column1>,<column2> AS <new_name>
From <table_name>;
```

Copy

### Condition Based on a Range

It will only retrieve data of those columns whose values will fall between value1 and value2 (both inclusive)

```sql
Select <co11>, <col2> 
From <table_name>
Where <value1> Between <value2>;
```

Copy

### Condition Based on a List

```sql
Select * from <table_name> 
Where <column_name> IN (<val1>,<val2>,<val3>);
```

Copy

```sql
"Select * from <table_name> 
Where <column_name> NOT IN (<val1>,<val2>,<val3>);"
```

Copy

### Condition Based on Pattern Match

```sql
Select <col1>,<col2> 
From <table_name>
Where <column> LIKE 'Ha%';
```

Copy

```sql
Select <col1>,<col2> 
From <table_name>
Where <column> LIKE 'Ha__y%';
```

Copy

### Searching NULL

It returns data that contains a NULL value in them

```sql
Select <column1>, <column2>
From <table_name> Where <Val> IS NULL;
```

Copy

## SQL Constraints

SQL constraints are the rules or checks enforced on the data columns of a table

### NOT NULL

It will create a table with NOT NULL constraint to its first column

```sql
Create table <table_name>
( <col1> <data_type> NOT NULL,
<col2> <data_type>,
<col3> <data_type>);
```

Copy

### DEFAULT

DEFAULT constraint provides a default value to a column

```sql
Create table <table_name>
( <col1> <data_type> DEFAULT 50,
<col2> <data_type>,
<col3> <data_type>);
```

Copy

### UNIQUE

UNIQUE constraint ensures that all values in the column are different

```sql
Create table <table_name>
( <col1> <data_type> UNIQUE,
<col2> <data_type>,
<col3> <data_type>);
```

Copy

### CHECK

CHECK constraint ensures that all values in a column satisfy certain conditions

```sql
Create table <table_name>
( <col1> <data_type> CHECK (condition),
<col2> <data_type>,
<col3> <data_type>);
```

Copy

### Primary Key

Primary key is used to uniquely identify each row in a table

```sql
Create table <table_name>
( <col1> <data_type> Primary Key,
<col2> <data_type>,
<col3> <data_type>);
```

Copy

### Foreign Key

```sql
CREATE TABLE Orders (
OrderID int NOT NULL,
OrderNumber int NOT NULL,
PersonID int,
PRIMARY KEY (OrderID),
FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

Copy

## Viewing Table Structure

### Desc or Describe command

It allows you to see the table structure

```sql
Desc <table_name>;
```

Copy

## Modifying Data

### Update Command

It will update the values of selected columns

```sql
Update <table_name>
SET <col1> = <new_value>, <col2> = <new_value>
Where <condition>;
```

Copy

## Deleting Data

### Delete Command

It will delete the entire row that will satisfy the condition

```sql
Delete From <table_name>
Where <condition>;
```

Copy

## Ordering Records

Order by clause is used to sort the data in ascending or descending order of specified column

### order by clause

It will return records in the ascending order of the specified column name's data

```sql
Select * from <table_name> order by <column_name>;
```

Copy

It will return records in the descending order of the specified column name's data

```sql
Select * from <table_name> order by <column_name> DESC;
```

Copy

### Ordering data on multiple columns

It will return records in the ascending order of column1 and descending order of column2

```sql
Select * From <table_name> order by <column1> ASC, <column2> DESC;
```

Copy

## Grouping Result

It is used to arrange identical data into groups so that aggregate functions can work on them

### Group by clause

It allows you to group two or more columns and then you can perform aggregate function on them

```sql
Select <column>, Count(*) from <table_name> group by <column>;
```

Copy

### Having clause

Having clause is used to put conditions on groups

```sql
Select avg(<column>), sum(<column>) from <table_name> group by <column_name> having <condition_to_satisfy>;
```

Copy

## Altering Table

These commands allow you to change the structure of the table

### To Add New Column

It will add a new column in your table

```sql
Alter Table <table_name>
Add <new_column>;
```

Copy

### To Modify Old Column

It will update the data type or size of old column

```sql
Alter Table <table_name>
Modify <old_column_name> [<new_data_type><size>];
```

Copy

### To Change Name of Column

It will change the name of the old column in the table

```sql
Alter Table Change <old_column_name> <new_column_name><data_type>;
```

Copy

## Dropping Table

### DROP command

It will delete the complete table from the database

```sql
Drop table <table_name>;
```

Copy

## MySQL Functions:

There are many functions in MySQL that perform some task or operation and return a single value

## Text/String Functions

Text function work on strings

### Char Function

It returns the character for each integer passed

```sql
Select Char(72,97,114,114,121);
```

Copy

### Concat Function

It concatenates two strings

```sql
Select Concat("Harry","Bhai");
```

Copy

### Lower/Lcase

It converts a string into lowercase

```sql
Select Lower("Harry Bhai");
```

Copy

### Upper/Ucase

It converts a string into uppercase

```sql
Select Upper("CodeWithHarry");
```

Copy

### Substr

It extracts a substring from a given string

```sql
Select Substr(string,m,n);
```

Copy

### Trim

It removes leading and trailing spaces from a given string

```sql
Select Trim(leading ' ' FROM ' Harry Bhai');
```

Copy

### Instr

It searches for given second string into the given first string

```sql
Select Instr(String1,String2);
```

Copy

### Length

It returns the length of given string in bytes

```sql
Select Length(String)
```

Copy

## Numeric Functions

Numeric function works on numerical data and returns a single output

### MOD

It returns modulus of two numbers

```sql
Select MOD(11,4);
```

Copy

### Power

It returns the number m raised to the nth power

```sql
Select Power(m,n);
```

Copy

### Round

It returns a number rounded off number

```sql
Select Round(15.193,1);
```

Copy

### Sqrt

It returns the square root of a given number

```sql
Select Sqrt(144);
```

Copy

### Truncate

It returns a number with some digits truncated

```sql
Select Truncate(15.75,1);
```

Copy

## Date/Time Functions

These are used to fetch the current date and time and allow you to perform several operations on them

### Curdate Function

It returns the current date

```sql
Select Curdate();
```

Copy

### Date Function

It extracts the date part of the expression

```sql
Select Date('2021-12-10 12:00:00');
```

Copy

### Month Function

It returns the month from the date passed

```sql
Select Month(date);
```

Copy

### Day Function

It returns the day part of a date

```sql
Select Day(date);
```

Copy

### Year Function

It returns the year part of a date

```sql
Select Year(date);
```

Copy

### Now Function

It returns the current date and time

```sql
Select now();
```

Copy

### Sysdate Function

It returns the time at which function executes

```sql
Select sysdate();
```

Copy

## Aggregate Functions

Aggregate functions or multiple row functions work on multiple data and returns a single result

### AVG Function

It calculates the average of given data

```sql
Select AVG(<column_name>) "Alias Name" from <table_name>;
```

Copy

### COUNT Function

It counts the number of rows in a given column

```sql
Select Count(<column_name>) "Alias Name" from <table_name>;
```

Copy

### MAX Function

It returns the maximum value from a given column

```sql
Select Max(<column_name>) "Alias Name" from <table_name>;
```

Copy

### MIN Function

It returns the minimum value from a given column

```sql
Select Min(<column_name>) "Alias Name" from <table_name>;
```

Copy

### SUM Function

It returns the sum of values in given column

```sql
Select Sum(<column_name>) "Alias Name" from <table_name>;
```

Copy

## MySQL Joins

Join clause is used to combine or merge rows from two or more tables based on a related attribute

### INNER JOIN

It returns all rows from multiple tables where the join condition is satisfied. It is the most common type of join.

```sql
SELECT columns FROM table1 INNER JOIN table2 ON table1.column = table2.column;
```

Copy

### LEFT OUTER JOIN

It returns all rows from the left-hand table specified in the ON condition and only those rows from the other table where the join condition is fulfilled.

```sql
SELECT columns FROM table1 LEFT [OUTER] JOIN table2 ON table1.column = table2.column;
```

Copy

### RIGHT OUTER JOIN

It returns all rows from the RIGHT-hand table specified in the ON condition and only those rows from the other table where the join condition is satisfied

```sql
SELECT columns FROM table1 RIGHT [OUTER] JOIN table2 ON table1.column = table2.column;
```

Copy

### FULL JOIN

It combines the results of both left and right outer joins

```sql
SELECT column_name FROM table1 FULL OUTER JOIN table2 ON table1.column_name = table2.column_name WHERE condition;
```

Copy

### SELF JOIN

In this join, table is joined with itself

```sql
SELECT column_name FROM table1 T1, table1 T2 WHERE condition;
```