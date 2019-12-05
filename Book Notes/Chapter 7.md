# Chapter 7: Relational Database Design

The goal of a relational database is to generate a gset of relation schemas that allows us to store information without redundancy, but 
also allowing quick retrieval. This is accomplished by designing schemas that are in an appropriate **normal form**.

## 7.1 | Features of Good Relational Designs
Suppose that we had started out when designing the university enterprise with the
schema _in_dep_, representing a natural join on the relations corresponding to _instructor_
and _department_: <br>
_in_dep_ (_ID, name, salary, dept name, building, budget_).

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-99.png)

Notice that we have to repeat the department information (“building” and “budget”) once for
each instructor in the department. This is redundant information. For example, the information about the Comp. Sci.
department (Taylor, 100000) is included in the tuples of instructors Katz, Srinivasan,
and Brandt.

Another problems is that a user might also want to update a deprtment's _budget_, which would require them to update every tuple with that department. Forgetting to update a tuple would cause data inconsistency.

Additionally, we cannot create a new department unless we add an instructor along with it. This is because tuples in the in dep table require values for _ID_, _name_, and _salary_.

### 7.7.1 | Decomposition
To avoid the problem that we have with the _in_dep_ relation, we must decompose it into two relations: _instructor_ and _department_.

Not all decompositions of schemas are helpful. Consider a decomposition of the schema _employee_ (_ID, name, street, city, salary_) into _employee1_ (_ID, name_) and _employee2_ (_name, street, city, salary_). Let us assume two employees, both named Kim have tuples in the original _employee_ relation:<br>
(57766, Kim, Main, Perryridge, 75000)<br>
(98776, Kim, North, Hampton, 67000)<br>.

Figure 7.3 shows these tuples, the resulting tuples using the schemas resulting from
the decomposition, and the result if we attempted to regenerate the original tuples using a natural join.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-100.png)

As we see in the figure, the two original tuples appear in the result
along with two new tuples that incorrectly mix data values pertaining to the two employees named Kim. We can indicate that a certain street, city, and salary pertain to someone named Kim, but we are unable to distinguish which of the Kims.

We would like to avoid such decompositions. We shall refer to such decompositions as being **lossy decompositions**, and, conversely, to those that are not as **lossless decompositions**.

### 7.7.2 | Lossless Decomposition

Let _R_ be a relation schema and let _R1_ and _R2_ form a decomposition of _R_; that is, _R_ = _R1_ ∪ _R2_. We say that the decomposition is a **lossless decomposition** if there is no loss of information by replacing _R_ with two relation
schemas _R1_ and _R2_.

In relational algebra given _r_ is an instance of the relation _R_, a lossless decomposition means that <br>
Π<sub>_R1_</sub> (_r_) ⋈ Π<sub>_R2_</sub> (_r_) = _r_.

Conversely, a decomposition is lossy when <br>
_r_ ⊂ Π<sub>_R1_</sub> (_r_) ⋈ Π<sub>_R2_</sub> (_r_)

## 7.2 | Decomposition Using Functional Dependencies

A database models a set of entities and relationships in the real world. There are usually
a variety of constraints (rules) on the data in the real world. For example, some of the
constraints that are expected to hold in a university database are:

1. Students and instructors are uniquely identified by their ID.
2. Each student and instructor has only one name.
3. Each instructor and student is (primarily) associated with only one department.
4. Each department has only one value for its budget, and only one associated building.

An instance of a relation that satisfies all such real-world constraints is called a
**legal instance** of the relation.

### 7.2.1 | Notational Conventions

- We use Greek letters for sets of attributes (e.g., α). We use an uppercase
Roman letter to refer to a relation schema. We use the notation _r_(_R_) to show that
the schema _R_ is for relation _r_.

- When a set of attributes is a superkey, we may denote it by _K_.

- We use a lowercase name (or single character) for relations.

- A relation, has a particular value at any given time; we refer to that as an instance
and use the term “instance of _r_.”

### 7.2.2 | Keys and Functional Dependencies

Real-world constraints can be represented formally as keys (superkeys, candidate keys, and primary keys) or as functional dependencies.

Whereas a superkey is a set of attributes that uniquely identifies an entire tuple, a
**functional dependency** allows us to express constraints that uniquely identify the values
of certain attributes. 

Consider a relation schema _r_(_R_), and let α ⊆ _R_ and β ⊆ _R_.
- Given an instance of _r_(_R_), we say that the instance **satisfies** the functional dependency α → β if for all pairs of tuples _t1_ and _t2_ in the instance such that _t1_[α] = _t2_[α], it is also the case that _t1_[β] = _t2_[β].
- We say that the functional dependency α → β **holds** on schema _r_(_R_) if, every legal
instance of _r_(_R_) satisfies the functional dependency.

The schema _in_dep_ (_ID, name, salary, dept name, building, budget_) has a functional dependency <br>
_dept_name_ → _budget_ <br>
which holds because for each department (identified by _dept_name_) there is a unique budget amount.

We denote the fact that the pair of attributes (_ID, dept_name_) forms a superkey for _in_dep_ by writing:<br>
_ID, dept_name_ → _name, salary, building, budget_.

We use functional dependencies in two ways:
1. To determine whether instances of relations satisfy a set of functional dependencies _F_.
2. To specify constraints on the set of legal relations.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-101.png)

