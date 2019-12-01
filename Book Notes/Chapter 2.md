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

### Section 2.5 was not covered in lecture.

## 2.6 | The Relational Algebra

The select, project, and rename operations are **unary operations** because they only operate on one relation.

The other operations, such as union, Cartesian product, and set difference, operate on pairs of relations and
are called **binary operations**.

### 2.6.1 | The Select Operation

The **select** operation, denoted by σ,selects tuples that satisfy a given predicate. The predicate appears as a subscript to σ and the argument relation is in parentheses after the σ.

- To select those tuples of the instructor relation where the instructor is in the “Physics” department, we write:<br/>
σ<sub>dept_name = “Physics”</sub> (instructor)

We allow comparisons using =, ≠, <, ≤, >, and ≥ in the selection predicate. Furthermore, we can combine several predicates into a larger predicate by using the connectives and (∧), or (∨), and not (¬).

- To find the instructors in Physics with a salary greater than $90,000, we write: <br/>
σ<sub>dept_name = “Physics” ∧ salary > 90000</sub> (instructor)

### 2.6.2 | The Project Operation
Suppose we want to list all instructors’ ID, name, and salary, but we do not care about
the dept_name. The **project operation** allows us to produce this relation. The project
operation, denoted by Π, returns its argument relation, with certain attributes left out.
The attributes that we want to appear in the result is put in the subscript of Π.

- To list all instructors’ ID, name, and salary, but not dept_name, we write: <br/>
Π<sub>ID, name, salary</sub> (instructor)

- To get the monthly salary of each instructor, we write: <br/>
Π<sub>ID,name,salary∕12</sub> (instructor)

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-97.png)

### 2.6.3 | Composition of Relational Operations
Since the result of a relational-algebra operation is of the same type
(relation) as its inputs, relational-algebra operations can be composed together into a
**relational-algebra expression**.

- Consider the more complicated query “Find the names of all instructors in the Physics
department.” We write: <br/>
Π<sub>name</sub> (σ<sub>dept_name = “Physics”</sub> (instructor))

### 2.6.4 | The Cartesian Product Operation
The **Cartesian-product operation**, denoted by a cross (×), allows us to combine information from any two relations. We write the Cartesian product of relations _r1_ and _r2_ as _r1_ × _r2_.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-98.png)

### 2.6.5 | The Join Operation
The **join operation** allows us to combine a selection and a Cartesian product into
a single operation. Consider relations _r(R)_ and _s(S)_, and let θ be a predicate on attributes in the
schema _R_ ∪ _S_. The join operation _r_ ⋈<sub>θ</sub> _s_ is defined as follows:
r ⋈<sub>θ</sub> s = σ<sub>θ</sub> (r × s).

- Suppose we want to find the information about all instructors together with the course
id of all courses they have taught. We write: <br/>
_instructor_ ⋈<sub>instructor.ID=teaches.ID</sub> _teaches_.

### 2.6.6 | Set Operations

The **union**, **intersection**, and **set-difference** operations are also present in relational algebra.

For these operations to be valid:

1. We must ensure that the input relations to the union operation have the same
number of attributes; the number of attributes of a relation is referred to as its
**arity**.

2. When the attributes have associated types, the types of the _i_ th attributes of both
input relations must be the same, for each _i_.

Such relations are referred to as **compatible relations**.

- Consider a query to find the set of all courses taught in the Fall 2017 semester, the
Spring 2018 semester, or both. We use the union operation to write this as: <br/>
Π<sub>_course_id_</sub> (σ<sub>_semester_=“Fall” ∧ _year_=2017</sub> (_section_)) ∪
Π<sub>_course_id_</sub> (σ<sub>_semester_=“Spring” ∧ _year_=2018</sub> (_section_)).

- Suppose that we wish to find the set of all courses taught in both the Fall 2017 and
the Spring 2018 semesters. Using set intersection, we can write: <br/>
Π<sub>_course_id_</sub> (σ<sub>_semester_=“Fall” ∧ _year_=2017</sub> (_section_)) ∩
Π<sub>_course_id_</sub> (σ<sub>_semester_=“Spring” ∧ _year_=2018</sub> (_section_)).

- We can find all the courses taught in the Fall 2017 semester but not in Spring 2018
semester by using set-difference: <br/>
Π<sub>_course_id_</sub> (σ<sub>_semester_=“Fall” ∧ _year_=2017</sub> (_section_)) -
Π<sub>_course_id_</sub> (σ<sub>_semester_=“Spring” ∧ _year_=2018</sub> (_section_)).

### 2.6.7 | The Assignment Operation
The **assignment operation**, denoted by ←, works like assignment in a programming language. 

- To illustrate this operation, consider the query to find courses that run in Fall 2017 as well as Spring 2018. We
could write it as: <br/>
_courses_fall_2017_ ← Π<sub>_course_id_</sub> (σ<sub>_semester_=“Fall” ∧ _year_=2017</sub> (_section_)) <br/>
_courses_spring_2018_ ← Π<sub>_course_id_</sub> (σ<sub>_semester_=“Spring” ∧ _year_=2018</sub> (_section_)) <br/>
_courses_fall_2017_ ∩ _courses_spring_2018_

### 2.6.8 | The Rename Operation
Unlike relations in the database, the results of relational-algebra expressions do not
have a name that we can use to refer to them. It is useful in some cases to give them
names; the **rename operator**, denoted by ρ, lets us do this.

Given a relational-algebra expression _E_, the expression<br/>
ρ<sub>_x_</sub> (_E_)<br/>
returns the result of expression _E_ under the name _x_.

A second form of the rename operation is as follows: Assume that a relational algebra expression _E_ has arity _n_. 
Then, the expression<br/>
ρ<sub>_x_(_A1,A2,...,An_)</sub> (_E_)<br/>
returns the result of expression _E_ under the name _x_, and with the attributes renamed
to _A1, A2, ..., An_.

- To illustrate renaming a relation, we consider the query “Find the ID and name of
those instructors who earn more than the instructor whose ID is 12121.” We can write this like: <br/>
Π<sub>_i.ID,i.name_</sub> ((σ<sub>_i.salary_ > _w.salary_</sub> (ρ<sub>_i_</sub> (instructor) × σ<sub>_w.id_=12121</sub>(ρ<sub>_w_</sub>
 (instructor))))).
 
 ### 2.6.9 | Equivalent Queries
 
There is often more than one way to write a query in relational algebra. Two queries are **equivalent** if they
give the same result on any database.
