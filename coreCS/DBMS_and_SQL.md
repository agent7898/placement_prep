# DBMS + SQL — Placement Study Guide

> **How to use this file:** DBMS is the highest return-on-effort core subject, because it is tested in *two independent ways*: theory MCQs (normalization, ACID, keys, indexes) and **live SQL writing** (almost every technical interview asks you to write a query on the spot). Theory you can revise; SQL you must **type out**. Do not read the query sections — reproduce them from scratch.

**Priority legend:** 🔴 MUST KNOW · 🟠 HIGH · 🟡 IMPORTANT · 🟢 GOOD TO KNOW

---

## Table of contents

1. [DBMS fundamentals](#1-dbms-fundamentals-)
2. [Relational model and keys](#2-relational-model-and-keys-)
3. [ER modelling](#3-er-modelling-)
4. [Functional dependencies and normalization](#4-functional-dependencies-and-normalization-)
5. [Relational algebra](#5-relational-algebra-)
6. [SQL foundations](#6-sql-foundations-)
7. [Joins](#7-joins-)
8. [Aggregation and grouping](#8-aggregation-and-grouping-)
9. [Subqueries and CTEs](#9-subqueries-and-ctes-)
10. [Window functions](#10-window-functions-)
11. [The 12 SQL patterns that get asked](#11-the-12-sql-patterns-that-get-asked-)
12. [Indexes and query performance](#12-indexes-and-query-performance-)
13. [Transactions and ACID](#13-transactions-and-acid-)
14. [Concurrency control and isolation](#14-concurrency-control-and-isolation-)
15. [SQL vs NoSQL, CAP, MongoDB](#15-sql-vs-nosql-cap-and-mongodb-)
16. [Rapid revision sheet](#16-rapid-revision-sheet)
17. [Interview question bank with model answers](#17-interview-question-bank-with-model-answers)
18. [Traps that cost marks](#18-traps-that-cost-marks)
19. [Practice set with solutions](#19-practice-set-with-solutions)

---

## 1. DBMS fundamentals 🔴

A **DBMS** is software that stores, retrieves, and manages data while guaranteeing integrity, security, concurrency, and recovery.

### Why not just use files? 🔴 (a classic opener)

| Problem with file systems                          | How a DBMS solves it                       |
| -------------------------------------------------- | ------------------------------------------ |
| Data redundancy and inconsistency                  | Normalization + a single source of truth   |
| Difficult access — each query needs a new program | Declarative query language (SQL)           |
| Data isolation — scattered formats                | Unified schema and catalog                 |
| Integrity problems                                 | Constraints (PK, FK, CHECK, NOT NULL)      |
| Atomicity problems — partial updates on crash     | Transactions with rollback + recovery logs |
| Concurrent access anomalies                        | Concurrency control (locking, MVCC)        |
| Security problems                                  | Users, roles, GRANT/REVOKE, views          |

### Three-schema architecture 🟠

| Level                          | What it describes                                                            | Who cares               |
| ------------------------------ | ---------------------------------------------------------------------------- | ----------------------- |
| **External / View**      | What each user group sees (views)                                            | End users, applications |
| **Conceptual / Logical** | The whole database's logical structure — tables, relationships, constraints | DBA, designer           |
| **Internal / Physical**  | How data is actually stored — files, indexes, pages                         | Storage engine          |

**Data independence:** *Logical* data independence = changing the conceptual schema without changing external views (harder to achieve). *Physical* data independence = changing storage/indexes without changing the logical schema (routinely achieved).

### Key vocabulary

**Schema** = structure/blueprint (rarely changes). **Instance** = the data at a point in time (changes constantly). **Relation** = table · **Tuple** = row · **Attribute** = column · **Degree** = number of columns · **Cardinality** = number of rows · **Domain** = set of allowed values for an attribute.

### DBA responsibilities 🟢

Schema definition, storage/access-method decisions, security and authorization, backup and recovery, performance monitoring and tuning.

---

## 2. Relational model and keys 🔴

### Integrity constraints

| Constraint                      | Rule                                                              |
| ------------------------------- | ----------------------------------------------------------------- |
| **Domain**                | Values must belong to the attribute's domain/type                 |
| **Entity integrity**      | The primary key cannot be NULL                                    |
| **Referential integrity** | A foreign key must match an existing primary key value or be NULL |
| **Key**                   | Declared uniqueness must hold                                     |

**Referential actions on FK:** `ON DELETE CASCADE` (delete children too), `SET NULL`, `SET DEFAULT`, `RESTRICT`/`NO ACTION` (block the delete). Being able to argue *which* to choose (e.g. cascade for order-items, restrict for customers with orders) is what separates a good answer.

### The key hierarchy 🔴

| Key                     | Definition                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------ |
| **Super key**     | Any set of attributes that uniquely identifies a tuple (may contain extras)          |
| **Candidate key** | A **minimal** super key — remove any attribute and uniqueness breaks          |
| **Primary key**   | The candidate key chosen by the designer; unique + NOT NULL; only one per table      |
| **Alternate key** | Candidate keys not chosen as primary                                                 |
| **Composite key** | A key made of two or more attributes                                                 |
| **Foreign key**   | An attribute referencing another table's primary key; enforces referential integrity |
| **Surrogate key** | A system-generated meaningless key (auto-increment ID, UUID)                         |
| **Natural key**   | A key with real-world meaning (email, PAN)                                           |

> **Relationship to remember:** Every candidate key is a super key; every primary key is a candidate key. Not every super key is minimal.

**Example** — `Student(roll_no, email, phone, name, dept)`:

- Super keys: `{roll_no}`, `{roll_no, name}`, `{email, dept}`, …
- Candidate keys: `{roll_no}`, `{email}`, `{phone}`
- Primary key: `roll_no` (chosen); Alternate keys: `email`, `phone`

**PRIMARY KEY vs UNIQUE** 🔴: a primary key is unique **and NOT NULL**, one per table, and is typically the clustering key. A UNIQUE constraint allows NULLs (in most DBMSs, one NULL — SQL Server; multiple NULLs — PostgreSQL/MySQL) and you can have many per table.

### NULL semantics 🔴 (a favourite trap)

NULL means "unknown", not zero and not an empty string.

- `NULL = NULL` evaluates to **UNKNOWN**, not TRUE. Always use `IS NULL` / `IS NOT NULL`.
- Any arithmetic with NULL yields NULL: `5 + NULL = NULL`.
- **Aggregate functions ignore NULLs** — except `COUNT(*)`, which counts rows.
- `COUNT(column)` counts non-NULL values only. So `COUNT(*) ≠ COUNT(col)` when NULLs exist.
- `WHERE col NOT IN (subquery)` returns **no rows** if the subquery yields even one NULL. Use `NOT EXISTS` instead. This is one of the most common real-world SQL bugs.
- SQL uses **three-valued logic**: TRUE, FALSE, UNKNOWN.

---

## 3. ER modelling 🟠

### Components

| Element                  | Notation                  | Meaning                                                                 |
| ------------------------ | ------------------------- | ----------------------------------------------------------------------- |
| Entity                   | Rectangle                 | A real-world object (Student)                                           |
| **Weak entity**    | Double rectangle          | Cannot be identified without an owner entity (Dependent of an Employee) |
| Attribute                | Ellipse                   | Property                                                                |
| Key attribute            | Underlined ellipse        | Uniquely identifying                                                    |
| Composite attribute      | Ellipse with sub-ellipses | Name → first, last                                                     |
| Multivalued attribute    | Double ellipse            | Phone numbers                                                           |
| Derived attribute        | Dashed ellipse            | Age (from DOB)                                                          |
| Relationship             | Diamond                   | Association between entities                                            |
| Identifying relationship | Double diamond            | Links a weak entity to its owner                                        |

### Cardinality 🔴

| Type          | Example                 | How to implement in tables                                                      |
| ------------- | ----------------------- | ------------------------------------------------------------------------------- |
| **1:1** | Person ↔ Passport      | Put the FK (with a UNIQUE constraint) on either side; usually the optional side |
| **1:N** | Department → Employees | **FK on the "many" side** (employee.dept_id)                              |
| **M:N** | Students ↔ Courses     | **A separate junction/bridge table** with FKs to both, PK = the pair      |

> **The single most important ER→schema rule:** many-to-many relationships **always** require a third table. If an interviewer asks you to design a schema, this is where they check you.

**Participation:** *total* (every entity must participate — double line, maps to NOT NULL FK) vs *partial* (optional).

**Generalization / specialization / aggregation** 🟡 — bottom-up merging into a superclass, top-down splitting into subclasses, and treating a relationship as an entity, respectively.

### Worked schema design example — a small e-commerce system

```sql
customers(customer_id PK, name, email UNIQUE, created_at)
categories(category_id PK, name)
products(product_id PK, name, price, stock, category_id FK → categories)
orders(order_id PK, customer_id FK → customers, order_date, status, total_amount)
order_items(order_id FK, product_id FK, quantity, unit_price,
            PRIMARY KEY(order_id, product_id))          -- resolves the M:N
payments(payment_id PK, order_id FK UNIQUE, amount, method, paid_at)  -- 1:1 with order
```

Be ready to justify: why `unit_price` is stored on `order_items` (prices change over time — the order must capture the price at purchase time), why `total_amount` on orders is a deliberate denormalization (avoids recomputing on every read), and why the junction table's PK is composite.

---

## 4. Functional dependencies and normalization 🔴

### Functional dependency

`X → Y` means: for any two tuples with the same X, the Y values must also be the same. "X determines Y."

**Armstrong's axioms** 🟡

- **Reflexivity:** if Y ⊆ X then X → Y
- **Augmentation:** if X → Y then XZ → YZ
- **Transitivity:** if X → Y and Y → Z then X → Z
- Derived: **Union** (X→Y, X→Z ⇒ X→YZ), **Decomposition**, **Pseudo-transitivity**

**Attribute closure X⁺** — the set of all attributes determined by X. Algorithm: start with X; repeatedly, if some FD's left side is inside your set, add its right side; stop when nothing changes.
**X is a candidate key ⟺ X⁺ = all attributes, and no proper subset of X has that property.**

**Worked example:** R(A,B,C,D,E) with FDs `A→B`, `B→C`, `CD→E`.

- A⁺ = {A} → {A,B} → {A,B,C}. Not all attributes → A alone is not a key.
- AD⁺ = {A,D} → {A,D,B} → {A,D,B,C} → (CD→E) → {A,B,C,D,E} = **all** ✔
- Is any proper subset of AD a key? A⁺ ≠ all, D⁺ = {D} ≠ all. So **AD is a candidate key.**

**Dependency types:**

- **Partial dependency** — a non-prime attribute depends on *part* of a composite candidate key.
- **Transitive dependency** — a non-prime attribute depends on another non-prime attribute.
- **Prime attribute** — belongs to some candidate key. **Non-prime** — belongs to none.

### The normal forms 🔴

| NF             | Requirement                                                                          | Removes                                             |
| -------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------- |
| **1NF**  | All attributes atomic; no repeating groups or multi-valued cells                     | Multi-valued attributes                             |
| **2NF**  | 1NF + no **partial** dependency of a non-prime attribute on a candidate key    | Partial dependencies                                |
| **3NF**  | 2NF + no **transitive** dependency of a non-prime attribute on a candidate key | Transitive dependencies                             |
| **BCNF** | For every non-trivial FD `X → Y`, **X must be a super key**                 | Remaining anomalies from overlapping candidate keys |
| **4NF**  | BCNF + no non-trivial **multivalued** dependencies                             | Independent multivalued facts                       |
| **5NF**  | 4NF + no join dependency not implied by candidate keys                               | Join anomalies                                      |

**Formal 3NF alternative:** for every FD X→Y, either X is a super key **or** Y is a prime attribute. (This is why 3NF is achievable with both lossless join *and* dependency preservation, while BCNF sometimes is not.)

### Fully worked normalization example 🔴 — memorize this narrative

**Unnormalized:**

| student_id | student_name | courses  | dept | dept_head |
| ---------- | ------------ | -------- | ---- | --------- |
| 1          | Asha         | DBMS, OS | CSE  | Dr. Rao   |
| 2          | Bala         | DBMS     | ECE  | Dr. Iyer  |

**1NF** — make `courses` atomic (one row per course):

| student_id | student_name | course | dept | dept_head |
| ---------- | ------------ | ------ | ---- | --------- |
| 1          | Asha         | DBMS   | CSE  | Dr. Rao   |
| 1          | Asha         | OS     | CSE  | Dr. Rao   |
| 2          | Bala         | DBMS   | ECE  | Dr. Iyer  |

Candidate key is now `{student_id, course}`.

**2NF** — `student_name`, `dept`, `dept_head` depend only on `student_id`, which is *part* of the key → partial dependency. Split:

```
Student(student_id PK, student_name, dept, dept_head)
Enrollment(student_id, course, PK(student_id, course))
```

**3NF** — in `Student`, `student_id → dept → dept_head`: `dept_head` is a non-prime attribute determined by another non-prime attribute → transitive. Split:

```
Student(student_id PK, student_name, dept FK)
Department(dept PK, dept_head)
Enrollment(student_id, course)
```

**Why this matters (state the anomalies removed):**

- **Update anomaly** — changing the CSE head previously required updating many rows; now one.
- **Insertion anomaly** — you couldn't record a new department before any student joined it; now you can.
- **Deletion anomaly** — deleting the last CSE student previously erased the department entirely; now it doesn't.

### BCNF example 🟠

R(student, course, instructor) with FDs: `{student, course} → instructor` and `instructor → course` (each instructor teaches exactly one course).

- Candidate keys: `{student, course}` and `{student, instructor}`.
- `instructor → course`: instructor is **not** a super key → **violates BCNF**. But `course` **is** a prime attribute, so it satisfies **3NF**.
- Decompose to `R1(instructor, course)` and `R2(student, instructor)`. This is lossless but **not dependency preserving** — `{student,course} → instructor` can no longer be checked on a single table. That trade-off is exactly why 3NF is often used in practice.

**Lossless join decomposition:** decomposing R into R1 and R2 is lossless if `R1 ∩ R2 → R1` or `R1 ∩ R2 → R2` (the common attributes form a key of at least one piece).

### Denormalization 🔴

Deliberately reintroducing redundancy — storing `total_amount` on the order, keeping a `comment_count` on a post, duplicating a `customer_name` on an invoice — to avoid expensive joins on read-heavy paths.

**Say the trade-off explicitly:** normalization optimizes for write consistency and storage; denormalization optimizes for read performance at the cost of redundancy, larger writes, and the risk of inconsistency (which you then have to manage with triggers, application logic, or scheduled reconciliation). Interviewers reward this framing far more than "normalize everything."

---

## 5. Relational algebra 🟡

The theoretical basis of SQL. Appears in MCQs.

| Operator          | Symbol | Meaning                                       | SQL equivalent            |
| ----------------- | ------ | --------------------------------------------- | ------------------------- |
| Selection         | σ     | Filter rows                                   | `WHERE`                 |
| Projection        | π     | Choose columns (**removes duplicates**) | `SELECT DISTINCT`       |
| Union             | ∪     | Rows in either (union-compatible)             | `UNION`                 |
| Set difference    | −     | Rows in A not in B                            | `EXCEPT` / `MINUS`    |
| Cartesian product | ×     | Every pairing                                 | `CROSS JOIN`            |
| Rename            | ρ     | Rename relation/attributes                    | `AS`                    |
| Natural join      | ⋈     | Join on common attributes                     | `NATURAL JOIN`          |
| Intersection      | ∩     | Rows in both                                  | `INTERSECT`             |
| Division          | ÷     | "for all" queries                             | Correlated `NOT EXISTS` |

The first six (σ, π, ∪, −, ×, ρ) are the **basic/primitive** operators; the rest are derived.

**Example:** names of students in the CSE department →
`π_name ( σ_{dept='CSE'} (Student) )`

> **Projection removes duplicates (it's set-based); `SELECT` in SQL does not (it's multiset/bag-based).** That distinction is an MCQ favourite.

---

## 6. SQL foundations 🔴

### Command categories

| Category                      | Commands                                                  | Note               |
| ----------------------------- | --------------------------------------------------------- | ------------------ |
| **DDL** — Definition   | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` | Auto-committed     |
| **DML** — Manipulation | `INSERT`, `UPDATE`, `DELETE`                        | Can be rolled back |
| **DQL** — Query        | `SELECT`                                                | —                 |
| **DCL** — Control      | `GRANT`, `REVOKE`                                     | Permissions        |
| **TCL** — Transaction  | `COMMIT`, `ROLLBACK`, `SAVEPOINT`                   | —                 |

### DELETE vs TRUNCATE vs DROP 🔴 (asked constantly)

|                         | **DELETE**          | **TRUNCATE**         | **DROP**              |
| ----------------------- | ------------------------- | -------------------------- | --------------------------- |
| Type                    | DML                       | DDL                        | DDL                         |
| Removes                 | Selected rows (`WHERE`) | All rows                   | The whole table + structure |
| Rollback                | Yes                       | Generally no (auto-commit) | No                          |
| Speed                   | Slow (row-by-row, logged) | Fast (deallocates pages)   | Fast                        |
| Triggers fired          | Yes                       | No                         | No                          |
| Identity/auto-increment | Not reset                 | Reset                      | Gone                        |
| WHERE clause            | Supported                 | Not supported              | Not applicable              |

### Logical query processing order 🔴 — explains most SQL "why?" questions

```
1. FROM        (and JOINs — build the working set)
2. ON          (join condition)
3. WHERE       (filter individual rows)
4. GROUP BY    (form groups)
5. HAVING      (filter groups)
6. SELECT      (compute expressions, apply aliases)
7. DISTINCT
8. ORDER BY    (sort)
9. LIMIT/OFFSET
```

**Three consequences you should be able to state:**

1. **You cannot use a SELECT alias in WHERE** — WHERE runs before SELECT. (You *can* use it in `ORDER BY`, which runs after, and MySQL permits it in `HAVING`.)
2. **You cannot use an aggregate in WHERE** — grouping hasn't happened yet. Use `HAVING`.
3. **WHERE filters rows before grouping; HAVING filters groups after.** Filtering early in WHERE is also faster.

### Constraints

`NOT NULL` · `UNIQUE` · `PRIMARY KEY` · `FOREIGN KEY` · `CHECK (age >= 18)` · `DEFAULT` · `AUTO_INCREMENT`/`SERIAL`/`IDENTITY`.

### Operators worth knowing

`BETWEEN a AND b` (inclusive) · `IN (…)` · `LIKE` with `%` (any sequence) and `_` (single character) · `IS NULL` · `COALESCE(a, b, c)` (first non-null) · `NULLIF(a,b)` · `CASE WHEN … THEN … ELSE … END` · `CAST(x AS type)`.

**Set operators:** `UNION` (removes duplicates, so it sorts — slower), `UNION ALL` (keeps duplicates, faster — prefer it when you know there are none), `INTERSECT`, `EXCEPT`/`MINUS`. All require the same number of columns with compatible types.

### Views 🟠

```sql
CREATE VIEW active_customers AS
SELECT customer_id, name, email FROM customers WHERE status = 'active';
```

A view is a stored query — a **virtual table**. Benefits: simplify complex queries, restrict column/row access (security), provide a stable interface over a changing schema. Views are generally updatable only if simple (single table, no aggregation/DISTINCT/GROUP BY). A **materialized view** physically stores the result and must be refreshed — faster reads, stale data risk.

### Stored procedures, functions, triggers 🟡

- **Stored procedure** — precompiled block of SQL, can have IN/OUT params, may modify data, called with `CALL`.
- **Function** — must return a value, usable inside a query; traditionally cannot modify data.
- **Trigger** — code fired automatically on `INSERT`/`UPDATE`/`DELETE` (BEFORE/AFTER). Useful for audit logs and derived-column maintenance; dangerous because the logic is invisible to application developers.

---

## 7. Joins 🔴

Sample data:

**employees**

| emp_id | name  | dept_id | salary | manager_id |
| ------ | ----- | ------- | ------ | ---------- |
| 1      | Asha  | 10      | 90000  | NULL       |
| 2      | Bala  | 20      | 70000  | 1          |
| 3      | Chan  | 10      | 60000  | 1          |
| 4      | Divya | NULL    | 50000  | 2          |

**departments**

| dept_id | dept_name   |
| ------- | ----------- |
| 10      | Engineering |
| 20      | Sales       |
| 30      | Marketing   |

| Join                      | Returns                                       | On this data                                    |
| ------------------------- | --------------------------------------------- | ----------------------------------------------- |
| **INNER JOIN**      | Only matching rows on both sides              | Asha, Bala, Chan (Divya and Marketing excluded) |
| **LEFT JOIN**       | All left rows + matches; NULLs where no match | + Divya with NULL dept_name                     |
| **RIGHT JOIN**      | All right rows + matches                      | + Marketing with NULL employee                  |
| **FULL OUTER JOIN** | All rows from both sides                      | + Divya **and** Marketing                 |
| **CROSS JOIN**      | Cartesian product                             | 4 × 3 = 12 rows                                |
| **SELF JOIN**       | Table joined to itself                        | Employee ↔ their manager                       |

```sql
-- INNER
SELECT e.name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;

-- SELF JOIN: each employee with their manager's name
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;

-- ANTI-JOIN: departments with no employees
SELECT d.dept_name
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
```

> **The most common interview mistake:** putting the filter for the right table in `WHERE` instead of `ON` in a LEFT JOIN.
> `LEFT JOIN orders o ON c.id = o.customer_id WHERE o.status = 'paid'` silently becomes an INNER JOIN, because the WHERE clause discards the NULL-padded rows. Put the condition in `ON` if you want to preserve unmatched left rows.

**FULL OUTER JOIN in MySQL** (which lacks it): emulate with `LEFT JOIN … UNION … RIGHT JOIN`.

---

## 8. Aggregation and grouping 🔴

`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`. All ignore NULLs except `COUNT(*)`.

```sql
SELECT d.dept_name,
       COUNT(*)        AS headcount,
       AVG(e.salary)   AS avg_salary,
       MAX(e.salary)   AS top_salary
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE e.salary > 40000            -- filters ROWS, before grouping
GROUP BY d.dept_name
HAVING COUNT(*) > 1               -- filters GROUPS, after grouping
ORDER BY avg_salary DESC;
```

**Rule:** every column in `SELECT` must either appear in `GROUP BY` or be inside an aggregate function. (MySQL historically allowed violations; `ONLY_FULL_GROUP_BY` now enforces it. Never rely on the loose behaviour.)

**`COUNT(*)` vs `COUNT(col)` vs `COUNT(DISTINCT col)`** — all rows · non-NULL values of col · distinct non-NULL values. This distinction is asked directly.

---

## 9. Subqueries and CTEs 🔴

### Types

**Scalar subquery** — returns one value:

```sql
SELECT name, salary FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**Multi-row subquery** — with `IN`, `ANY`, `ALL`:

```sql
SELECT name FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments WHERE dept_name = 'Engineering');
```

**Correlated subquery** — references the outer query, so it re-evaluates per outer row (slower, but expressive):

```sql
-- employees earning more than their own department's average
SELECT e.name, e.salary
FROM employees e
WHERE e.salary > (SELECT AVG(e2.salary) FROM employees e2 WHERE e2.dept_id = e.dept_id);
```

**EXISTS** — stops at the first match; often faster than `IN` and **NULL-safe**:

```sql
SELECT c.name FROM customers c
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

> **`IN` vs `EXISTS` vs `JOIN`:** use `EXISTS` for existence checks over large subquery result sets and whenever NULLs may be present; `IN` reads well for small, known lists; a `JOIN` is right when you actually need columns from the other table. Modern optimizers often rewrite between them, so the correctness argument (NULL handling) matters more than the folklore performance argument.

### CTEs — Common Table Expressions 🔴

```sql
WITH dept_avg AS (
    SELECT dept_id, AVG(salary) AS avg_sal
    FROM employees
    GROUP BY dept_id
)
SELECT e.name, e.salary, d.avg_sal
FROM employees e
JOIN dept_avg d ON e.dept_id = d.dept_id
WHERE e.salary > d.avg_sal;
```

**Why CTEs:** readability, reuse of a named intermediate result, and — critically — **recursion**.

**Recursive CTE** (org hierarchy, category trees, graph traversal):

```sql
WITH RECURSIVE org_chart AS (
    SELECT emp_id, name, manager_id, 1 AS level        -- anchor member
    FROM employees WHERE manager_id IS NULL
    UNION ALL
    SELECT e.emp_id, e.name, e.manager_id, oc.level + 1 -- recursive member
    FROM employees e
    JOIN org_chart oc ON e.manager_id = oc.emp_id
)
SELECT * FROM org_chart ORDER BY level;
```

---

## 10. Window functions 🔴

A window function computes a value **across a set of related rows without collapsing them** — that's the difference from `GROUP BY`.

```sql
function() OVER (
    PARTITION BY col      -- optional: split into groups
    ORDER BY col          -- optional: ordering within each partition
    ROWS BETWEEN …        -- optional: frame
)
```

### Ranking functions — know the difference cold 🔴

Salaries 100, 90, 90, 80:

| salary | `ROW_NUMBER()` | `RANK()`        | `DENSE_RANK()`     |
| ------ | ---------------- | ----------------- | -------------------- |
| 100    | 1                | 1                 | 1                    |
| 90     | 2                | 2                 | 2                    |
| 90     | 3                | 2                 | 2                    |
| 80     | 4                | **4** (gap) | **3** (no gap) |

- **ROW_NUMBER()** — always unique, arbitrary tie-breaking.
- **RANK()** — ties share a rank, then it **skips**.
- **DENSE_RANK()** — ties share a rank, **no skipping**. Use this for "Nth highest salary".

### Other window functions

| Function                                         | Use                                                     |
| ------------------------------------------------ | ------------------------------------------------------- |
| `LAG(col, n)` / `LEAD(col, n)`               | Previous / next row's value — month-over-month changes |
| `FIRST_VALUE` / `LAST_VALUE` / `NTH_VALUE` | Boundary values within a partition                      |
| `NTILE(n)`                                     | Split rows into n buckets (quartiles)                   |
| `SUM/AVG/COUNT() OVER (…)`                    | Running totals and moving averages                      |

```sql
-- Running total of revenue by date
SELECT order_date, amount,
       SUM(amount) OVER (ORDER BY order_date
                         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM orders;

-- Month-over-month change
SELECT month, revenue,
       LAG(revenue) OVER (ORDER BY month)                       AS prev_revenue,
       revenue - LAG(revenue) OVER (ORDER BY month)             AS delta
FROM monthly_revenue;

-- 3-day moving average
SELECT order_date, amount,
       AVG(amount) OVER (ORDER BY order_date
                         ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg_3d
FROM orders;
```

> Window functions are evaluated **after** `WHERE`/`GROUP BY`/`HAVING` but **before** `ORDER BY`. That means **you cannot filter on a window function in `WHERE`** — wrap the query in a subquery or CTE and filter outside. This is exactly why the "Top N per group" pattern below needs a subquery.

---

## 11. The 12 SQL patterns that get asked 🔴

Learn these until you can write them without thinking. They cover the overwhelming majority of interview SQL.

### 1. Nth highest salary (three ways)

```sql
-- (a) Subquery — 2nd highest
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- (b) LIMIT/OFFSET — Nth highest (distinct)
SELECT DISTINCT salary FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;          -- OFFSET N-1

-- (c) DENSE_RANK — the general, correct-with-ties answer
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
) t
WHERE rnk = 2;
```

Say why (c) is preferred: it handles duplicate salaries correctly and generalizes to any N. Method (a) returns NULL when there is no second distinct salary — mention that edge case, because interviewers look for it.

### 2. Top N per group (department-wise top 2 earners)

```sql
SELECT dept_id, name, salary
FROM (
    SELECT dept_id, name, salary,
           DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk
    FROM employees
) t
WHERE rnk <= 2;
```

### 3. Find duplicates

```sql
SELECT email, COUNT(*) AS cnt
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

### 4. Delete duplicates, keeping one

```sql
DELETE FROM users
WHERE id NOT IN (SELECT min_id FROM (
    SELECT MIN(id) AS min_id FROM users GROUP BY email
) t);
-- (the extra derived table is needed in MySQL, which won't read the same table it deletes from)

-- Cleaner where supported:
WITH ranked AS (
    SELECT id, ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn FROM users
)
DELETE FROM users WHERE id IN (SELECT id FROM ranked WHERE rn > 1);
```

### 5. Employees earning more than their manager

```sql
SELECT e.name AS employee
FROM employees e
JOIN employees m ON e.manager_id = m.emp_id
WHERE e.salary > m.salary;
```

### 6. Customers who never ordered

```sql
SELECT c.name FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
-- or:  WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id)
```

### 7. Department-wise average, only above the company average

```sql
SELECT dept_id, AVG(salary) AS avg_sal
FROM employees
GROUP BY dept_id
HAVING AVG(salary) > (SELECT AVG(salary) FROM employees);
```

### 8. Running total / cumulative sum

```sql
SELECT order_date, amount,
       SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM orders;
```

### 9. Month-over-month growth %

```sql
SELECT month, revenue,
       ROUND(100.0 * (revenue - LAG(revenue) OVER (ORDER BY month))
                   / LAG(revenue) OVER (ORDER BY month), 2) AS growth_pct
FROM monthly_revenue;
```

### 10. Pivot with conditional aggregation

```sql
SELECT dept_id,
       SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
       SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count
FROM employees
GROUP BY dept_id;
```

### 11. Consecutive records (e.g. 3 consecutive logins)

```sql
SELECT DISTINCT l1.user_id
FROM logins l1
JOIN logins l2 ON l1.user_id = l2.user_id AND l2.day = l1.day + 1
JOIN logins l3 ON l1.user_id = l3.user_id AND l3.day = l1.day + 2;
-- or with LAG/LEAD, or the "day - ROW_NUMBER()" gaps-and-islands trick
```

### 12. Second-highest per group, plus the "gaps and islands" idea

```sql
-- islands: group consecutive dates by (date - row_number) being constant
SELECT user_id, MIN(day) AS streak_start, MAX(day) AS streak_end, COUNT(*) AS streak_len
FROM (
    SELECT user_id, day,
           day - ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY day) AS grp
    FROM logins
) t
GROUP BY user_id, grp;
```

---

## 12. Indexes and query performance 🔴

An **index** is an auxiliary data structure that speeds up lookups — conceptually a sorted map from column value → row location. Without one, the DB does a **full table scan**, O(n).

### Structure

Most relational indexes are **B+ trees**: balanced, high fan-out (so a lookup is 3–4 disk reads even for millions of rows), and **all data lives in the leaves, which are linked** — which is exactly why B+ trees are excellent for **range queries** (`BETWEEN`, `>`, `ORDER BY`) as well as equality. **Hash indexes** are O(1) for equality but useless for ranges and ordering.

### Clustered vs non-clustered 🔴

| **Clustered**                                                                    | **Non-clustered**                                       |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Determines the **physical/logical ordering of rows**; the table *is* the index | A separate structure holding key → row pointer               |
| **One per table** (usually the primary key)                                      | Many per table                                                |
| No extra lookup — leaf pages contain the actual rows                                  | May need a second lookup to fetch the row ("bookmark lookup") |
| Excellent for range scans on the key                                                   | Excellent for selective point lookups                         |

In InnoDB (MySQL), the primary key is the clustered index and every secondary index stores the PK value — which is why a large PK bloats every index, and why an auto-increment PK avoids page splits.

### Composite indexes and the leftmost-prefix rule 🔴

An index on `(a, b, c)` can serve queries filtering on `a`, `(a,b)`, or `(a,b,c)` — but **not** on `b` alone or `(b,c)`. Order the columns by selectivity and by how they're queried. This is a favourite interview question.

**Covering index** — an index containing every column the query needs, so the DB never touches the table ("index-only scan").

### When indexes hurt 🔴

- Every `INSERT`/`UPDATE`/`DELETE` must maintain every index → slower writes.
- Storage overhead.
- On **low-cardinality** columns (a `gender` flag), the optimizer will often ignore the index because a scan is cheaper.
- Too many indexes confuse the optimizer and waste memory.

### Things that prevent index use (very practical, very askable)

```sql
WHERE YEAR(created_at) = 2024      -- ✘ function on the column kills the index
WHERE created_at >= '2024-01-01' AND created_at < '2025-01-01'   -- ✔

WHERE name LIKE '%son'             -- ✘ leading wildcard
WHERE name LIKE 'son%'             -- ✔

WHERE user_id = '123'              -- ✘ implicit type conversion on an INT column
WHERE status != 'active'           -- ✘ negation is rarely selective
```

### Query optimization checklist 🟠

1. Run `EXPLAIN` / `EXPLAIN ANALYZE` and read the access type — look for `ALL` (full scan) vs `ref`/`range`/`const`.
2. Index the columns in `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`.
3. `SELECT` only the columns you need — never `SELECT *` in production code.
4. Eliminate the **N+1 query problem** (a loop issuing one query per row) — replace with a single join or an `IN` batch.
5. Prefer `UNION ALL` over `UNION` where duplicates are impossible.
6. Paginate large result sets; for deep pagination prefer keyset pagination (`WHERE id > :last_id LIMIT 20`) over huge `OFFSET`s.
7. Avoid correlated subqueries in the SELECT list over large tables — rewrite as joins.
8. Keep statistics fresh so the optimizer chooses well.

### Sharding, partitioning, replication 🟡

- **Vertical partitioning** — split columns across tables. **Horizontal partitioning** — split rows by range/hash within one DB.
- **Sharding** — horizontal partitioning across **different servers**. Scales writes; makes cross-shard joins and transactions painful.
- **Replication** — copies of the data. Primary–replica: writes to the primary, reads from replicas → read scaling + failover, at the cost of **replication lag** (a read right after a write may see stale data).

---

## 13. Transactions and ACID 🔴

A **transaction** is a logical unit of work: all of it happens, or none of it does.

```sql
BEGIN;
  UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
  UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT;   -- or ROLLBACK;
```

### ACID 🔴 — explain each with the transfer example

| Property              | Meaning                                                                                             | In the transfer example                                                |
| --------------------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Atomicity**   | All or nothing                                                                                      | You can't debit A without crediting B; a crash mid-way rolls back      |
| **Consistency** | Takes the DB from one valid state to another, respecting all constraints                            | Total money across accounts is unchanged; no constraint is violated    |
| **Isolation**   | Concurrent transactions don't corrupt each other; the result is as if they ran in some serial order | Another transfer running simultaneously can't read the half-done state |
| **Durability**  | Once committed, it survives crashes                                                                 | After COMMIT, a power failure cannot lose the transfer                 |

**How they're implemented** (this is the follow-up question):

- Atomicity & Durability → **write-ahead logging (WAL)**: log the change before applying it, so recovery can **undo** uncommitted work and **redo** committed work.
- Isolation → locking (2PL) or **MVCC**.
- Consistency → constraints + the other three, plus correct application logic.

### Transaction states

Active → Partially Committed → **Committed**, or Active → Failed → **Aborted** (rolled back).

---

## 14. Concurrency control and isolation 🔴

### The concurrency anomalies 🔴 — memorize the definitions precisely

| Anomaly                       | What happens                                                                                                                  |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **Dirty read**          | T1 reads data that T2 wrote but **has not committed**; T2 then rolls back, so T1 read something that never existed      |
| **Non-repeatable read** | T1 reads a row twice and gets **different values**, because T2 updated and committed in between                         |
| **Phantom read**        | T1 runs the same **range query** twice and the **set of rows changes**, because T2 inserted/deleted matching rows |
| **Lost update**         | Two transactions read the same value, both modify it, and the second overwrites the first                                     |

> **Non-repeatable read vs phantom read** is the distinction people fumble: non-repeatable = *the same row's value* changed; phantom = *new rows appeared/disappeared* in the same query's result set.

### Isolation levels 🔴

| Level                      | Dirty read   | Non-repeatable read | Phantom read | Cost                                      |
| -------------------------- | ------------ | ------------------- | ------------ | ----------------------------------------- |
| **READ UNCOMMITTED** | ✔ possible  | ✔ possible         | ✔ possible  | Fastest, unsafe                           |
| **READ COMMITTED**   | ✘ prevented | ✔ possible         | ✔ possible  | Default in PostgreSQL, Oracle, SQL Server |
| **REPEATABLE READ**  | ✘           | ✘ prevented        | ✔ possible* | Default in MySQL InnoDB                   |
| **SERIALIZABLE**     | ✘           | ✘                  | ✘ prevented | Slowest, strongest                        |

\* MySQL's InnoDB actually prevents phantoms at REPEATABLE READ using **next-key locking** — a good detail to mention, along with the caveat that the SQL standard doesn't require it.

**The one-line summary:** isolation levels trade **concurrency/throughput** against **consistency guarantees**. Pick the weakest level that is still correct for the workload.

### Locking 🟠

- **Shared (S) lock** — read lock; multiple readers may hold it simultaneously.
- **Exclusive (X) lock** — write lock; incompatible with everything else.
- **Granularity** — row-level (high concurrency, more overhead) vs page vs table-level.

**Two-Phase Locking (2PL)** — every transaction has a **growing phase** (acquires locks, releases none) and a **shrinking phase** (releases locks, acquires none). **2PL guarantees conflict-serializability**, but can still cause cascading rollbacks; **Strict 2PL** (hold all exclusive locks until commit) prevents that and is what real systems use.

**Deadlock in databases** — T1 holds A and wants B; T2 holds B and wants A. Handling: **timeout**, **wait-for graph detection** (the DB picks a victim and rolls it back — your application must be prepared to retry), or prevention schemes like **wait-die** / **wound-wait** using transaction timestamps. The practical advice: **always acquire locks in a consistent order** (e.g. by ascending primary key) and keep transactions short.

### MVCC — Multi-Version Concurrency Control 🟠

Instead of blocking readers, the DB keeps multiple versions of each row; each transaction sees a consistent snapshot as of its start. **Readers never block writers and writers never block readers** — the key advantage. Used by PostgreSQL and InnoDB. Cost: version storage and periodic cleanup (`VACUUM`).

### Optimistic vs pessimistic concurrency 🟡

- **Pessimistic** — lock first, assume conflict (`SELECT … FOR UPDATE`). Good under high contention.
- **Optimistic** — don't lock; on write, verify nothing changed (a `version` column / `WHERE version = :v`). Good under low contention; the loser retries.

### Schedules 🟡

A schedule is **serializable** if it's equivalent to some serial execution. **Conflict serializability** is checked with a **precedence graph** — serializable ⟺ the graph is **acyclic**. Also know **recoverable** schedules (a transaction commits only after the ones it read from commit) and **cascadeless** schedules (only read committed data).

---

## 15. SQL vs NoSQL, CAP, and MongoDB 🔴

### SQL vs NoSQL

| SQL (relational)                                    | NoSQL                                                          |
| --------------------------------------------------- | -------------------------------------------------------------- |
| Fixed, predefined schema                            | Flexible/dynamic schema                                        |
| Tables, rows, columns                               | Documents, key-value, wide-column, graph                       |
| Vertical scaling primarily (though sharding exists) | Designed for horizontal scaling                                |
| Strong ACID guarantees                              | Varies — often eventual consistency (many now offer ACID too) |
| Joins are first-class                               | Joins limited; data is often denormalized/embedded             |
| Great for complex queries and relationships         | Great for high-volume, evolving, or semi-structured data       |
| MySQL, PostgreSQL, Oracle, SQL Server               | MongoDB, Cassandra, Redis, DynamoDB, Neo4j                     |

**NoSQL families:** document (MongoDB), key-value (Redis, DynamoDB), wide-column (Cassandra, HBase), graph (Neo4j).

> **Do not say** "NoSQL is faster" or "NoSQL has no transactions." MongoDB has supported multi-document ACID transactions since 4.0. The honest answer is about **data model fit and access patterns**, not speed.

### CAP theorem 🔴

In a **distributed** system you can guarantee at most two of:

- **Consistency** — every read sees the most recent write.
- **Availability** — every request gets a (non-error) response.
- **Partition tolerance** — the system keeps working despite dropped messages between nodes.

**The correct framing:** network partitions *will* happen, so **P is not optional**. The real choice is **CP vs AP during a partition**: refuse requests to stay consistent (CP — HBase, MongoDB in its default config), or serve possibly-stale data to stay available (AP — Cassandra, DynamoDB). Traditional single-node RDBMSs are CA only because they aren't partitioned.

**BASE** (the NoSQL counterpart to ACID): **B**asically **A**vailable, **S**oft state, **E**ventual consistency.

### MongoDB 🟠

| SQL term    | MongoDB term                               |
| ----------- | ------------------------------------------ |
| Database    | Database                                   |
| Table       | **Collection**                       |
| Row         | **Document** (BSON)                  |
| Column      | **Field**                            |
| Primary key | `_id` (an **ObjectId** by default) |
| Join        | `$lookup` (aggregation stage)            |

**BSON** = binary JSON — adds types JSON lacks (Date, ObjectId, Decimal128, binary) and is faster to traverse.
**ObjectId** — 12 bytes: 4-byte timestamp + 5-byte random value + 3-byte counter. It's roughly time-sortable, which is a genuinely useful property.

**CRUD:**

```javascript
db.users.insertOne({ name: "Asha", age: 22, skills: ["Java", "SQL"] })
db.users.find({ age: { $gt: 20 } }).sort({ age: -1 }).limit(5)
db.users.updateOne({ name: "Asha" }, { $set: { age: 23 }, $push: { skills: "React" } })
db.users.deleteOne({ name: "Asha" })
db.users.createIndex({ email: 1 }, { unique: true })
```

**Aggregation pipeline:**

```javascript
db.orders.aggregate([
  { $match:  { status: "completed" } },                              // WHERE
  { $group:  { _id: "$customerId", total: { $sum: "$amount" } } },   // GROUP BY
  { $sort:   { total: -1 } },                                        // ORDER BY
  { $limit:  10 }                                                    // LIMIT
])
```

**Embedding vs referencing** 🔴 — the core MongoDB design decision:

| Embed when                                       | Reference when                     |
| ------------------------------------------------ | ---------------------------------- |
| Data is always read together                     | Data is queried independently      |
| The relationship is one-to-few                   | One-to-many with unbounded growth  |
| The child has no independent lifecycle           | The child is shared across parents |
| e.g. a user's addresses inside the user document | e.g. orders referencing a product  |

Watch out for the **16 MB document size limit** and for unbounded arrays (the classic anti-pattern: embedding every comment inside a post document).

**"Why did you choose MongoDB?" (the project-defence trap)** — a strong answer connects **data model → access patterns → consistency needs → scaling plan → alternatives considered**. For example: "Our documents were self-contained and schema-evolving, we read whole aggregates by ID far more often than we joined across them, we didn't need cross-entity transactions, and horizontal scaling was on the roadmap. We considered PostgreSQL with JSONB and would have chosen it if we'd needed relational reporting." Never say "we needed a database."

---

## 16. Rapid revision sheet

| #  | Question                                | One-line answer                                                                                           |
| -- | --------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| 1  | Super vs candidate vs primary key       | Any unique set → minimal unique set → the chosen one, NOT NULL                                          |
| 2  | Why normalize                           | Remove redundancy and update/insert/delete anomalies                                                      |
| 3  | 1NF / 2NF / 3NF                         | Atomic values / no partial dependency / no transitive dependency                                          |
| 4  | BCNF                                    | Every determinant must be a super key                                                                     |
| 5  | 3NF vs BCNF                             | 3NF also allows prime attributes on the right side; BCNF is stricter but may lose dependency preservation |
| 6  | DELETE vs TRUNCATE                      | DML, rollbackable, WHERE allowed vs DDL, fast, all rows, no rollback                                      |
| 7  | WHERE vs HAVING                         | Filters rows before grouping vs filters groups after                                                      |
| 8  | Query execution order                   | FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT                                        |
| 9  | INNER vs LEFT JOIN                      | Matches only vs all left rows with NULLs for non-matches                                                  |
| 10 | RANK vs DENSE_RANK vs ROW_NUMBER        | Gaps on ties / no gaps / always unique                                                                    |
| 11 | ACID                                    | Atomicity, Consistency, Isolation, Durability                                                             |
| 12 | Dirty vs non-repeatable vs phantom read | Uncommitted data / same row changed / row set changed                                                     |
| 13 | Isolation levels                        | Read Uncommitted < Read Committed < Repeatable Read < Serializable                                        |
| 14 | Clustered vs non-clustered index        | Defines physical row order, one per table vs separate structure, many allowed                             |
| 15 | When indexes hurt                       | Write-heavy tables, low-cardinality columns, too many indexes                                             |
| 16 | Leftmost prefix rule                    | Index (a,b,c) serves a, ab, abc — not b or bc alone                                                      |
| 17 | CAP                                     | Under a partition, choose consistency or availability                                                     |
| 18 | SQL vs NoSQL                            | Fixed schema + joins + ACID vs flexible schema + horizontal scale                                         |
| 19 | Embed vs reference in MongoDB           | Read together and bounded vs independent/shared/unbounded                                                 |
| 20 | View                                    | A stored query acting as a virtual table                                                                  |

---

## 17. Interview question bank with model answers

**Q: Write a query for the 2nd highest salary. Now make it work for the Nth.**
Show the `DENSE_RANK()` version and explain that it handles ties and generalizes by changing `rnk = N`. Mention the `LIMIT 1 OFFSET N-1` alternative and the NULL edge case when fewer than N distinct salaries exist.

**Q: Your query got slow after the table grew to 10 million rows. What do you do?**
Run `EXPLAIN` first — never guess. Look for a full table scan, check whether the WHERE/JOIN columns are indexed, verify no function is applied to an indexed column, check whether the join order and statistics are sane, look for N+1 patterns from the application, reduce the selected columns, and consider a covering index or partitioning if the table is genuinely huge. Then measure again.

**Q: When would you deliberately break normalization?**
On read-heavy analytical or dashboard paths where joins dominate the cost, or when a value is expensive to recompute (order totals, comment counts). I'd add the redundancy, then keep it correct with either application-level updates in the same transaction, a trigger, or a periodic reconciliation job — and I'd document that the column is derived.

**Q: Two users book the last seat simultaneously. How do you prevent double booking?**
Wrap it in a transaction and either (a) pessimistically lock the row: `SELECT … FOR UPDATE` on the seat before checking availability, or (b) optimistically use a conditional update: `UPDATE seats SET status='booked' WHERE seat_id=? AND status='available'` and check the affected row count — if it's 0, someone beat you. A unique constraint on `(show_id, seat_id)` gives a final database-level guarantee.

**Q: Difference between `WHERE` and `ON` in a LEFT JOIN?**
`ON` decides which rows match during the join; `WHERE` filters the result afterwards. Putting a right-table condition in `WHERE` removes the NULL-padded rows and silently converts the LEFT JOIN into an INNER JOIN.

**Q: What does an index actually cost?**
Slower writes (every insert/update/delete maintains it), disk space, and memory in the buffer pool. It also can mislead the optimizer if the column is low-cardinality. Indexes are a read/write trade-off, not free speed.

**Q: Explain isolation levels using a bank example.**
At READ UNCOMMITTED you could see a balance mid-transfer that gets rolled back. At READ COMMITTED you only see committed balances, but reading twice in one report can give different numbers. At REPEATABLE READ your reads of the same row are stable, but a new transaction's inserts can still add rows to a range query. SERIALIZABLE makes the whole report behave as if nothing else ran concurrently — at a throughput cost.

**Q: What is the N+1 query problem?**
Fetching a list of N parents with one query, then issuing one query per parent to fetch children — 1 + N queries. Fix with a single JOIN, an `IN (…)` batch fetch, or the ORM's eager-loading option.

**Q: How do you design a schema for a system with users, posts, likes and comments?**
`users(id PK, …)`, `posts(id PK, user_id FK, content, created_at)`, `comments(id PK, post_id FK, user_id FK, content, created_at)`, `likes(user_id FK, post_id FK, PRIMARY KEY(user_id, post_id))`. Note that the likes table's composite PK enforces "one like per user per post" at the database level, and that you'd index `posts(user_id, created_at DESC)` for the feed query, and probably denormalize a `like_count` onto posts.

---

## 18. Traps that cost marks

1. **`NULL = NULL` is not TRUE.** Use `IS NULL`.
2. **`NOT IN` with a NULL in the subquery returns nothing.** Use `NOT EXISTS`.
3. **`COUNT(*)` counts rows; `COUNT(col)` skips NULLs.**
4. **You cannot use a SELECT alias in `WHERE`** (execution order), nor an aggregate.
5. **A right-table condition in `WHERE` turns a LEFT JOIN into an INNER JOIN.**
6. **`TRUNCATE` cannot be rolled back** in most systems and won't fire triggers.
7. **`RANK()` skips numbers on ties; `DENSE_RANK()` does not.** Use DENSE_RANK for "Nth highest".
8. **Every candidate key is a super key, not the reverse.**
9. **3NF ≠ BCNF.** A table can be in 3NF and still violate BCNF (prime attribute on the right side).
10. **Normalization does not always improve performance** — it usually adds joins.
11. **An index does not speed up writes** — it slows them.
12. **A function on an indexed column disables the index.**
13. **Composite index `(a,b)` doesn't help a query filtering only on `b`.**
14. **`UNION` deduplicates (and therefore sorts); `UNION ALL` doesn't.**
15. **CAP is about behaviour during a partition**, not a permanent "pick two."
16. **MongoDB does support ACID transactions** — don't state otherwise.
17. **A view doesn't store data** (unless it's materialized).
18. **`DISTINCT` is not free** — it usually forces a sort or hash.
19. **DDL statements auto-commit**, ending any open transaction.
20. **Deadlocks are normal in databases** — your application must retry, not just log.

---

## 19. Practice set with solutions

Use these tables:

```
employees(emp_id, name, dept_id, salary, manager_id, hire_date)
departments(dept_id, dept_name)
orders(order_id, customer_id, order_date, amount, status)
customers(customer_id, name, city, signup_date)
```

**Q1.** Third-highest distinct salary per department.

<details><summary>Solution</summary>

```sql
SELECT dept_id, salary
FROM (
    SELECT dept_id, salary,
           DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk
    FROM employees
) t
WHERE rnk = 3;
```

</details>

**Q2.** Departments where the average salary exceeds the company-wide average, showing headcount.

<details><summary>Solution</summary>

```sql
SELECT d.dept_name, COUNT(*) AS headcount, AVG(e.salary) AS avg_sal
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name
HAVING AVG(e.salary) > (SELECT AVG(salary) FROM employees);
```

</details>

**Q3.** Customers who placed orders in 2024 but none in 2025.

<details><summary>Solution</summary>

```sql
SELECT DISTINCT c.customer_id, c.name
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE YEAR(o.order_date) = 2024
  AND NOT EXISTS (
      SELECT 1 FROM orders o2
      WHERE o2.customer_id = c.customer_id
        AND o2.order_date >= '2025-01-01' AND o2.order_date < '2026-01-01'
  );
```

Note: prefer range predicates over `YEAR()` on indexed columns in production.

</details>

**Q4.** Monthly revenue with month-over-month growth percentage.

<details><summary>Solution</summary>

```sql
WITH monthly AS (
    SELECT DATE_FORMAT(order_date, '%Y-%m') AS ym, SUM(amount) AS revenue
    FROM orders WHERE status = 'completed'
    GROUP BY DATE_FORMAT(order_date, '%Y-%m')
)
SELECT ym, revenue,
       LAG(revenue) OVER (ORDER BY ym) AS prev_revenue,
       ROUND(100.0 * (revenue - LAG(revenue) OVER (ORDER BY ym))
                   / NULLIF(LAG(revenue) OVER (ORDER BY ym), 0), 2) AS growth_pct
FROM monthly ORDER BY ym;
```

`NULLIF(..., 0)` guards against division by zero — mention it, it's the kind of detail interviewers notice.

</details>

**Q5.** R(A,B,C,D) with FDs `AB→C`, `C→D`, `D→A`. Find all candidate keys and the highest normal form.

<details><summary>Solution</summary>

- (AB)⁺ = ABC D = all → **AB is a candidate key.**
- (BC)⁺ = BC → D → A = all → **BC is a candidate key.**
- (BD)⁺ = BD → A → (AB→C) → C = all → **BD is a candidate key.**
- Prime attributes: A, B, C, D — **all** attributes are prime.
- 2NF/3NF: since there are no non-prime attributes, there can be no partial or transitive dependency on non-prime attributes → **it is in 3NF**.
- BCNF: `C→D` — is C a super key? C⁺ = CDA, not B → no. **Violates BCNF.**
- **Answer: highest normal form is 3NF.**

</details>

**Q6.** What's wrong with this query, and fix it?

```sql
SELECT dept_id, name, MAX(salary) FROM employees GROUP BY dept_id;
```

<details><summary>Solution</summary>

`name` is neither grouped nor aggregated — the returned name is arbitrary and unrelated to the max salary. Correct version:

```sql
SELECT e.dept_id, e.name, e.salary
FROM employees e
JOIN (SELECT dept_id, MAX(salary) AS ms FROM employees GROUP BY dept_id) m
  ON e.dept_id = m.dept_id AND e.salary = m.ms;
-- or with ROW_NUMBER()/RANK() over a partition
```

</details>

**Q7.** Explain why this query is slow and fix it:

```sql
SELECT * FROM orders WHERE YEAR(order_date) = 2024 AND status != 'cancelled';
```

<details><summary>Solution</summary>

`YEAR(order_date)` wraps the indexed column in a function, so the index on `order_date` cannot be used → full table scan. `!=` is also poorly selective, and `SELECT *` fetches unnecessary columns.

```sql
SELECT order_id, customer_id, amount, status
FROM orders
WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01'
  AND status IN ('completed', 'pending', 'shipped');
```

with an index on `(order_date, status)`.

</details>

**Q8.** Find users who logged in on 3 or more consecutive days.

<details><summary>Solution</summary>

```sql
SELECT user_id, MIN(login_date) AS streak_start, COUNT(*) AS streak_length
FROM (
    SELECT user_id, login_date,
           DATE_SUB(login_date,
                    INTERVAL ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) DAY) AS grp
    FROM (SELECT DISTINCT user_id, login_date FROM logins) d
) t
GROUP BY user_id, grp
HAVING COUNT(*) >= 3;
```

The "date minus row number" trick makes consecutive dates share a constant group value — the standard gaps-and-islands technique.

</details>

---

### Study order recommendation

1. Keys & constraints → 2. Normalization (work the example by hand twice) → 3. SQL basics + execution order → 4. Joins → 5. Aggregation → 6. Subqueries & CTEs → 7. Window functions → 8. **Write all 12 patterns in §11 from memory** → 9. Indexes → 10. Transactions & isolation → 11. CAP/NoSQL/MongoDB.

**Practice platform note:** do the LeetCode SQL 50 and HackerRank SQL (Basic → Advanced) sets. SQL is the one core-CS topic where practice output is directly measurable, and interviewers will ask you to *write*, not describe.
