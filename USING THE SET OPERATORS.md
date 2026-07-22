# USING THE SET OPERATORS

## What are SET Operators?

**SET Operators** are used to combine the results of **two or more `SELECT` statements** into a single result set.

---

# Rules for Using SET Operators

Before using SET operators, the following conditions must be satisfied:

1. Both queries must have the **same number of columns**.
2. The corresponding columns must have **compatible data types**.
3. The **ORDER BY** clause can be used **only once**, at the end of the combined query.
4. The output column names are taken from the **first SELECT statement**.

---

# Types of SET Operators

1. `UNION`
2. `UNION ALL`
3. `INTERSECT`
4. `MINUS` (Set Difference)

---

# Sample Tables

## emp1

| EID | ENAME | SALARY |
|----:|--------|-------:|
|100|AAA|10000|
|101|BBB|20000|
|102|CCC|30000|

---

## emp2

| EID | ENAME | SALARY |
|----:|--------|-------:|
|1234|XXX|20000|
|2345|YYY|34000|
|100|AAA|10000|

---

# 1. UNION

The **UNION** operator combines the results of two queries and **removes duplicate rows**.

### Syntax

```sql
SELECT column_names
FROM table1

UNION

SELECT column_names
FROM table2;
```

### Example

```sql
SELECT eid AS emp_id, ename
FROM emp1

UNION

SELECT eid AS employee_id, ename
FROM emp2;
```

### Output

| EMP_ID | ENAME |
|-------:|--------|
|100|AAA|
|101|BBB|
|102|CCC|
|1234|XXX|
|2345|YYY|

> **Note:** Duplicate rows are removed automatically.

---

# 2. UNION ALL

The **UNION ALL** operator combines the results of two queries **including duplicate rows**.

### Syntax

```sql
SELECT column_names
FROM table1

UNION ALL

SELECT column_names
FROM table2;
```

### Example

```sql
SELECT eid AS emp_id, ename
FROM emp1

UNION ALL

SELECT eid AS employee_id, ename
FROM emp2;
```

### Output

| EMP_ID | ENAME |
|-------:|--------|
|100|AAA|
|101|BBB|
|102|CCC|
|1234|XXX|
|2345|YYY|
|100|AAA|

> **Note:** Duplicate rows are **not removed**.

---

# 3. INTERSECT

The **INTERSECT** operator returns **only the rows that are common** to both queries.

### Syntax

```sql
SELECT column_names
FROM table1

INTERSECT

SELECT column_names
FROM table2;
```

### Example

```sql
SELECT eid AS emp_id, ename
FROM emp1

INTERSECT

SELECT eid AS employee_id, ename
FROM emp2;
```

### Output

| EMP_ID | ENAME |
|-------:|--------|
|100|AAA|

---

# 4. MINUS (Set Difference)

The **MINUS** operator returns the rows that exist in the **first query but not in the second query**.

### Syntax

```sql
SELECT column_names
FROM table1

MINUS

SELECT column_names
FROM table2;
```

### Example

```sql
SELECT eid AS emp_id, ename
FROM emp1

MINUS

SELECT eid AS employee_id, ename
FROM emp2;
```

### Output

| EMP_ID | ENAME |
|-------:|--------|
|101|BBB|
|102|CCC|

---

# ORDER BY with SET Operators

The `ORDER BY` clause should be used **only once**, after the final `SELECT` statement.

### Example

```sql
SELECT eid, ename
FROM emp1

UNION

SELECT eid, ename
FROM emp2

ORDER BY ename;
```

---

# Example 1

Display the total salary for selected departments using `DECODE()` and `GROUP BY`.

```sql
SELECT job_id,
       SUM(DECODE(department_id,10,salary)) AS dept_10,
       SUM(DECODE(department_id,20,salary)) AS dept_20,
       SUM(DECODE(department_id,30,salary)) AS dept_30
FROM employees
WHERE department_id IN (10,20,30)
GROUP BY job_id
ORDER BY job_id;
```

---

# Example 2

Combine department-wise salary totals with department-job salary totals.

```sql
SELECT department_id,
       ' ' AS job_id,
       SUM(salary)
FROM employees
GROUP BY department_id

UNION

SELECT department_id,
       job_id,
       SUM(salary)
FROM employees
GROUP BY department_id, job_id;
```

### Sample Output

| DEPARTMENT_ID | JOB_ID | SUM(SALARY) |
|--------------:|--------|------------:|
|4| |100000|
|5|CLERK|250000|
|7| |...|
|12| |...|
|47| |...|

---

# Example 3

Display employees from multiple departments using `UNION ALL`.

```sql
SELECT job_id, department_id
FROM employees
WHERE department_id = 20

UNION ALL

SELECT job_id, department_id
FROM employees
WHERE department_id = 10

UNION ALL

SELECT job_id, department_id
FROM employees
WHERE department_id = 30;
```

### Sample Output

| JOB_ID | DEPARTMENT_ID |
|---------|--------------:|
|Assistant|20|
|Professor|20|
|Clerk|10|
|Teaching|10|
|Trainer|30|

---

# Difference Between SET Operators

| Operator | Removes Duplicates | Purpose |
|-----------|-------------------|----------|
| `UNION` | ✅ Yes | Combines results and removes duplicates |
| `UNION ALL` | ❌ No | Combines results including duplicates |
| `INTERSECT` | ✅ Yes | Returns common rows |
| `MINUS` | ✅ Yes | Returns rows present only in the first query |

---

# Summary

## SET Operators

- `UNION` → Combines results and removes duplicates.
- `UNION ALL` → Combines results including duplicates.
- `INTERSECT` → Returns only common rows.
- `MINUS` → Returns rows present in the first query but not in the second.

---

# Practice Queries

```sql
-- UNION
SELECT eid, ename
FROM emp1
UNION
SELECT eid, ename
FROM emp2;

-- UNION ALL
SELECT eid, ename
FROM emp1
UNION ALL
SELECT eid, ename
FROM emp2;

-- INTERSECT
SELECT eid, ename
FROM emp1
INTERSECT
SELECT eid, ename
FROM emp2;

-- MINUS
SELECT eid, ename
FROM emp1
MINUS
SELECT eid, ename
FROM emp2;

-- UNION with ORDER BY
SELECT eid, ename
FROM emp1
UNION
SELECT eid, ename
FROM emp2
ORDER BY ename;
```
