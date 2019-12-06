# Chapter 15: Query Processing

**Query processing** refers to the range of activities involved in extracting data from a
database.

## 15.1 | Overview

Three basic steps in query processing:
1. Parsing and translation.
2. Optimization.
3. Evaluation.

Before processing a query, the system must translate it to a machine-usable form based on the extended relational algebra. 
The first action is to translate a given query into its internal form. This translation process is similar to the work performed
by the parser of a compiler (syntax checking). Then, The system constructs a
parse-tree representation of the query, which it then translates into a relational-algebra
expression.

