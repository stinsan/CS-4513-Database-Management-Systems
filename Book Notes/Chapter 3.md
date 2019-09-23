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
