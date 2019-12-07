# Chapter 15: Query Processing

**Query processing** refers to the range of activities involved in extracting data from a
database.

## 15.1 | Overview
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-107.png)

Three basic steps in query processing:
1. Parsing and translation.
2. Optimization.
3. Evaluation.

Before processing a query, the system must translate it to a machine-usable form based on the extended relational algebra. 
The first action is to translate a given query into its internal form. This translation process is similar to the work performed
by the parser of a compiler (syntax checking). Then, The system constructs a
parse-tree representation of the query, which it then translates into a relational-algebra
expression.

Queries can be translated to its relational-algebra form in several different ways.
For example, consider the query: <br>
select salary
from instructor
where salary < 75000;

This can be translated into either relational-algebra expression: <br>
σ<sub>salary<75000</sub> (Π<sub>salary</sub> (instructor)) <br>
Π<sub>salary</sub> (σ<sub>salary<75000</sub> (instructor))

Additionally, there are several algorithms to execute each relational algebra operation. For example, we can search for every tuple in instructor with a salary less than 75000, or we can use an index search if it's available.

Annotations may state the algorithm to be used for a specific operation or the particular index or indices to use. A relational-algebra operation annotated with instructions on how to evaluate it is called an **evaluation primitive**. A sequence of
primitive operations that can be used to evaluate a query is a **query-execution plan**.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-108.png)

The different ways that a query can be written can have different costs. **Query optimization** is where the system chooses the best execution plan.

### Section 15.2 was not covered in lecture.

## Statistical Information for Cost Estimation (from lecture notes)

To choose a query processing strategy, the DBMS may store the following information for each relation _r_:

- _n<sub>r</sub>_: The number of tuples in _r_.
- _b<sub>r</sub>_: The number of blocks containing tuples of _r_.
- _I<sub>r</sub>_: The size of a tuple in _r_.
- _f<sub>r</sub>_: The number of tuples of _r_ that fit into one block. Called the blocking factor.
- V(_A_, _r_): number of distinct values that appear in _r_ for attribute A; same as the size of ∏<sub>_A_</sub> (_r_).

If tuples of rare stored together physically in a file, then _f<sub>r</sub>_ = | n<sub>r</sub>_ / _f<sub>r</sub>_ |.

## Cartesian Product Size Estimation (from lecture notes)
Let _r_ and _s_ be relations we want to perform a Cartesian product on. Then, _r_ x _s_ has _n<sub>r</sub>_ *  _n<sub>s</sub>_  number of tuples with each tuple being (_I<sub>r</sub>_ + _I<sub>s</sub>_) bytes.

## Selection Size Estimation (from lecture notes)
For a selection query of the relational algebra form σ<sub>_A=v_</sub> (_r_), the estimated number of records that will satisfy the selection is _n<sub>r</sub>_ / V(_A_, _r_), assuming that the values of _A_ are uniformly distributed.

## Join Size Estimation (from lecture notes)
Let _R_ and _S_ be schemas we want to perform a join on.

There are four cases:
- If _R_ ∩ _S_ = ∅, then the size of _r_ ⋈ _s_ is the same as the size of _r_ x _s_ (aka _n<sub>r</sub>_ *  _n<sub>s</sub>_ ).
- If  _R_ ∩ _S_ = _K1_ and K1 is a key for _R_, then a tuple from _s_ will join with at most one tuple from _r_. Thus, the size of _r_ ⋈ _s_ is less than or equal to the size of _s_.
- If  _R_ ∩ _S_ = _K2_ and K2 is a key for _S_, then a tuple from _r_ will join with at most one tuple from _s_. Thus, the size of _r_ ⋈ _s_ is less than or equal to the size of _r_. 
- If  _R_ ∩ _S_ = {_A_} and _A_ is a not a key for _R_ nor _S_, then all tuples in _r_ will join with ( _n<sub>r</sub>_ * _n<sub>s</sub>_) / V(_A, s_) tuples in _s_. Thus, estimated size of _r_ ⋈ _s_ is ( _n<sub>r</sub>_ * _n<sub>s</sub>_ ) / V(_A, s_).
Similarly, the estimated size of _s_ ⋈ _r_ is ( _n<sub>r</sub>_ * _n<sub>s</sub>_) / V(_A, r_). This under the assumption that the values of _A_ are uniformly distributed.
