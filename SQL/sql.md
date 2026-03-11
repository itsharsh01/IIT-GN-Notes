# 📘 SQL Quick Reference Guide

> A compact SQL revision sheet covering fundamental concepts, commands, and functions.

---

## 📑 Table of Contents

1. [ACID Properties](#1-acid-properties)
2. [BASE Properties](#2-base-nosql)
3. [Codd's 12 Rules](#3-codds-12-rules)
4. [SQL Command Types](#4-sql-command-types)
5. [Normalization](#5-normalization)
6. [Basic Query Structure](#6-basic-query-structure)
7. [Filtering Operators](#7-filtering-operators)
8. [Joins](#8-joins)
9. [Aggregation Functions](#9-aggregation-functions)
10. [Window Functions](#10-window-functions)
11. [Subqueries](#11-subqueries)
12. [Set Operations](#12-set-operations)
13. [Constraints & Indexes](#13-constraints--indexes)
14. [Transactions](#14-transactions)

---

## 1. ACID Properties

> Defines the **reliability of transactions** in relational databases.

| Property | Meaning | Example |
|---|---|---|
| **A**tomicity | All or nothing — if one step fails, everything rolls back | Transfer fails → both debit & credit reversed |
| **C**onsistency | DB stays in a valid state before & after a transaction | Balance can't go below zero |
| **I**solation | Concurrent transactions don't interfere with each other | Two withdrawals at same time don't corrupt balance |
| **D**urability | Committed data is permanent, even after a crash | Payment confirmed → saved forever |

---

## 2. BASE (NoSQL)

> Used by **distributed databases** — trades strict consistency for availability.

| Property | Meaning |
|---|---|
| **B**asically Available | System always responds, even if some nodes fail |
| **S**oft State | Data may temporarily differ across nodes |
| **E**ventually Consistent | All replicas sync up over time |

**Example:** Social media like counts syncing across servers.

---

## 3. Codd's 12 Rules

> Defines what makes a **true relational database**.

| Rule | Description |
|---|---|
| Rule 0 | Must be relational |
| Rule 1 | All data in tables |
| Rule 2 | Data accessible via table name + column + primary key |
| Rule 3 | NULL support |
| Rule 4 | Schema stored as tables |
| Rule 5 | Comprehensive SQL language support |
| Rule 6 | Views should be updatable |
| Rule 7 | Set operations supported (INSERT / UPDATE / DELETE) |
| Rule 8 | Physical storage independent of logical structure |
| Rule 9 | Logical structure independent from applications |
| Rule 10 | Integrity constraints stored in DB |
| Rule 11 | Distributed databases behave the same |
| Rule 12 | Low-level access cannot bypass integrity rules |

---

## 4. SQL Command Types

### DDL — Data Definition Language
> Defines database **structure**

| Command | Use |
|---|---|
| `CREATE` | Create a table or database |
| `ALTER` | Modify structure |
| `DROP` | Delete table/database |
| `TRUNCATE` | Remove all rows, keep structure |
| `RENAME` | Rename a table |

```sql
CREATE TABLE users (
  id   INT,
  name VARCHAR(50)
);
```

---

### DML — Data Manipulation Language
> Manipulates **data inside tables**

| Command | Use |
|---|---|
| `INSERT` | Add new rows |
| `UPDATE` | Modify existing rows |
| `DELETE` | Remove rows |

```sql
INSERT INTO users VALUES (1, 'Harsh');
```

---

### DCL — Data Control Language
> Controls **access and permissions**

| Command | Use |
|---|---|
| `GRANT` | Give permissions |
| `REVOKE` | Remove permissions |

```sql
GRANT SELECT ON users TO analyst;
```

---

### TCL — Transaction Control Language
> Controls **transactions**

| Command | Use |
|---|---|
| `COMMIT` | Save changes permanently |
| `ROLLBACK` | Undo changes |
| `SAVEPOINT` | Set a checkpoint within a transaction |

```sql
ROLLBACK;
```

---

## 5. Normalization

> Organizes data to **reduce redundancy** and improve consistency.

### 1NF — First Normal Form
- Atomic values only
- No repeating groups

| ❌ Bad | ✅ Good |
|---|---|
| Harsh → ML, AI | Harsh → ML |
| | Harsh → AI |

### 2NF — Second Normal Form
- Must satisfy 1NF
- No **partial dependency** (non-key columns depend on whole primary key)

> **Fix:** Split student and course into separate tables.

### 3NF — Third Normal Form
- Must satisfy 2NF
- No **transitive dependency** (non-key column depending on another non-key column)

> **Fix:** Separate department and department head into their own table.

---

## 6. Basic Query Structure

```sql
SELECT   name, salary          -- 1. columns to fetch
FROM     employees             -- 2. source table
WHERE    salary > 5000         -- 3. filter rows
GROUP BY department            -- 4. group for aggregation
HAVING   COUNT(*) > 5          -- 5. filter groups
ORDER BY salary DESC           -- 6. sort result
LIMIT    10                    -- 7. cap rows returned
OFFSET   5;                    -- 8. skip first N rows
```

| Clause | Purpose |
|---|---|
| `SELECT` | Choose columns to return |
| `DISTINCT` | Remove duplicate rows |
| `FROM` | Source table |
| `WHERE` | Filter before grouping |
| `GROUP BY` | Aggregate rows by column |
| `HAVING` | Filter after grouping |
| `ORDER BY` | Sort ASC or DESC |
| `LIMIT` | Max rows to return |
| `OFFSET` | Skip first N rows |
| `AS` | Rename column/table alias |

```sql
-- Alias example
SELECT salary AS employee_salary FROM employees;
```

---

## 7. Filtering Operators

| Operator | Use | Example |
|---|---|---|
| `IN` | Match a list of values | `WHERE dept IN ('HR','IT')` |
| `BETWEEN` | Range check (inclusive) | `WHERE salary BETWEEN 4000 AND 7000` |
| `LIKE` | Pattern match | `WHERE name LIKE 'H%'` |
| `IS NULL` | Check for null | `WHERE manager_id IS NULL` |
| `IS NOT NULL` | Check for non-null | `WHERE manager_id IS NOT NULL` |

**LIKE Patterns:**

| Pattern | Matches |
|---|---|
| `'H%'` | Starts with H |
| `'%h'` | Ends with h |
| `'%ar%'` | Contains "ar" |
| `'H_rsh'` | H + any one char + rsh |

---

## 8. Joins

> Combine rows from two or more tables.

```
Table A     Table B
  1           1  ← INNER JOIN returns this
  2           3
  3               ← LEFT JOIN includes A's 2
```

| Join Type | Returns |
|---|---|
| `INNER JOIN` | Only matching rows from both tables |
| `LEFT JOIN` | All rows from left + matches from right (NULL if no match) |
| `RIGHT JOIN` | All rows from right + matches from left |
| `FULL JOIN` | All rows from both, NULLs where no match |
| `CROSS JOIN` | Every combination (Cartesian product) |

```sql
-- INNER JOIN
SELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;

-- LEFT JOIN
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

---

## 9. Aggregation Functions

> Perform calculations on a **set of rows**.

| Function | Description | Example |
|---|---|---|
| `COUNT(*)` | Count rows | `SELECT COUNT(*) FROM employees` |
| `SUM(col)` | Total of values | `SELECT SUM(salary) FROM employees` |
| `AVG(col)` | Average value | `SELECT AVG(salary) FROM employees` |
| `MIN(col)` | Smallest value | `SELECT MIN(salary) FROM employees` |
| `MAX(col)` | Largest value | `SELECT MAX(salary) FROM employees` |

```sql
SELECT department, COUNT(*), AVG(salary)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

---

## 10. Window Functions

> Perform calculations **across rows** without collapsing them into one row.

```sql
FUNCTION_NAME() OVER (
  PARTITION BY column   -- optional: reset window per group
  ORDER BY column       -- order within window
)
```

| Function | Description |
|---|---|
| `ROW_NUMBER()` | Sequential unique row number |
| `RANK()` | Rank with **gaps** for ties (1,1,3) |
| `DENSE_RANK()` | Rank **without gaps** for ties (1,1,2) |
| `NTILE(n)` | Divide rows into n equal buckets |
| `LAG(col)` | Access **previous** row's value |
| `LEAD(col)` | Access **next** row's value |
| `FIRST_VALUE(col)` | First value in the window |
| `LAST_VALUE(col)` | Last value in the window |

```sql
-- Rank employees by salary
SELECT name, salary,
  RANK()       OVER (ORDER BY salary DESC) AS rank,
  DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank,
  ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
FROM employees;

-- Compare with previous row
SELECT name, salary,
  LAG(salary)  OVER (ORDER BY salary) AS prev_salary,
  LEAD(salary) OVER (ORDER BY salary) AS next_salary
FROM employees;
```

---

## 11. Subqueries

> A query **nested inside** another query.

### Scalar Subquery — returns a single value
```sql
SELECT name FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

### Correlated Subquery — references outer query
```sql
SELECT e1.name FROM employees e1
WHERE salary > (
  SELECT AVG(salary) FROM employees e2
  WHERE e2.department = e1.department
);
```

### EXISTS — checks if subquery returns any rows
```sql
SELECT name FROM users u
WHERE EXISTS (
  SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

### ANY / ALL
```sql
-- ANY: true if any value in subquery matches
WHERE salary > ANY (SELECT salary FROM employees WHERE department='HR')

-- ALL: true only if all values in subquery match
WHERE salary > ALL (SELECT salary FROM employees WHERE department='HR')
```

---

## 12. Set Operations

> Combine results from **multiple SELECT statements**.

| Operation | Description |
|---|---|
| `UNION` | Combine, remove duplicates |
| `UNION ALL` | Combine, keep duplicates |
| `INTERSECT` | Only rows in **both** results |
| `EXCEPT` | Rows in first result **not** in second |

```sql
-- Employees who are also managers
SELECT name FROM employees
INTERSECT
SELECT name FROM managers;

-- Employees who are NOT managers
SELECT name FROM employees
EXCEPT
SELECT name FROM managers;
```

> ⚠️ All SELECT statements in set operations must have the **same number of columns** with compatible data types.

---

## 13. Constraints & Indexes

### Constraints

| Constraint | Purpose | Example |
|---|---|---|
| `PRIMARY KEY` | Unique row identifier | `id INT PRIMARY KEY` |
| `FOREIGN KEY` | Links to another table's PK | `REFERENCES users(id)` |
| `UNIQUE` | No duplicate values in column | `email VARCHAR(100) UNIQUE` |
| `CHECK` | Validates data before insert | `salary INT CHECK (salary > 0)` |
| `DEFAULT` | Sets fallback value | `status VARCHAR(20) DEFAULT 'active'` |
| `NOT NULL` | Column must have a value | `name VARCHAR(50) NOT NULL` |

```sql
CREATE TABLE users (
  id       INT PRIMARY KEY,
  email    VARCHAR(100) UNIQUE NOT NULL,
  salary   INT CHECK (salary > 0),
  status   VARCHAR(20) DEFAULT 'active'
);
```

### Index
> Speeds up query performance on frequently searched columns.

```sql
CREATE INDEX idx_salary ON employees(salary);
```

> ⚠️ Indexes speed up **reads** but slow down **writes**. Don't over-index.

---

## 14. Transactions

> Group multiple SQL operations into a **single atomic unit**.

```sql
BEGIN;

  UPDATE accounts SET balance = balance - 500 WHERE id = 1;
  UPDATE accounts SET balance = balance + 500 WHERE id = 2;

COMMIT;   -- save both changes
-- or
ROLLBACK; -- undo both if something went wrong
```

### SAVEPOINT
> Create a checkpoint to roll back to a specific point (not the start).

```sql
BEGIN;
  INSERT INTO orders VALUES (1, 'Item A');
  SAVEPOINT after_first_insert;

  INSERT INTO orders VALUES (2, 'Item B');
  ROLLBACK TO after_first_insert; -- undoes only Item B

COMMIT;
```

---

*Last updated: March 2026*