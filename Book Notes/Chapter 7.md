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
