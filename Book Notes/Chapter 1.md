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


