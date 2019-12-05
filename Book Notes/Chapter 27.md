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

### 27.1.2 | Formal Definition

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
- If _P1(s)_ is a formula containing a free tuple variable _s_, and _r_ is a relation, then
∃ _s_ ∈ _r_ (_P1(s)_) and ∀ _s_ ∈ _r_ (_P1(s)_) <br>
are also formulae.

### 27.1.3 | Safety of Expressions

We must prevent the situation where a tuple-relational-calculus expression may
generate an infinite relation.

Suppose that we write the expression {_t_ | ¬(_t_ ∈ _instructor_)}. There are infinitely many tuples that are not in _instructor_, and we wish to safeguard against these expressions.

The **domain** of a tuple relational formula _P_, denoted dom(_P_), is the set of all values referenced by _P_, as well as values that appear in a tuple of a relation mentioned in P.

- dom(_t_ ∈ _instructor_ ∧ _t[salary]_ > 80000) is the set containing 80000 as well as the set of all values appearing in any attribute of any tuple in the _instructor_ relation.

- dom(¬ (_t_ ∈ _instructor_)) is also the set of all values appearing in _instructor_, since the relation _instructor_ is mentioned in
the expression.

An expression {_t_ | _P(t)_} is **safe** if all values that appear in the result
are values from dom(_P_).

- The expression {_t_ |¬ (_t_ ∈ _instructor_)} is not safe. It is true that dom(¬ (_t_ ∈ _instructor_)) is the set of all values appearing in _instructor_. However, it is possible to have a tuple _t_ not in _instructor_ that contains values that do not appear
in _instructor_.

## 27.2 | The Domain Relational Calculus

A second form of relational calculus, called **domain relational calculus**, uses domain
variables that take on values from an attributes domain, rather than values for an entire
tuple

### 27.2.1 | Formal Definition

An expression in the domain relational calculus is of the form <br>
<_x1, x2, ..., xn_> | P(_x1, x2, ..,, xn_)} <br>
where _x1, x2, ..., xn_ represent domain variables and _P_ represents a formula composed of
atoms.

An atom in the domain relational calculus has one of the following forms:
- <_x1, x2, ..., xn_> ∈ _r_, where _r_ is a relation on n attributes and _x1, x2, ..., xn_ are
domain variables or domain constants.
- _x_ Θ _y_, where _x_ and _y_ are domain variables and Θ is a comparison operator (<, ≤,
=, ≠, >, ≥).
- _x_ Θ _c_, where _x_ is a domain variable, Θ is a comparison operator, and _c_ is a constant
in the domain of the attribute for which _x_ is a domain variable.

Formulae are built from atoms in the domain relational calculus in the same way as in the tuple relational calculus.

### 27.2.2 | Example Queries
- Find the instructor _ID_, _name_, _dept_name_, and _salary_ for instructors whose _salary_ is
greater than $80,000: <br>
**{ < _i, n, d, s_ > | < _i, n, d, s_ > ∈ _instructor_ ∧ _s_ > 80000}**.

- Find all instructor ID for instructors whose salary is greater than $80,000: <br>
**{ < _i_ > | ∃ _n, d, s_ (< _i, n, d, s_ > ∈ _instructor_ ∧ _s_ > 80000)}**.

- Find the names of all instructors in the Physics department together with the
course id of all courses they teach: <br>
**{ < _n, c_ > | ∃ _i, a, se, y_ (< _i, c, a, se, y_ > ∈ _teaches_ ∧ ∃ _d, s_ (< _i, n, d, s_ > ∈ _instructor_ ∧ _d_ = “Physics”))}**.

-  Find the set of all courses taught in the Fall 2017 semester, the Spring 2018
semester, or both: <br>
**{ < _c_ > | ∃ _a, s, y, b, r, t_ (< _c, a, s, y, b, r, t_ > ∈ _section_
∧ _s_ = “Fall” ∧ _y_ = “2017”) <br>
∨ ∃ _a, s, y, b, r, t_ (< _c, a, s, y, b, r, t_ > ∈ _section_
∧ _s_ = “Spring” ∧ _y_ = “2018”)}**.

- Find all students who have taken all courses offered in the Biology department: <br>
**{ < _i_ > | ∃ _n, d, tc_ (< _i, n, d, tc_ > ∈ _student_) ∧ <br>
∀ _ci, ti, dn, cr_ (< _ci, ti, dn, cr_ > ∈ _course_ ∧ _dn_ = “Biology” ⇒  ∃ _si, se, y, g_ (< _i, ci, si, se, y, g_ > ∈ _takes_ ))}**

### 27.2.3 | Safety of Expressions

An expression such as { < _i, n, d, s_ > | ¬(< _i, n, d, s_ > ∈ _instructor_)}
is unsafe, because it allows values in the result that are not in the domain of the expression.

For the domain relational calculus, we must be concerned also about the form of
formulae within “there exists” and “for all” clauses. 

We say that an expression { < _x1, x2, ..., xn_ > | _P_ (_x1, x2, ..., xn_)} is safe if all of the following hold:
1. All values that appear in tuples of the expression are values from dom(_P_).
2. For every “there exists” subformula of the form ∃ _x_ (_P1(x)_), the subformula is
true if and only if there is a value _x_ in dom(_P1_) such that _P1(x)_ is true.
3. For every “for all” subformula of the form ∀ _x_ (_P1(x)_), the subformula is true if
and only if _P1(x)_ is true for all values _x_ from dom(_P1_).

### Subsequent sections were not covered in lecture.
