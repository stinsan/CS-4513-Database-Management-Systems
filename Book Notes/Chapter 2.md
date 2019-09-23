# Introduction to the Relational Model
## 2.1 | Structure of Relational Databases

A relational database consists of a collection of **tables**, each of which is assigned a
unique name.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-23.png)
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-24.png)
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-25.png)

The _prereq_ table in Figure 2.3 indicates that two courses are related in the sense
that one course is a prerequisite for the other. As another example, when we consider
the table _instructor_ in Figure 2.1, a row in the table can be thought of as representing the relationship between a specified _ID_ and 
the corresponding values for _name_, _dept_name_, and _salary_.

In the relational model, the term **relation** is used to refer to a table, while the
term **tuple** is used to refer to a row. Similarly, the term **attribute** refers to a column of a
table.

The term **relation instance** refers to a specific instance of a relation, that
is, containing a specific set of rows. The instance of _instructor_ shown in Figure 2.1 has
12 tuples, corresponding to 12 instructors.

For each attribute of a relation, there is a set of permitted values, called the **domain**
of that attribute. Thus, the domain of the _salary_ attribute of the _instructor_ relation is
the set of all possible salary values, while the domain of the _name_ attribute is the set of
all possible instructor names.

A domain is **atomic** if elements of the domain are considered to be indivisible units.
For example, suppose the table _instructor_ had an attribute _phone_number_, which can
store a set of _phone_numbers_ corresponding to the instructor. Then the domain of
_phone_number_ would not be atomic, since an element of the domain is a set of phone
numbers, and it has subparts, namely, the individual phone numbers in the set.

The **null value** is a special value that signifies that the value is unknown or does not
exist. For example, suppose as before that we include the attribute _phone_number_ in the
_instructor_ relation. It may be that an instructor does not have a phone number at all,
or that the telephone number is unlisted. 

## 2.2 | Database Schema

**Database Schema**: the logical design of the database.

**Database Instance**: a snapshot of the data in the database at a given instant in time.

**Relation Schema**: corresponds to the programming-language notion of variable type definitions.

Consider the _department_ relation of Figure 2.5. The schema for that relation is: <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_department (dept_name, building, budget)_.

Consider the _section_ relation of Figure 2.6. The schema for that relation is:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_section (course_id, sec_id, semester, year, building, room_number, time_slot_id)_.

We also need a relation to describe the association between instructors and the class
sections that they teach. Figure 2.7 shows a sample instance of the _teaches_ relation. 
The relation schema to describe this association is: <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_teaches (ID, course_id, sec_id, semester, year)_.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-26.png)
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-29.png)
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-28.png)

## 2.3 | Keys

**Superkey**: a set of one or more attributes that uniquely identify a tuple in the relation; a super key may contain extraneous attributes.
- The _ID_ attribute of the relation _instructor_ is sufficient to distinguish one instructor tuple from another. Thus, _ID_ is a superkey.
- The _name_ attribute of instructor, on the other hand, is not a superkey, because several instructors might have the same name.

**Candidate Key**: a superkey for which no proper subset is a superkey itself; a "minimal superkey"; there can be multiple candidate keys per relation.
- The combination of _ID_ and _name_ is a superkey for the relation _instructor_. However, _name_ is an extraneous attribute. Thus, only the attribute _ID_ is considered a candidate key.

**Primary Key**: a candidate key that is chosen by the database designer as the principal means of identifying tuples within a relation; primary keys are underlined in a relation schema.
- Consider a _classroom_ relation _classroom (<ins>building</ins>, <ins>room_number</ins>, capacity)_. Here the primary key consists of two attributes, _building_ and _room_number_. Neither attribute by itself can uniquely identify a classroom, although together they uniquely identify a classroom.

**Foreign-Key Constraint**: given an attribute(s) _A_ of relation _r1_ and a primary-key _B_ of
relation _r2_, the value of _A_ for each tuple in _r1_ must also be the value of _B_ for some tuple in _r2_.

**Foreign-Key**: from the above definition, the attribute set _A_ is a foreign-key from _r1_ referencing _r2_.

**Referencing Relation**: from the above definition, the relation _r1_ is called the referencing relation of the foreign-key constraint.

**Referenced Relation**: from the above definition, the relation _r2_ is called the referenced relation of the foreign-key constraint.-

- The attribute _dept_name_ in _instructor_ is a foreign-key from _instructor_, referencing _department_.
- The attributes _building_ and _room_number_ of the _section_ relation together form a foreign key referencing the _classroom_ relation.

**Referential Integrity Constraint**: requires that the values appearing in specified attributes of any tuple in the referencing
relation also appear in specified attributes of at least one tuple in the referenced relation; relaxes the requirement that the
referenced attributes form the primary key of the referenced relation.

- Consider the values in the _time_slot_id_ attribute of the _section_ relation. We require that these values must exist in the
_time_slot_id_ attribute of the _time_slot_ relation. Such a requirement is an example of a referential integrity constraint. Note that
_time_slot_ does not form a primary key of the _time_slot_ relation, although it is a part of the primary key; thus, we cannot use a
foreign-key constraint to enforce the above constraint.

## 2.4 | Schema Diagrams

**Schema Diagram**: a visual depiction of a database schema, along with primary keys and foreign-key constraints.

Each relation appears as a box, with the relation name at the top in blue and the attributes listed inside the box.
Primary-key attributes are shown underlined. Foreign-key constraints appear as arrows from the foreign-key attributes of the referencing
relation to the primary key of the referenced relation. We use a two-headed arrow, instead of a single-headed arrow, to indicate a
referential integrity constraint that is not a foreign-key constraint. 

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-30.png)

### Sections 2.5 - 2.7 were not covered in lecture.

