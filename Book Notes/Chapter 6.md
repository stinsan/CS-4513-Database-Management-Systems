# Chapter 6: Database Design Using the E-R Model

## 6.1 | Overview of the Design Process

I do not want to write notes about this section.

## 6.2 | The Entity-Relationship Model

**Entity-Relationship (E-R) Data Model**: *facilitates database design by
allowing specification of an enterprise schema that represents the overall logical structure of a database*

**E-R Diagram**: *a diagrammatic representation of an E-R data model*

### 6.2.1 | Entity Sets
**Entity**: *a "thing" or "object" in the real world that is distinguishable from all other objects*
- e.g. every student in a university is an entity that can have their own unique attributes such as *student_id*, per se

**Entity Set**: * set of entities of the same type that share the same properties or attributes*
- e.g. The entity set *student* might represent all students in a university

**Extension**: *the actual collection of entities belonging to the entity set*
- e.g. the set of actual instructors in the university form the extension of the entity set *instructor*

An entity set is represented in an E-R diagram by a rectangle, which is divided
into two parts. The first part, which in this text is shaded blue, contains the name of the entity set. 
The second part contains the names of all the attributes of the entity
set. Attributes that are part of the primary key are
underlined. The E-R diagram below shows two entity sets instructor and student. 

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-5.png)

### 6.2.2 | Relationship Sets
**Relationship**: *an association among several entities*
- e.g. We can define a relationship *advisor* that associates instructor Katz with student Shankar. 
This relationship specifies that Katz is an advisor to student Shankar.

**Relationship Set**: *a set of relationships of the same type*

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-6.png)

**Relationship Instance**: *an association between the named entities in the real world enterprise that is being modeled in an E-R schema*
- e.g. From the above graphic, the individual instructor entity Katz, 
who has instructor ID 45565, and the student entity Shankar, who has student ID 12345, 
participate in a relationship instance of *advisor*.

A relationship set is represented in an E-R diagram by a diamond, which is linked 
via lines to a number of different entity sets (rectangles).

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-7.png)

**Formal Definition of a Relationship Set**: *a mathematical relation on n ≥ 2 (possibly non-distinct) entity sets such that
if E<sub>1</sub>, E<sub>2</sub>, ..., E<sub>n</sub> are entity sets, the relationship set R is a subset of*

{(e<sub>1</sub>, e<sub>2</sub>, ..., e<sub>n</sub>) | e<sub>1</sub> ∈ E<sub>1</sub>, e<sub>2</sub> ∈ E<sub>2</sub>,..., e<sub>n</sub> ∈ E<sub>n</sub>}

*where (e<sub>1</sub>, e<sub>2</sub>, ..., e<sub>n</sub>) is a relationship instance.*

A relationship may also have attributes called **descriptive attributes**. As an example
of descriptive attributes for relationships, consider the relationship set takes which relates entity sets student and section. We may wish to store a descriptive attribute grade
with the relationship to record the grade that a student received in a course offering.
An attribute of a relationship set is represented in an E-R diagram by an undivided
rectangle. We link the rectangle with a dashed line to the diamond representing that
relationship set.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-8.png)

**Degree of the Relationship Set**: *the number of entity sets that participate in a relationship set*
- e.g. a binary relationship is of degree 2, a ternary relationship is of degree 3, and so on
- e.g. a binary relationship *teach* between two entity sets *instructor* and *class*
- e.g. a ternary relationship *proj_guide* between *instructor*, *student*, and *project*.
