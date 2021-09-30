# TeraData SQL

Installation

VM Ware WorkStation

TeraData 

TeraData Utility Tools (TUT)

ODBC Driver



root: username, password



TeraData Studio...where we will write sql queries



Teradata SQL Assistant Tool





New Connection Profile...TeraData

Name: 

Server Name: ip in VM Ware

UserName: dbc

Password: 





Under Defaut User, database will be created

```sql
CREATE DATABASE dbName FROM DBC AS PERM = 1000000000;
```









What is Teradata?

Teradata is one of the popular relational database management system. It is mainly suitable for building large scale data warehousing applications.



Components of Teradata

- Node
- Parsing Engine
- Message Passing Layer (BYNET)
- AMP (Access Module Processor)
- Virtual Disk



Node

- Each individual server in a Teradata system is reffered as a node.
- A node consits of its own operating system, CPU, memory, own copy of Teradata RDBMS software and disk space.



Parsing Engine

- Session Control
  - manages user's session
  - manages upto 120 sessions
- Parser
  - validates user & privileges
  - check sql syntax
- Optimizer
  - optimizes the query to come up with execution plan
- Dispatcher
  - passes the execution plan through BYNET

![image-20210929112709178](/home/jeet-dhanesha/.config/Typora/typora-user-images/image-20210929112709178.png)





Messaging Passing Layer (BYNET)

- It allows the communication between PE & AMP & also between the nodes.
- It recives the execution plan from PE & sends to AMP.
- It recives the results from the AMPs and sends to PE.



Access Module Processor (AMP)

- AMPs, called as Virtual Processors (vprocs) are the one that actually stores and retrives the data from disk.
- AMPs recieve the data and execution plan from PE.
- It performs data type conversion, aggregation, filter, sorting & stores the data in the disks associated with them.



Teradata Architechture 

[img]



Features of Teradata

- Unlimited Parallelism
- Shared Nothing Architechture
- Linear Scalability
- Robust Utilities



Teradata Storage Architechture

- Suppose 4 rows are to be inserted. And 1st row goes to PE.
- PE contains Hashing Algorithm. Hashing Algorithm will run on primary key of the row.
- It will generate hash key. Hash key decides to which AMP storage will the data goes to.

[img]



Teradata Retrieval Architechture

- Suppose 1st row is to be retrived.
- Based on hash key it will look for data into different AMPs.
- Data will be sent by AMP to Message Passing Layer and returned.

[img]



Teradata will store data evenly across the AMPs.





Primary Index in Teradata

```sql
CREATE TABLE Dice.Worker(
	EmployeeNo INTEGER,
    FirstName VARCHAR(30),
    LastName VARCHAR(30),
    DOB DATE FORMAT 'YYYY-MM-DD'
)
UNIQUE PRIMARY INDEX(EmployeeNo);
```



```sql
CREATE TABLE Dice.Worker(
	EmployeeNo INTEGER,
    FirstName VARCHAR(30),
    LastName VARCHAR(30),
    DOB DATE FORMAT 'YYYY-MM-DD'
)
PRIMARY INDEX(EmployeeNo);
```



- If `primary index` is not mentioned, Teradata will consider first column as the `non unique primary index`.



Data Retrieval Without Primary Index

- For a query, all the AMPs are to be searched. [ Slow ]

Data Retrieval With Primary Index

- For a query, it knows which AMP to search for data. [ Fast]

[img]





Unique Primary Index (UPI)



[img]





All the data with same index are stored in same AMP.

Duplicate Primary Index will put load on a particular AMP.





Secondary Index in Teradata

Why to define Secondary Index?

- fast results when query is not based on `primary index`

Types of Secondary Index

- Unique Secondary Index (USI)
- Non-Unique Secondary Index (NUSI)



Unique Secondary Index

1-2 AMP operation



Drawbacks of Secondary Index



## Multiset & Set Tables

- `Multiset` Table allows duplicate values.

- `Set` table allows duplicate tables.

```sql
CREATE SET TABLE Dice.Employee (
	Employee_no INTEGER,
    Dept_no INTEGER,
    Last_name VARCHAR(20),
    First_name VARCHAR(20),
    Salary INTEGER,
    Social_Security CHAR(11)
)
PRIMARY INDEX(Employee_no);
```



```sql
CREATE MULTISET TABLE Dice.Employee (
	Employee_no INTEGER,
    Dept_no INTEGER,
    Last_name VARCHAR(20),
    First_name VARCHAR(20),
    Salary INTEGER,
    Social_Security CHAR(11)
) PRIMARY INDEX(Employee_no);
```



Interms of performance, multiset table is better because it doesnt have to check for duplicates.



### No Primary Index (NOPI)

- A NOPI is simply a table without a primary index.
- A NOPI table distributes the data randomly, but evenly among different AMPs.
- Data is inserted based on random distribution.
- Since no hashing is involved, data load will be significantly faster.
- Will always be a `Multiset Table`.

```sql
CREATE MULTISET TABLE Dice.Worker(
    Employee_no INTEGER,
    Dept_no INTEGER,
    Last_name VARCHAR(20),
    First_name VARCHAR(20),
    Salary INTEGER,
    Social_Security CHAR(11)
) NO PRIMARY INDEX;
```

Data will be searched in all AMPs.

Uses:

Limitations:





### Types of Spaces in Teradata

- Permanent Space
- Spool Space
- Temporary Space



#### Permanent Space

- Space where objects such as databases, users, tables, views etc are created and stored.
- Permanent space is released when data is deleted or when objects are dropped from the database.
- Unused permanent space is available for  spool or temporary space.

#### Spool Space

- Unused space in



##### Volatile Table

```sql
        CREATE VOLATILE TABLE Sample_Table, NO LOG
(
Dept_No              Integer,
Salary                    Integer
)
On Commit Preserve Rows;
```







### Union & Union All

- `Union` removes the duplicate.

- `Union All` includes all the duplicate.

Rules for Union

- No of columns must be same.
- Columns should have same data types.

Example of `Union`:

```sql
SELECT employee_name FROM Dice.Employee
UNION
SELECT worker_name FROM Dice.Workers;
```

Example of `Union All`:

```sql
SELECT employee_name FROM Dice.Employee
UNION ALL
SELECT worker_name FROM Dice.Workers;
```



Primary Index Vs Primary Key

| Primary Index                           | Primary Key                       |
| --------------------------------------- | --------------------------------- |
| Physical mechanism of access & storage. | Logical concept of data modeling. |
| Each table must have atleast one.       | No limits on column number.       |
| Both unique & non-unique.               | Must be unique.                   |
| Can be null.                            | Cannot be null.                   |



RAID Technology (Disk Level Protection Technique)

Raid 1

Raid 1 Architechture

Raid 5

Raid 5 Architechture



Parity will contain only important information to recover the data.



## Brief Overview of Teradata Administrator 

### Create Database in Teradata Administrator

Create Database ...

Database Name

Owner:

Perm Space

Spool Space

Temp Space

Before Journal: teradata takes snapshot before applying any changes



Teradata SQL Assistant

Establish Database Connection

DBC is the default user.



### Create a User in Teradata

