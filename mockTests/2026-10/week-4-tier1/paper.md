# Mock Test — October 2026, Week 4 · Tier 1 · Paper 1

**Pattern:** TCS NQT / Infosys / HCL / Allegion technical OA · **Level:** Easy
**Time:** 100 minutes (suggested: 25 min MCQs, 60 min coding, 15 min SQL)
**Marking:** +1 per correct MCQ, no negative marking. Coding and SQL: full credit only if all test cases (including hidden ones) pass.

Write your MCQ answers and SQL query in [answers.md](answers.md) and your code in `Coding1.java` / `Coding2.java` in this folder.

---

## Section A — MCQs (17)

### Operating Systems

**Q1.** Which CPU scheduling algorithm can cause **starvation** of long processes?

- A) First Come First Serve
- B) Round Robin
- C) Shortest Job First
- D) None of the above

**Q2.** Which of the following is **NOT** one of the four necessary conditions for deadlock?

- A) Mutual exclusion
- B) Hold and wait
- C) Preemption
- D) Circular wait

**Q3.** Thrashing occurs when:

- A) The CPU spends more time paging (swapping pages in and out) than executing processes
- B) The ready queue contains only CPU-bound processes
- C) The cache hit ratio reaches 100%
- D) The disk runs out of free space

### Computer Networks

**Q4.** At which OSI layer does a **router** primarily operate?

- A) Data Link
- B) Network
- C) Transport
- D) Session

**Q5.** What is the default port number for **HTTPS**?

- A) 80
- B) 443
- C) 21
- D) 25

**Q6.** Which of the following protocols is **connectionless**?

- A) TCP
- B) UDP
- C) FTP
- D) SMTP

### DBMS & SQL

**Q7.** The `Employees` table has a `dept` column with these 9 values:

`HR, HR, IT, IT, IT, Sales, IT, Sales, Sales`

How many rows does this query return?

```sql
SELECT dept, COUNT(*)
FROM Employees
GROUP BY dept
HAVING COUNT(*) > 2;
```

- A) 0
- B) 1
- C) 2
- D) 3

**Q8.** Which normal form eliminates **transitive dependencies** of non-key attributes on the primary key?

- A) 1NF
- B) 2NF
- C) 3NF
- D) 4NF

**Q9.** Which statement about `DELETE`, `TRUNCATE`, and `DROP` is **TRUE**?

- A) `TRUNCATE` supports a `WHERE` clause to remove selected rows
- B) `DELETE` is a DDL command
- C) `TRUNCATE` removes all rows but keeps the table structure
- D) `DROP` removes all rows but keeps the table structure

### OOP & Java

**Q10.** What is the output?

```java
String a = "hello";
String b = "hello";
String c = new String("hello");
System.out.println((a == b) + " " + (a == c) + " " + a.equals(c));
```

- A) `true true true`
- B) `true false true`
- C) `false false true`
- D) `true false false`

**Q11.** Method **overloading** is an example of:

- A) Runtime polymorphism
- B) Compile-time polymorphism
- C) Encapsulation
- D) Multiple inheritance

### Data Structures & Algorithms

**Q12.** What is the **worst-case** time complexity of Quick Sort?

- A) O(n log n)
- B) O(n²)
- C) O(n)
- D) O(log n)

**Q13.** Evaluate the postfix expression: `5 3 + 2 *`

- A) 11
- B) 16
- C) 13
- D) 10

**Q14.** An **inorder** traversal of a Binary Search Tree visits the keys in:

- A) Level order
- B) Descending order
- C) Ascending (sorted) order
- D) Random order

**Q15.** What is the time complexity of this code?

```java
for (int i = 1; i < n; i *= 2) {
    for (int j = 0; j < n; j++) {
        count++;
    }
}
```

- A) O(n)
- B) O(log n)
- C) O(n log n)
- D) O(n²)

### System Design

**Q16.** The primary purpose of a **load balancer** is to:

- A) Encrypt data stored in the database
- B) Distribute incoming requests across multiple servers
- C) Compress static files before sending them to clients
- D) Replace the need for a database

**Q17.** Adding a cache (e.g., Redis) in front of a database mainly helps to:

- A) Reduce read latency and database load
- B) Guarantee strong consistency for writes
- C) Reduce permanent storage cost
- D) Remove the need for backups

---

## Section B — Coding (2)

Read from **standard input**, write to **standard output**. Print nothing else (no prompts like "Enter a string").

### Coding 1 — Character Frequency in Order of Appearance

Given a string `S` of lowercase English letters, print each distinct character followed by its total count, in the order the characters **first appear** in `S`.

**Input format:** One line containing `S`.

**Output format:** One line with the character-count pairs joined together, no spaces.

**Constraints:**

- `1 ≤ |S| ≤ 10^5`
- `S` contains only `a`–`z`

**Sample 1**

```text
Input:
programming

Output:
p1r2o1g2a1m2i1n1
```

**Sample 2**

```text
Input:
aabbbc

Output:
a2b3c1
```

### Coding 2 — Second Largest Distinct Element

Given an array of `N` integers, print the **second largest distinct** value. If it doesn't exist, print `-1`.

**Input format:**

- Line 1: integer `N`
- Line 2: `N` space-separated integers

**Output format:** A single integer.

**Constraints:**

- `1 ≤ N ≤ 10^5`
- `-10^9 ≤ A[i] ≤ 10^9`

**Sample 1**

```text
Input:
6
12 35 1 10 34 1

Output:
34
```

**Sample 2**

```text
Input:
3
10 10 10

Output:
-1
```

---

## Section C — SQL (1)

Write a single query (MySQL syntax). Your query will be run on hidden data as well, so don't hard-code values from the sample.

### SQL 1 — High-Paying Departments

**Tables:**

```sql
Departments(dept_id INT PRIMARY KEY, dept_name VARCHAR(30))
Employees(emp_id INT PRIMARY KEY, name VARCHAR(30), dept_id INT, salary INT)
```

`Employees.dept_id` references `Departments.dept_id`.

**Task:** For every department whose **average salary is greater than 50000**, return `dept_name` and the average salary as `avg_salary`. Sort by `avg_salary`, highest first.

**Sample data**

`Departments`

| dept_id | dept_name |
|---|---|
| 1 | HR |
| 2 | IT |
| 3 | Sales |
| 4 | Legal |

`Employees`

| emp_id | name | dept_id | salary |
|---|---|---|---|
| 1 | Asha | 1 | 40000 |
| 2 | Ravi | 2 | 70000 |
| 3 | Meena | 2 | 90000 |
| 4 | John | 3 | 50000 |
| 5 | Priya | 3 | 65000 |
| 6 | Kiran | 1 | 45000 |
| 7 | Arjun | 2 | 80000 |

**Expected output** (decimal formatting may differ by database)

| dept_name | avg_salary |
|---|---|
| IT | 80000 |
| Sales | 57500 |
