SQL and Databases on the Quick
========================================================
author: Tyler Susmilch
date: May 10th, 2017
autosize: true

Awknowledgements
========================================================

- Bill Howe, University of Washington (Introduction to Data Science)  
- The Practical SQL Handbook; Bowman, Emerson, Darnovsky
- Countless other people's ideas I've picked-up over the last five years, nothing here is original from me

What Is a Database?
========================================================
A collection of information organized to afford efficient retrieval.

http://www.usg.edu/galileo/skills/unit04/primer04_01.phtml

What Is a Database?
=========================================================
-Data should be self-describing  
-Data should have a schema

Jim Gray, "The Fourth Paradigm"

What do Databases Solve?
========================================================
1) Sharing  
&nbsp;&nbsp;&nbsp;-Assists with concurrency by multiple readers and writers  
2) Data Model Enforcement  
&nbsp;&nbsp;&nbsp;-Ensure a clean, organized data set exists for use  
3) Scale  
&nbsp;&nbsp;&nbsp;-Assists work with datasets that are too large for memory  
&nbsp;&nbsp;&nbsp;-Often implements and executes algorithms on your behalf  
4) Flexibility  
&nbsp;&nbsp;&nbsp;-Anticipates a broad set of ways that the data may be used  

Bill Howe, University of Washington

Relational Database Management Systems
=========================================================
Allow for the flexible utilization of a single set of data, instead of a tight coupling between data structure and application.  

Manipulation of tabular/relational data follows an algebraic method, allowing independence from physical data representation.  

Algebraic Optimization
==========================================================
$$N = ((z*2)+((z*3)+0))/1$$

Algebraic Laws:  
1.&nbsp;  (+) identity: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $x+0 = x$  
2.&nbsp;  (/) identity: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $x / 1 = x$  
3.&nbsp;  (\*) distributes: &nbsp;$(n*x+n*y) = n*(x+y)$  
4.&nbsp;  (\*) commutes: &nbsp;&nbsp;&nbsp;$x*y = y*x$  

Apply rules 1, 3, 4, 2:  
$N = (2+3)*z$  

SQL, Relational Databases, and Relational Algebra
===========================================================
- SQL Query -> Relational Database -> Relational Algebra  
- Relational Algebra Optimization -> Query Plan  
- Query Plan executes -> Query Results  
- Query Results -> Relational Table  

SQL is Declarative
===========================================================
You are specifying the "WHAT", not the "HOW"  

You let the database engine worry about the best way to retrieve. That's the power of Relational Algebra and Cost Optimization.

The FROM clause
===========================================================
Specifies where to get the data for your operations.

We'll use rudimentary "Customers" table and "Employees" table for these examples.  

You can also alias your objects.

Union
============================================================
$$Customers \cup Employees$$  


```sql
select * from Customers
UNION (ALL)
select * from Employees
```

Note - the (*) is in place of listing all the columns (in our case, all 2)

Difference
=============================================================
$$Customers - Employees$$  


```sql
select * from Customers
EXCEPT
select * from Employees
```

Note, there are other ways to perform this operation as well.

Relational Algebra Selection (WHERE)
=============================================================
All records that satisfy some condition.  

In a super confusing twist, Relational Algebra "Selection" maps to SQL "WHERE" clause.  


```sql
select *
from Customers
where last_name = 'Smith' (and/or/not first_name = 'Aldon')
```

Specifying Conditions (WHERE and Join)
============================================================
- =, <, >, <=, >-, <>
- IN
- BETWEEN
- LIKE (%)
- IS NULL
- NOT (negation)
- Function results

Pretty much any boolean

Relational Algebra Projection (SELECT)
=============================================================
Eliminates columns. Allows aliasing.  

Again... confusing... Relational Algebra "Projection" maps to SQL "Select" clause.  


```sql
select last_name ln
from Customers
```

Note - "true" Relational Algebra/Set Theory wouldn't allow duplication.  

Equi-Join / Inner Join (1/2)
============================================================
Join match tables horizontally.  


```sql
select *
from Customers cust, Employees emp
where cust.last_name = emp.last_name
```

Equi-Join / Inner Join (2/2)
=============================================================
  

```sql
select *
from Customers
  (inner) join Employees
    on Customers.last_name = Employees.last_name
```

Both patterns are acceptable to the database engine.  
This is also how you do the Intersection set operation like:  
$$Customers \cap Employees$$

Theta-Join
=============================================================
The join condition needn't be an equality.


```sql
select *
from Customers
  (inner) join Employees
    on Customers.last_name < Employees.last_name
```

Outer Joins
==============================================================
-Include the record even if it doesn't have a match.  
-Represent missing values with NULL  

Flavors?
- Left
- Right
- Full  

Join Visualization
==============================================================

Cross (or Cartesian) Product
=============================================================
Get every combination of records possible.  


```sql
select *
from Customers
  cross join Employees
```

DISTINCT
==============================================================
Everything is pretty much a "bag" unless you state otherwise.  


```sql
select distinct last_name
from Customers
```

This will remove duplicates from result set, but beware (EXPENSIVE)  

Sorting (ORDER BY)
===============================================================
Sorted in given order, ascending is default

```sql
select last_name, first_name
from Customers
order by last_name, first_name
```
vs

```sql
select last_name, first_name
from Customers
order by last_name desc, first_name (desc)
```

Aggregate Functions
===============================================================
Function is performed on some set of rows.  


```sql
select count(*) cnt from Customers
```
OR

```sql
select count(distinct last_name) cnt from Customers
```

Other Common Aggregate Functions
===============================================================
- SUM
- AVG
- MAX
- MIN

Generally, NULL values are ignored by aggregate functions  

GROUP BY clause
==============================================================
Create a grouping of rows based on the values in columns therein.


```sql
select last_name, count(*) cnt
from Customers
group by last_name
```

HAVING clause
==============================================================
Filter the results based upon the results of the aggregate function.  


```sql
select last_name, count(*) cnt
from Customers
group by last_name
having count(*) > 1
order by last_name
```

Clause Order (written)
==============================================================
SELECT is required, and FROM is required in many implementations.
Followed By (optional):  
WHERE  
GROUP BY  
HAVING  
ORDER BY  

Clause Order (executed, simplification)
===============================================================
FROM  
WHERE  
GROUP BY  
HAVING  
SELECT  
ORDER BY/DISTINCT  

CASE Statement (SQL's "If") [1/2]
================================================================


```sql
select
    cust.*
  , CASE last_name
      WHEN 'Smith'
        THEN 'Y'
      ELSE 'N'
    END "Common"
from Customers cust
```
OR...

CASE Statement (SQL's "If") [2/2]
=================================================================


```sql
select
    cust.*
  , CASE WHEN last_name = 'Smith' THEN 'Y'
         ELSE 'N'
    END "Common"
from Customers cust
```

Other Topics To Extend
===============================================================
-Variants (SQL, PL-SQL, T-SQL, PL/pgSQL)  
-Common Table Expressions  
-Recursion  
-Analytic Functions  
-Cursors  
-Stored Procedures  
-User Defined Functions  
-Constraints, Triggers, and Cascades  
-Data Modeling (OLAP, OLTP) / "Tidy" data  
-Database Types (Relational, NoSQL, Columnar, Memory-Only, Graph, etc.) 
