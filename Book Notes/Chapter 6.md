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

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-9.png)

## 6.3 | Complex Attributes

**Domain (value set)**: *a set of permitted values for each attribute*
- e.g. An attribute *course_id* might have a domain of all text strings of a certain length.
- e.g. An attribute *semester* might be strings from the set {Fall, Winter, Spring, Summer}.

**Simple and Composite Attributes**: *simple attributes cannot be divided into subparts, while composite attributes can*
- e.g.  An attribute *name* could be structured as a composite attribute consisting of
*first_name*, *middle_initial*, and *last_name*.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-10.png)

**Single-Valued and Multivalued Attributes**: *single-valued attributes can only have one value, while multivalued attributes can have multiple values; multivalued attributes are denoted in an E-R diagram like so: {phone_numbers}*
- e.g. An attribute *first_name* would be single-valued, while an attribute *phone_numbers* would be multivalued.

**Derived Attributes**: *derived attributes can be derived from the values of other related attributes or entities; a derived attribute is denoted in an E-R diagram like so: age ( )*
-  e.g. An attribute _age_ can be calculated from an attribute *date_of_birth* and the current date.

An attribute takes a **null value** when an entity does not have a value for it. The null
value may indicate “not applicable”.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-11.png)

## 6.4 | Mapping Cardinalities

**Mapping Cardinalities (Cardinality Ratios)**: *expresses the number of entities to which another entity can be associated via a relationship set*

For a binary relationship set R between entity sets A and B, the mapping cardinality
must be one of the following:

1. **One-to-one**: *an entity in A is associated with at most one entity in B, and an entity in B is associated with at most one entity in A.*

2. **One-to-many**: *an entity in A is associated with any number of entities in B; an entity in B, however, can be associated with at most one entity in A.*

3. **Many-to-one**: *an entity in A is a sociated with at most one entity in B; an entity in B, however, can be associated with any number of entities in A.*

4. **Many-to-many**: *an entity in A is associated with any number of entities in B; an entity in B is associated with any number of entities in A.*

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-12.png)

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-13.png)

**Cardinalities on an E-R Diagram**:
1. **One-to-one**: We draw a directed line from the relationship set to both entity sets. In the example below, the directed lines to *instructor* and _student_ indicates that and instructor may advse at most one student, and a student may have at most one advisor.

2. **One-to-many**: We draw a directed line from the relationship set to the "one" side of the relationship. In the example below, there is a directed line from the relationship set _advisor_ to the entity set _instructor_, and an undirected line to tthe entity set _student_. This indicates that an instructor may advise many students, but a student may have at most one advisor.

3. **Many-to-one**: We draw a directed line from the relationship set to the "one" side of the relationship. In the example below, there is an undirected line from the relationship set _advisor_ to the entity set _instructor_ and a directed line to the entity set _student_. This indicates that an instructor may advise at most one student, but a student may have many advisors.

4. **Many-to-many**: We draw an undirected line from the relationship set to both entity sets. In the example below, there are undirected lines from the relationship set _advisor_ to both entity sets _instructor_ and _student_. This indicates that an instructor may advise many students, and a student may have many advisors.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-14.png)

**Total and Partial Participation**: *The participation of an entity set E in a relationship set R is said to be total if every
entity in E must participate in at least one relationship in R. If it is possible that some
entities in E do not participate in relationships in R, the participation of entity set E in
relationship R is said to be partial.*

- e.g. For example, a university may require every student to have at least one advisor; 
Therefore, the participation of student in the relationship set advisor is total. In contrast, an instructor need not advise any
students, and the participation of instructor in the advisor relationship set is therefore partial.

We indicate total participation of an entity in a relationship set using double lines.
The figure below shows an example of the advisor relationship set where the double line
indicates that a student must have an advisor.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-15.png)

E-R diagrams also provide a way to indicate more complex constraints on the number of times each entity participates in relationships in a relationship set. A line may have an associated minimum and maximum cardinality, shown in the form _l..h_, where _l_ is the minimum and _h_ the maximum cardinality. A minimum value of 1 indicates total
participation of the entity set in the relationship set; that is, each entity in the entity
set occurs in at least one relationship in that relationship set. A maximum value of
1 indicates that the entity participates in at most one relationship, while a maximum
value ∗ indicates no limit.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-16.png)

## 6.5 | Primary Key
### 6.5.1 | Entity Sets
I am skipping this section.

### 6.5.2 | Relationship Sets
I am also skipping this section.

### 6.5.3 | Weak Entity Sets
A **weak entity set** is one whose existence is dependent on another entity set, called its **identifying entity set**;
instead of associating a primary key with a weak entity, we use the primary key of the
identifying entity, along with extra attributes, called **discriminator attributes** to uniquely
identify a weak entity. An entity set that is not a weak entity set is termed a **strong entity
set**.

In E-R diagrams, a weak entity set is depicted via a double rectangle with the discriminator being underlined with a dashed line. The relationship set connecting the
weak entity set to the identifying strong entity set is depicted by a double diamond. In
the figure below, the weak entity set *section* depends on the strong entity set *course* via the
relationship set *sec_course*.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-18.png)

## 6.6 | Removing Redundant Attributes in Entity Sets
I am skipping this section.

## 6.7 | Reducing E-R Diagrams to Relational Schemas
### 6.7.1 | Representation of Strong Entity Sets
Let _E_ be a strong entity set with only simple descriptive attributes _a1, a2, ..., an_. We
represent this entity with a schema called _E_ with _n_ distinct attributes. Take in account the
entity 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_student(<ins>ID</ins>, name, tot_cred)_,

