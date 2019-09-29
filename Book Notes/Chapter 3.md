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
**from** _instructor, teaches_;

This outputs a relation that is a combination every tuple in _instructor_ and every tuple in _teaches_ even if they are unrelated to one another, which is not useful. The table below shows the result:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-39.png)

The where clause combined with the Cartesian product from the from clause produces a more useful result, though.

The following query matches _teaches_ tuples with _instructor_ tuples that have the same _ID_ value. That is, it returns a table that
matches an instructor with all the classes he/she teaches:

**select** _name, course_id_<br/>
**from** _instructor, teaches_<br/>
**where** _instructor.ID = teaches_ID_;

This query produces:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-40.png)

If we wished to find only instructor names and course identifiers for instructors
in the Computer Science department, we could add an extra predicate to the where
clause, as shown below:

**select** _name, course_id_<br/>
**from** _instructor, teaches_<br/>
**where** _instructor.ID = teaches_ID and instructor.dept_name = 'Comp. Sci.';_

## 3.4 | Additional Basic Operations

### The Rename Operation
The **as** clause provides a way of renaming attributes of a relation in an SQL query. There are three reasons as to why we would want to do this:
1. Two or more relations in the from clause may have the same attribute name, resulting in a duplicate attribute in the output table.
2. If an arithmetic expression is used in the select clause, the resultant attribute is nameless.
3. We may just want to change the attribute name in the resulting table. Ya know, for kicks.

The as clause takes the form of

_old-name_ **as** _new-name_,

and can appear in both select and from clauses.

Take, for example, the previously mentioned query:

**select** _name, course_id_<br/>
**from** _instructor, teaches_<br/>
**where** _instructor.ID = teaches_ID_;

If we wanted to replace attribute _name_ to _instructor_name_ in order to be clearer to which name, the instructor's or the course's, we are referring to, we would use the following query:

**select** _name_ **as** _instructor_name, course_id_<br/>
**from** _instructor, teaches_<br/>
**where** _instructor.ID = teaches_ID_;

We can also use an as clause to replace a long relation name with a shorter one for convenience:

**select** _T.name, S.course_id_ <br/>
**from** _instructor_ **as** _T, teaches_ **as** _S_<br/>
**where** _T.ID = S.ID_;

Another reason to rename a relation is a case where we wish to compare tuples
in the same relation. Suppose that we want to write the query “Find the names of all instructors whose
salary is greater than at least one instructor in the Biology department.” We can write
the SQL expression:

**select distinct** _T.name_<br/>
**from** _instructor_ **as** _T_, _instructor_ **as** _S_<br/>
**where** _T.salary > S.salary_ **and** _S.dept_name = 'Biology';_

### String Operations
This was not covered in lecture.

### Attribute Specification in the Select Clause
The asterisk symbol can be used in a select clause to represent "all attributes." The following query selects all attributes in the _instructor_ relation:

**select** _instructor.*_<br/>
**from** _instructor, teaches_<br/>
**where** _instructor.ID = teaches_ID;_

### Ordering of the Display of Tuples
The **order by** clause causes the tuples in the result of a query to appear in sorted
order.

To list in alphabetic order all instructors in the Physics department, we write:

**select** _name_<br/>
**from** _instructor_<br/>
**where** _dept_name = 'Physics'_<br/>
**order by** _name;_

By default, the order by clause lists items in ascending order. To specify the sort order,
we may specify **desc** for descending order or **asc** for ascending order.

Furthermore,
ordering can be performed on multiple attributes. Suppose that we wish to list the
entire instructor relation in descending order of salary. If several instructors have the
same salary, we order them in ascending order by name. We express this query in SQL
as follows:

**select** *<br/>
**from** _instructor_<br/>
**order by** _salary_ **desc**_, name_ **asc**_;_

### Where-Clause Predicates
There also exists **between** and **not between** comparison clauses to help simplify queries.

Take the following query for example:

**select** _name_<br/>
**from** _instructor_<br/>
**where** _salary <= 100000_ **and** _salary >= 90000;_

This can be simplified to:

**select** _name_<br/>
**from** _instructor_<br/
**where** _salary_ **between** 90000 **and** 100000;_

Row constructor notation can also be used in comparison operators. For example, _(a1, a2) <= (b1, b2)_ is true if _a1 <= b1_ and _a2 <= b2_.

Take for example the following query:

**select** _name, course_id_<br/>
**from** _instructor, teaches_<br/>
**where** _instructor.ID = teaches.ID_ **and** _dept_name = 'Biology';_

This query can be simplified by using row constructor notation as follows:

**select** _name, course_id_<br/>
**from** _instructor, teaches_<br/>
**where** _(instructor, dept_name) = (teaches.ID, 'Biology');_ 

## 3.5 | Set Operations
The SQL operations **union**, **intersect**, and **except** operate on relations and correspond to
the mathematical set operations ∪, ∩, and −. 

For the examples in this section, two relations will be used:
- The set of courses taught in the Fall 2017 semester, which will be named _c1_:

**select** _course_id_<br/>
**from** _section_<br/>
**where** _semester = 'Fall'_ **and** _year = 2017;_

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-41.png)

- The set of courses taught in the Spring 2018 semester, which will be named _c2_:

**select** _course_id_<br/>
**from** _section_<br/>
**where** _semester = 'Spring'_ **and** _year = 2018;_

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-42.png)

### The Union Operation
To find the set of all courses taught either in Fall 2017 or in Spring 2018, or both, we must take the union of _c1_ and _c2_,
which is written as:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-43.png)

The union operation eliminates duplicates, so the resulting relation is:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-44.png)

If duplicates are desired, the operation **union all** should be used instead.

### The Intersect Operation
To find the set of all courses taught in both the Fall 2017 and Spring 2018, we must find the intersection of _c1_ and _c2_, which is written as:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-45.png)

The intersection operation also eliminates duplicates, so the resulting relation is:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-46.png)

If duplicates are desired, the operation **intersect all** should be used instead.

### The Except Operation
To find all courses taught in the Fall 2017 semester but not in the Spring 2018 semester we must find _c1_ except _c2_, which is written as:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-47.png)

The except operation also eliminates duplicates, so the resulting relation is:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-48.png)

If duplicates are desired, the operation **except all** should be used instead.

## 3.6 | Null Values

An arithmetic operation involving a null value results in null as well. For example, if _r.A_ is null, then _r.A + 5_ is also null.

Comparison operations involving a null value results in **unknown**, which provides a third logical value in addition to true and false.
Since predicates in a where clause can involve boolean operators such as and, or, and not on the results of the comparison operations, the definitions of the boolean operations are extended to deal with the third value, unknown.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-50.png)

SQL uses the special keyword **is null** or **is not null** in a predicate to test for a null value. Thus, to
find all instructors who appear in the instructor relation with null values for salary, we
write:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-51.png)

SQL allows us to test whether the result of a comparison is unknown, rather than
true or false, by using the clauses **is unknown** and **is not unknown**. For example:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-52.png)

## 3.7 | Aggregate Functions



