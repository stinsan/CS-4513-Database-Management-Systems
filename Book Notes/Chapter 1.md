# Chapter 1: Introduction

**Database-Management System (DBMS)**: 
- a collection of interrelated data
- contains information about a particular enterprise
- stores and retrieves information conveniently and efficiently

## Chapter 1.1: Database-System Applications

**Database Usage**:
- Banking: *transactions*
- Airlines: *reservations, schedules*
- Universities:  *registration, grades*
- Sales: *customers, products, purchases*
- Online retailers: *order tracking, customized recommendations*
- Manufacturing: *production, inventory, orders, supply chain*
- Human resources:  *employee records, salaries, tax deductions*

**Two Modes of Database Usage:**
1. Online transaction processing
2. Data analytics

## Chapter 1.2: Purpose of Database Systems

**Why use databases over file systems?**
- Data redundancy and inconsistency: *multiple file formats, duplication of information in different files*
- Difficulty in accessing data: *may need to write a new program to access new data*
- Data isolation: *multiple files and formats*
- Integrity problems: *hard to add new constraints or change existing ones*
- Atomicity of updates: *failures may leave database in an inconsistent state with partial updates carried out*
- Concurrent access by multiple users: *uncontrolled concurrent accesses can lead to inconsistencies*
- Security problems: *Hard to provide user access to some, but not all, data*

## Chapter 1.3: View of Data

A major purpose of a database system is to
provide users with an **abstract** view of the data. That is, the system hides certain details
of how the data are stored and maintained.

### Chapter 1.3.1: Data Models

**Data Model**: the underlying structure of a database that is 
a collection of conceptual tools for describing data, data relationships, data semantics, and consistency constraints.

**Four Categories of Data Models**:

1. **Relational Model**:
- The relational model uses a collection of tables to represent both
data and the relationships among those data.

2. **Entity-Relationship Model**:
- The entity-relationship (E-R) data model uses a collection of basic objects, called entities, and relationships among these objects.

3. **Semi-Structured Data Model**:
- The semi-structured data model permits the specification of data where individual data items of the same type may have different
sets of attributes, such as XML or JSON

4. **Object-Based Data Model**:
- The object-based data model is an extension of the relational model with notions of encapsulation, methods, and object identity.

### Chapter 1.3.2: Relational Data Model
An example of two tables in a relational data model:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/0-databases.png)

### Chapter 1.3.3: Data Abstraction

**The Three Levels of Abstraction**:
1. **Physical Level**: *the lowest level of abstraction, describes complex low-level data structures in detail*
2. **Logical Level**: *describes what data are stored in the database, and what relationships exist among those data*
3. **View Level**: *the highest level of abstraction, application programs can hide details of data types, views can also hide information for security purposes*

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/1-databases.png)

### Chapter 1.3.4: Instances and Schemas
**Instance**: *the collection of information stored in the database at a particular moment*

**Schema**: *the overall design of a database, can have differing designs based on the levels of abstraction (physical and logical schemas), can also have several schemas at the view level that describe
different views of the database (subschemas)*

**Physical Data Independence**: *the ability to change the physical schema without changing the logical schema.*

## Chapter 1.4: Database Languages
**Data-Definition Language (DDL)**: *specifies the database schema*

**Data-Manipulation Language (DML)**: *expresses database queries and updates*

### Chapter 1.4.1: Data-Definition Language
We specify the storage structure and access methods used by the database system
by a set of statements in a special type of DDL called a **data storage and definition
language**.

Database systems also implements some integrity constraints:
- **Domain Constraints**: *a domain of possible values must be associated with every
attribute (for example, integer types, character types, date/time types)*
- **Referential Integrity**: *ensures that a value that appears in one relation for 
a given set of attributes also appears in a certain set of attributes in another relation*
- **Authorization**: *differentiates users as far as the type of
access they are permitted on various data values in the database*

The output of the DDL is placed in the **data dictionary**, which contains **metadata** -- that is, data about data.

### Chapter 1.4.2: The SQL Data-Definition Language 

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/2-databases.png)

### Chapter 1.4.3: Data-Manipulation Language

**Procedural DMLs**: *require a user to specify what data are needed and how to get
those data*

**Declarative DMLs (non-procedural DMLs)**: *require a user to specify what data are needed without specifying how to get those data

A **query** is a statement requesting the retrieval of information. The portion of a
DML that involves information retrieval is called a **query language**.

### Chapter 1.4.4: The SQL Data-Manipulation Language

Here is an example of an SQL query that finds the names of all instructors in the History department:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/3-databases.png)

The following query finds the instructor ID and department name of all instructors associated
with a department with a budget of more than $95,000:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/4-databases.png)

## I am skipping Chapters 1.5 to 1.8 because I do not want to write notes about them.

