# Chapter 27: Formal-Relational Query Languages

## 27.1 | The Tuple Relational Calculus
A relational-algebra expression provides a sequence of procedures
that generates the answer to our query. The **tuple relational calculus**, by contrast, is a
nonprocedural query language that describes the desired information without giving a specific procedure to obtain that information.

A query in tuple relational calculus is represented as: <br>
{_t_ | _P(t)_}, <br>
which means the set of all tuples _t_ where the predicate _P(t)_ is true.

For notation, we use _t[A]_ to denote the value of tuple _t_ on attribute _A_, and we use _t_ ∈ _r_ to
denote that tuple _t_ is in relation _r_.

### 27.1.1 | Example Queries
- Find the _ID_, _name_, _dept_name_, _salary_ for instructors whose salary is greater than $80,000: <br>
{_t_ |  _t_ ∈ _instructor_ ∧ _t[salary]_ > 80000}.


Suppose that we want only the _ID_ attribute, rather than all attributes of the instructor relation.
To express this request, we use: <br>
∃ _t_ ∈ _r_ (_Q(t)_) <br>
