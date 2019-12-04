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
**{_t_ |  _t_ ∈ _instructor_ ∧ _t[salary]_ > 80000}**.

Suppose that we want only the _ID_ attribute, rather than all attributes of the instructor relation.
To express this request, we use: <br>
∃ _t_ ∈ _r_ (_Q(t)_), <br>
which means "the exists a tuple _t_ in relation _r_ such that predicate _Q(t)_ is true."

- We can write the query “Find the instructor ID for each instructor with a salary greater than $80,000” as: <br>
**{_t_ | ∃ _s_ ∈ _instructor_ (_t[ID]_ = _s[ID]_ ∧ _s[salary]_ > 80000)} **.

- Consider the query “Find the names of all instructors whose department is in the Watson building.” We write the query as follows:<br>
**{_t_ | ∃ _s_ ∈ _instructor_ (_t[name]_ = _s[name]_ ∧ ∃ _u_ ∈ _department_ (_u[dept name]_ = _s[dept name]_ ∧ _u[building]_ = “Watson”))}**.

- To find the set of all courses taught in the Fall 2017 semester, the Spring 2018
semester, or both, we used the union operation in the relational algebra. In the tuple
relational calculus, we shall need two “there exists” clauses, connected by or (∨): <br>
**{_t_ | ∃ _s_ ∈ _section_ (_t[course id]_ = _s[course id]_)
∧ _s[semester]_ = “Fall” ∧ _s[year]_ = 2017)
∨ <br>
∃ _u_ ∈ section (_u[course id]_ = _t[course id]_)
∧ _u[semester]_ = “Spring” ∧ _u[year]_ = 2018)}**.

- If we now want only those course id values for courses that are offered in both the
Fall 2017 and Spring 2018 semesters, all we need to do is to change the or (∨) to and
(∧) in the preceding expression: <br>
**{_t_ | ∃ _s_ ∈ _section_ (_t[course id]_ = _s[course id]_)
∧ _s[semester]_ = “Fall” ∧ _s[year]_ = 2017)
∧ <br>
∃ _u_ ∈ section (_u[course id]_ = _t[course id]_)
∧ _u[semester]_ = “Spring” ∧ _u[year]_ = 2018)}**.

- Now consider the query “Find all the courses taught in the Fall 2017 semester but
not in Spring 2018 semester.” For this, we must use the not (¬) symbol: <br>
**{_t_ | ∃ _s_ ∈ _section_ (_t[course id]_ = _s[course id]_)
∧ _s[semester]_ = “Fall” ∧ _s[year]_ = 2017)
∧ <br>
¬∃ _u_ ∈ section (_u[course id]_ = _t[course id]_)
∧ _u[semester]_ = “Spring” ∧ _u[year]_ = 2018)}**.

- Now consider the query "Find all students who have taken all courses offered in the
Biology department." To write this query, we introduce
the “for all” construct, denoted by ∀. The notation: ∀ t ∈ r (Q(t)) means “Q is true for all tuples t in relation r.” <br>
**{_t_ | ∃ _r_ ∈ _student_ (_r[ID]_ = _t[ID]_) ∧ <br>
( ∀ _u_ ∈ _course_ (_u[dept name]_ = _“ Biology”_ ⇒ <br>
∃ _s_ ∈ _takes_ (_t[ID]_ = _s[ID]_ <br>
∧ _s[course id]_ = _u[course id]_))}**. <br>
In English, this means "“The set of all students (i.e., (_ID_) tuples _t_)
such that, for all tuples _u_ in the course relation, if the value of _u_ on attribute _dept_name_
is _’Biology’_, then there exists a tuple in the _takes_ relation that includes the student _ID_
and the _course_id_.

## 27.1.2 | Formal Definition

A tuple-relational-calculus expression is of
the form: <br>
{t|P(t)} <br>
where P is a **formula**.

A tuple variable is said to be a **free variable** unless it is quantified by a ∃ or ∀. Thus, in: <br>
_t_ ∈ _instructor_ ∧ ∃ _s_ ∈ _department_(_t[dept name]_ = _s[dept name]_) <br>
_t_ is a free variable. Tuple variable _s_ is said to be a **bound variable**.

A formula is built up out of **atoms**. An atom has one of the
following forms:

- _s_ ∈ _r_, where _s_ is a tuple variable and _r_ is a relation.
- _s[x]_ Θ _u[y]_, where _s_ and _u_ are tuple variables, _x_ is an attribute on which _s_ is defined,
_y_ is an attribute on which _u_ is defined, and Θ is a comparison operator (<, ≤, =,
≠, >, ≥).
- _s[x]_ Θ _c_, where _s_ is a tuple variable, _x_ is an attribute on which _s_ is defined, Θ is a
comparison operator, and _c_ is a constant in the domain of attribute _x_.

We build up formulae from atoms by using the following rules:

- An atom is a formula.
- If _P1_ is a formula, then so are ¬_P1_ and (_P1_).
- If _P1_ and _P2_ are formulae, then so are _P1_ ∨ _P2_, _P1_ ∧ _P2_, and _P1_ ⇒ _P2_.