since _ID_ is the primary key of the entity set, it is also the primary key
of the relation schema.

### 6.7.2 | Representation of Strong Entity Sets with Complex Attributes
Take in account the _instructor_ entity in figure 6.8:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-11.png)

For the composite attribute _name_, the schema generated for _instructor_ contains the attributes _first_name, middle_initial_, and _last_name_; there is no separate attribute or schema for
_name_. Similarly, for the composite attribute _address_, the schema generated contains
the attributes _street, city, state_, and _postal_code_. Since _street_ is a composite attribute it is
replaced by _street_number, street_name_, and _apt_number_.

Multivalued attributes are treated differently from other attributes; new relation
schemas are created for these attributes.

Derived attributes are not explicitly represented in the relational data model. However, they can be represented as stored procedures, functions, or methods in other data models.

The relational schema derived from the version of entity set _instructor_ with complex
attributes, without including the multivalued attribute, is thus:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_instructor (ID, first_name, middle_initial, last_name, </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street_number, street_name, apt_number, </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city, state, postal_code, date_of_birth)_.

For a multivalued attribute _M_, we create a relation schema _R_ with an attribute _A_
that corresponds to _M_ and attributes corresponding to the primary key of the entity
set or relationship set of which _M_ is an attribute. Thus, the multivalued attribute _phone_number_
from figure 6.8 becomes the relational schema 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_instructor_phone(<ins>ID</ins>, <ins>phone_number</ins>)_.

Each phone number of an instructor is represented as a unique tuple in the relation on
this schema. Thus, if we had an _instructor_ with _ID_ 22222, and phone numbers 555-1234
and 555-4321, the relation _instructor_phone_ would have two tuples (22222, 555-1234)
and (22222, 555-4321).

### 6.7.3 | Representation of Weak Entity Sets
Let _A_ be a weak entity set with attributes _a1, a2, ..., am_. Let _B_ be the strong entity set
on which _A_ depends. Let the primary key of _B_ consist of attributes _b1, b2, ..., bn_. We
represent the entity set _A_ by a relation schema called _A_ with one attribute for each
member of the set:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_{a1, a2, ..., am} ∪ {b1, b2, ..., bn}_

For schemas derived from a weak entity set, the combination of the primary key of
the strong entity set and the discriminator of the weak entity set serves as the primary
key of the schema. In addition to creating a primary key, we also create a foreign-key
constraint on the relation _A_, specifying that the attributes _b1, b2, ..., bn_ reference the
primary key of the relation _B_.

As an illustration, consider the weak entity set _section_ in figure 6.15 becomes the relational schema

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_section(<ins>course_id</ins>, <ins>sec_id</ins>, <ins>semester</ins>, <ins>year</ins)_.
  
### 6.7.4 | Representation of Relationship Sets
Let _R_ be a relationship set, let _a1, a2, ..., am_ be the set of attributes formed by the union
of the primary keys of each of the entity sets participating in _R_, and let the descriptive
attributes (if any) of _R_ be _b1, b2, ..., bn_. We represent this relationship set by a relation
schema called _R_ with one attribute for each member of the set:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_{a1, a2, ..., am} ∪ {b1, b2, ..., bn}_.

As an illustration, consider the relationship set advisor in the E-R diagram of Figure
6.15. This relationship set involves the following entity sets:
- _instructor_, with the primary key _ID_.
- _student_, with the primary key _ID.

Since the relationship set has no attributes, the _advisor_ schema has two attributes, the
primary keys of _instructor_ and _student_. Since both attributes have the same name, we rename them _i_ID_ and _s_ID_. Since the advisor relationship set is many-to-one from _student_
to _instructor_ the primary key for the advisor relation schema is _s_ID_.

We also create foreign-key constraints on the relation schema _R_ as follows:</br> 
For each entity set _Ei_ related by relationship set _R_, we create a foreign-key constraint from relation schema _R_, with the attributes of _R_ that were derived from primary-key attributes
of _Ei_ referencing the primary key of the relation schema representing _Ei_.

Returning to our earlier example, we thus create two foreign-key constraints on
the _advisor_ relation, with attribute _i_ID_ referencing the primary key of _instructor_ and
attribute _s_ID_ referencing the primary key of _student_.

Applying the preceding techniques to the other relationship sets in the E-R diagram
in figure 6.15, we get the relational schemas depicted in figure 6.17.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-20.png)

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-21.png)

### 6.7.5 | Redundancy of Schemas

In general, the schema for the relationship set linking a weak entity set to its corresponding strong entity set is redundant and does not need to be present in a relational database design based upon an E-R diagram.

## 6.8 | Extended E-R Features
### 6.8.1 | Specialization

**Specialization**: *specialization is the process of designating subgroupings within an entity set*

**Overlapping Specialization**: *the entity may belong to multiple specialized entity sets; this is shown on an ER diagram by using
two arrows*
- e.g. A _person_ can be both a _student_ and an _employee_.

**Disjoint Specialization**: *the entity may belong to at most one specialized entity set; this is shown on an ER diagram by using a 
single arrow*
- e.g. An _employee_ can only have one job, either a _secretary_ or an _instructor_.

### 6.8.2 | Generalization

Essentially, **generalization** is the opposite of specialization. Specialization represents a top-down design process, while generalization represents a bottom-up design process.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-19.png)

### 6.8.3 | Inheritance

The attributes and relationships of higher-leveled entities are **inherited** by the lower-leveled entities.

### 6.8.4 | Constraints on Specializations

**Total Specialization/Generalization**: *each higher-level entity must belong to a lower-level entity set*

**Partial Specialization/Generalization**: *some higher-level entities may not belong
to any lower-level entity set*
