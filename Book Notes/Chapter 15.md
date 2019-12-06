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