Consider the above instance of relation _r_. Observe that _A_ → _C_ is satisfied but not _C_ → _A_. The two tuples with an _A_ value of _a1_ have a _C_ value of _c1_. Similarly, the two tuples with an _A_ value of _a2_ have a _C_ value of _c2_; thus, _A_ → _C_ is satisfied. However, _C_ → _A_ is not satisfied because the tuples with a _C_ value of _c2_ do not have matching _A_ values; two tuples have an _A_ value of _a2_ and one has a value of _a3_.

Some functional dependencies are said to be **trivial** because they are satisfied by all
relations. For example, _A_ → _A_ is satisfied by all relations involving attribute _A_. Similarly, _AB_ → _A_ is satisfied
by all relations involving attribute _A_. In general, a functional dependency of the form
α → β is trivial if β ⊆ α.

Given that a set of functional dependencies _F_ holds on a relation _r_(_R_), it may
be possible to infer that certain other functional dependencies must also hold on the
relation. For example, given a schema _r_(_A, B, C_), if functional dependencies _A_ → _B_ and
_B_ → _C_ hold on _r_, we can infer the functional dependency _A_ → _C_ must also hold on _r_.

We shall use the notation _F+_ to denote the **closure** of the set _F_, that is, the set of
all functional dependencies that can be inferred given the set _F_.

### 7.2.3 | Lossless Decomposition and Functional Dependencies

Let _R_, _R1_, _R2_, and _F_ be as above. _R1_ and _R2_ form a lossless decomposition of _R_ if at
least one of the following functional dependencies is in _F+_:

1. _R1_ ∩ _R2_ → _R1_
2. _R1_ ∩ _R2_ → _R2_

In other words, if _R1_ ∩ _R2_ forms a superkey for either _R1_ or _R2_, the decomposition of _R_ is
a lossless decomposition.

Consider the following schema and its decomposition: <br>
_in_dep_ (_ID, name, salary, dept_name, building, budget_) <br>
_instructor_ (_ID, name, dept_name, salary_) <br>
_department_ (_dept_name, building, budget_)

The instersection _instructor_ ∩ _department_ = _dept_name_, and dept_name → _dept_name, building, budget_, thus the lossless-decomposition rule is satisfied.

## 7.3 | Normal Forms
### 7.3.1 | Boyce-Codd Normal Form 
One of the more desirable normal forms that we can obtain is **Boyce–Codd normal form**
(BCNF). It eliminates all redundancy that can be discovered based on functional dependencies.

#### Definition
A relation schema _R_ is in BCNF with respect to a set _F_ of functional dependencies if,
for all functional dependencies in _F+_, at least one of the following holds:
1. α → β is a trivial functional dependency (i.e., β ⊆ α).
2. α is a superkey for schema _R_.

The schema _in_dep_ (_ID, name, salary, dept name, building, budget_) is not in BCNF because the functional dependency _dept_name_ → _budget_ holds on _in_dep_, but _dept_name_ is not a superkey (because a department may have a number of different instructors).

The two schema resulting from the decomposition of _in_dep_ are in BCNF, however: <br>
The _instructor_ schema is in BCNF because all of the nontrivial functional dependencies that hold, such as _ID_ → _name, dept name, salary_, have _ID_ on the left side, and _ID_ is a superkey.
Similarly for the _department_ schema. All nontrivial functional dependencies that hold, such as _dept_name_ → _building, budget_ have a superkey (_dept_name_) on the left side.

To decompose a schema into BCNF, first pick one nontrivial functional dependency α → β such that α is not a superkey for _R_. Next decompose _R_ into these two schemas:

1. (α ∪ β)
2. (R − (β − α))

In the case of _in_dep_, α = _dept_name_, β = {_building, budget_}, and _in_dep_ is replaced by:

1. (α ∪ β) = (_dept_name_, _building_, _budget_)
2. (_R_ − (β − α)) = (_ID, name, _dept_name_, salary_)

#### BCNF and Dependency Preservation
Testing the entire database for consistency constraints (e.g. functional dependencies) everytime it is updated is costly. If testing a functional dependency can be done by considering just one relation, then the cost of testing this constraint is low. In some
cases, decomposition into BCNF can prevent efficient testing of certain functional dependencies.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-102.png)

Take the above E-R diagram which specifies the constraint that “a student may have more than one advisor, but at most one corresponding 
to a given department.” The schema derived from the _dept_advisor_ relationship set is _dept_advisor_ (_s_ID_, _i_ID_, _dept_name_). 
Suppose we have the additional constraint that “an instructor can act as advisor for only a single department.” Then, the following functional dependencies hold on _dept_advisor_: <br>
_i_ID_ → _dept_name_ <br>
_s_ID_, _dept_name_ → _i_ID_

We see that _dept_advisor_ is not in BCNF because _i_ID_ is not a superkey. Following our rule for BCNF
decomposition, we get: <br>
(_s_ID, i_ID_) <br>
(_i_ID, dept_name_), <br>
which are both in BCNF.

Note, however, that in our BCNF design, there is no schema that includes all the
attributes appearing in the functional dependency _s_ID, dept_name_ → _i_ID_. Because of this, we say that our design is not **dependency preserving**.

### 7.3.2 | Third Normal Form
Because dependency preservation is usually considered desirable, we consider another normal form, weaker than
BCNF, that will allow us to preserve dependencies. That normal form is called the third
normal form.

A relation schema _R_ is in **third normal form** (3NF) with respect to a set _F_ of functional
dependencies if, for all functional dependencies in _F+_ of the form α → β, where α ⊆ R
and β ⊆ R, at least one of the following holds:

1. α → β is a trivial functional dependency
2. α is a superkey for _R_.
3. Each attribute _A_ in β − α is contained in a candidate key for _R_.

