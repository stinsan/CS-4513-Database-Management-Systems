# Chapter 3: Introduction to SQL
## 3.1 | Overview of the SQL Query Language

**Data-Definition Language (DDL)**: Provides commands for defining relation schemas, deleting relations, and modifying relation schemas.

**Data-Manipulation Language (DML)**: Provides the ability to query information from the database and to insert tuples into, delete tuples from, and modify tuples in the database.

## 3.2 | SQL Data Definition

The SQL DDL allows specification of not only a set of relations, but also information
about each relation, including:
- The schema for each relation.
- The types of values associated with each attribute.
- The integrity constraints.
- The set of indices to be maintained for each relation
- The security and authorization information for each relation.
- The physical storage structure of each relation on disk.

### 3.2.1 | Basic Types

The SQL standard supports a variety of built-in types, including:
- **char**(_n_): A fixed-length character string with user-specified length _n_.
- **varchar**(_n_): A variable-length character string with user-specified maximum length _n_.
- **int**: An integer.
- **smallint**: A small integer.
- **numeric**(_p_, _d_): A fixed-point number with user-specified precision. The number consists of _p_ digits (plus a sign), and _d_ of the _p_ digits are to the right of the decimal point. Thus, _numeric(3, 1)_ allows 44.5 to be stored, but neither 444.5 nor 0.32 can be stored.
- **real, double precision**: Floating-point and double-precision floating-point numbers.
- **float(_n_)** A floating-point number with precision of at least _n_ digits.

**Null**: Each type may include a special value called the null value. A null value indicates
an absent value that may exist but be unknown or that may not exist at all.

### 3.2.2 | Basic Schema Definition

We define an SQL relation by using the **create table** command. 

The following command creates a relation _department_ in the database:
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-31.png)

The relation created above has three attributes, _dept_name_, which is a character string
of maximum length 20, _building_, which is a character string of maximum length 15,
and _budget_, which is a number with 12 digits in total, two of which are after the decimal point. The create table command also specifies that the _dept_name_ attribute is the primary key of the _department_ relation.

The general form of the _create table_ command is:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-32.png)

where _r_ is the name of the relation, each _Ai_ is the name of an attribute in the schema of
relation _r_, and _Di_ is the domain of attribute _Ai_.

SQL supports a number of different integrity constraints. In this section, we discuss only a few of them:
- **primary key**_(A1, A2, ..., An)_: The primary-key specifications says that attributes _A1, A2, ..., An_ form the primary key for the relation.
- **foreign key**_(A1, A2, ..., An)_ **references** _s_: The foreign key specification saays the the values of attributes (_A1, A2, ..., An_) for any tuple in the relation must correspond to values of the primary key attributes of some tuple in relation _s_.
- **not null**: The _not null_ constraint of an attribute specifies that the null value is not allowed for that attribute.

Figure 3.1 shows the SQL data definition for part of the university database:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-33.png)

There are also commands for manipulating existing tables:

- **drop table** _r_: This command deletes all information about the dropped relation _r_ from the database, deleting all tuples of _r_ and the schema for _r_.

- **delete from** _r_: This command retains relation _r_, but deletes all tuples in _r_.

- **alter table** _r_ **add** _A D_: The _alter table_ command adds attributes to an existing relation, where _r_ is the name of an existing relation, _A_ is the name of the attribute to be added, and _D_ is the type of added attribute. All tuples in the relation are assigned _null_ as the value for the new attribute.

- **alter table** _r_ **drop** _A_: This command drops attributes from a relation, where _r_ is the name of an existing relation, and A is the name of an attribute of the relation.

## 3.3 | Basic Structure of SQL Queries

The basic structure of an SQL query consists of three clauses: **select**, **from**, and **where**.

### Queries on a Single Relation:

Consider the simple query, "Find the names of all instructors." 

Instructor names are found in the _instructor_ relation, so we put that in the from clause. The instructor's name appears under the _name_ attribute, so we put that in the select clause. The query looks like this:

**select** _name_<br/>
**from** _instructor_;

The resulting table looks like:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-35.png)

Now, consider the query, "Find the department names of all instructors." The SQL query looks like this:

**select** _dept_name_ <br/>
**from** _instructor_;

The resulting table looks like:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-36.png)

Note how there re duplicate department names. If we want to force the elimination of duplicates, we insert the keyword **distinct** after the select clause:

**select distinct** _dept_name_<br/>
**from** _instructor_;

The select-clause can also take arithmetic operations such as +, -, *, and /. Take, for example, the query:

**select** _ID, name, dept_name, salary * 1.1_<br/>
**from** _instructor_;

This query returns a relation that is the same as the _instructor_ relation, except that the _salary_ attribute is multiplied by 1.1. This shows what would result if we gave a 10% raise to each instructor, but does not actually result in any change to the _instructor_ relation.

The where clause allows us to select rows from a relation that satisfy a specified predicate. Consider the query, "Find the names of all instructors in the Computer Science department who have a salary greater than $70,000." This can be written in SQL as:

**select** _name_<br/>
**from** _instructor_<br/>
**where** _dept_name = 'Comp. Sci.' and salary > 70000;_

The result:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-37.png)

Logical connectives such as _and, or,_ and _not_ can be used in the where clause. The operands of the logical connectives can also involve comparison operators such as _<, <=, >, >=, =,_ and _<>_.

### Queries on Multiple Relations
Suppose we want to answer the query, "Retrieve the name of all instructors, along with their department names and department building name."

Looking at the _instructor_ relation, the department name is under the attribute _dept_name_, but the building name is present under the _building_ attribute of the _department_ relation. To answer the query, each
tuple in the _instructor_ relation must be matched with the tuple in the _department_ relation
whose _dept_name_ value matches the _dept_name_ value of the _instructor_ tuple:

**select** _name, instructor.dept_name, building_<br/>
**from** _instructor, department_<br/>
**where** _instructor.dept_name = department.dept_name;_

Note that the attribute _dept_name_ occurs in both the relations _instructor_ and _department_, and the relation name is used as a prefix (in _instructor.dept_name_, and _department.dept_name_) to make clear to which attribute we are referring.

The result of the query:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-38.png)

The from clause acts as a Cartesian product of the relations listed in the clause.

Take, for example, the query:

**select** *<br/>
**from** _instructor, teaches_

This outputs a relation that is a combination every tuple in _instructor_ and every tuple in _teaches_ even if they are unrelated to one another, which is not useful. The table below shows the result:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-39.png)

The where clause combined with the Cartesian product from the from clause produces a more useful result, though.

The following query matches _teaches_ tuples with _instructor_ tuples that have the same _ID_ value. That is, it returns a table that
matches an instructor with all the classes he/she teaches:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-40.png)
