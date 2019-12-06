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

Again consider the schema for the _dept_advisor_ relation, which has the
following functional dependencies: <br>
_i_ID_ → _dept_name_ <br>
_s_ID_, _dept_name_ → _i_ID_

Here, α = _i_ID_, β = _dept_name_, and β − α = _dept_name_. Since the functional dependency _s_ID_, _dept_name → _i_ID_ holds on 
_dept_advisor_, the attribute _dept_name_ is contained in a candidate key and, therefore, _dept_advisor_ is in 3NF.

## 7.4 | Functional Dependency Theory

### 7.4.1 | Closure of a Set of Functional Dependencies
A functional dependency _f_ on _R_ is **logically implied** by a set of functional dependencies _F_ on _R_ if every instance of a relation _r_(_R_) that satisfies _F_ also satisfies _f_. 

Suppose we are given a relation schema _r_(_A, B, C, G, H, I_) and the set of functional dependencies:<br>
_A_ → _B_ <br>
_A_ → _C_ <br>
_CG_ → _H_ <br>
_CG_ → _I_ <br>
_B_ → _H_. <br>
The functional dependency A → H is logically implied because _A_ → _B_ then _B_ → _H_.

The **closure** of F, denoted by F+, is the set of all functional dependencies logically implied by F.

We can use **Armstrong's axioms** to find logically implied functional dependencies. By applying these rules repeatedly, we can find all of F+, given F:

1. **Reflexivity rule**.  If α is a set of attributes and β ⊆ α, then α → β holds.
2. **Augmentation rule**. If α → β holds and γ is a set of attributes, then γα → γβ holds.
3. **Transitivity rule**. If α → β holds and β → γ holds, then α → γ holds.

To simplify matters further, we list additional rules:

4. **Union rule**. If α → β holds and α → γ holds, then α → βγ holds.
5. **Decomposition rule**. If α → βγ holds, then α → β holds and α → γ holds.
6. **Pseudotransitivity rule**. If α → β holds and γβ → δ holds, then αγ → δ holds.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-103.png)

### 7.4.2 | Closure of Attribute Sets

An attribute _B_ is **functionally determined** by α if α → _B_.

We call the set of all attributes functionally determined
by α under a set _F_ of functional dependencies the closure of α under _F_; we denote it by α+. Below is an algorithm to compute α+.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-104.png)

To illustrate how the algorithm works, we shall use it to compute (_AG_)+ with functional dependencies <br>
_A_ → _B_ <br>
_A_ → _C_ <br>
_CG_ → _H_ <br>
_CG_ → _I_ <br>
_B_ → _H_. <br>

1. We start with _result_ = _AG_.
2. _A_ → _B_ causes us to include _B_ in _result_. To see this fact, we observe that _A_ → _B_ is
in _F_. _A_ ⊆ _result_ (which is _AG_), so _result_ := _result_ ∪ _B_.
3. _A_ → _C_ causes _result_ to become _ABCG_.
4. _CG_ → _H_ causes _result_ to become _ABCGH_.
5. _CG_ → _I_ causes _result_ to become _ABCGHI_.

There are several uses of the attribute closure algorithm:
- If α+ contains all attributes in _R_, then α is a superkey.
- We can check if a functional dependency α → β holds (or, in other words, is in
F+), by checking if β ⊆ α+. That is, we compute α+, and
then check if it contains β.

## 7.5 | Algorithms for Decomposition Using Functional Dependencies

### 7.5.1 | BCNF Decomposition

#### Testing for BCNF
- To check if a nontrivial dependency α → β causes a violation of BCNF, compute
α+, and verify that it includes all attributes of _R_; that is, it is a superkey for _R_.

- It suffices to check only the dependencies in the given set F for violation of BCNF, rather than check all dependencies in F+.

#### BCNF Decomposition Algorithm
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-105.png)
 
If R is not in BCNF, we can decompose R into a collection of BCNF schemas R1, R2, …, Rn by the above algorithm.

As a example of the use of the BCNF decomposition algorithm, suppose we
have a database design using the class relation, whose schema is as shown below: <br>
_class_ (_course_id, title, dept_name, credits, sec_id, semester, year, building,
room_number, capacity, time slot_id_).

The set of functional dependencies that we need to hold on this schema are:<br>
_course_id_ → _title, dept_name, credits_ <br>
_building, room_number_ → _capacity_ <br>
_course_id, sec_id, semester, year_ → _building, room_number, time_slot_id_ 

A candidate key for this schema is {_course_id, sec_id, semester, year_}.

We can see that _class_ is not in BCNF because of the functional dependency _course_id_ → _title, dept_name, credits_;
_course_id_ is not a superkey.

Because of this, we replace class with two relations with the following schemas: <br>
_course_ (_course_id, title, dept_name, credits_) <br>
_class-1_ (_course_id, sec_id, semester, year, building, room_number, capacity, time_slot_id_).

We can see that _course_ is in BCNF because the only functional dependency on it is _course_id_ → _title, dept_name, credits_ and _course_id_ is a superkey.

Now look at _class-1_. This is not in BCNF because of the functional dependency _building, room_number_ → _capacity_; (_building, room_number_) is not a superkey.

Thus, we decompose _class-1_ into the two schema: <br>
_classroom_ (_building, room_number, capacity_) <br>
_section_ (_course_id, sec_id, semester, year, building, room_number, time_slot_id_), <br>
which are both in BCNF.

### This is where the lectures diverge from the book.
