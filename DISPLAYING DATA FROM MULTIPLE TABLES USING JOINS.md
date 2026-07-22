# DISPLAYING DATA FROM MULTIPLE TABLES USING JOINS

## What is a Join?

A **JOIN** is used to combine data from **two or more tables** based on a related column.

---

# Types of Joins

- Inner Join
  - Equi Join
  - Non-Equi Join
- Outer Join
  - Left Outer Join
  - Right Outer Join
  - Full Outer Join
- Natural Join
- Self Join
- Cross Join

---

# 1. Inner Join

An **INNER JOIN** returns **only the matching rows** from both tables.

### Syntax

```sql
SELECT column_names
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;
```

### Example

```sql
SELECT ename, salary, dname
FROM emp
INNER JOIN dept
ON emp.did = dept.did;
```

### Output

| ENAME | SALARY | DNAME |
|--------|--------|--------|
| RAM | 20000 | CSE |
| RAJ | 25000 | IT |
| SAM | 35000 | CSE |

---

# 2. Outer Join

Outer joins return matching rows along with non-matching rows.

## 2.1 Left Outer Join

Returns **all rows from the left table** and matching rows from the right table.

### Syntax

```sql
SELECT columns
FROM emp
LEFT OUTER JOIN dept
ON emp.did = dept.did;
```

### Example

```sql
SELECT eid, ename, salary, dname
FROM emp
LEFT OUTER JOIN dept
ON emp.did = dept.did;
```

### Output

| EID | ENAME | SALARY | DNAME |
|----:|--------|--------|--------|
|100|RAM|20000|CSE|
|101|RAJ|25000|IT|
|102|SAM|35000|CSE|
|103|SAJI|40000|NULL|

---

## 2.2 Right Outer Join

Returns **all rows from the right table** and matching rows from the left table.

### Syntax

```sql
SELECT columns
FROM emp
RIGHT OUTER JOIN dept
ON emp.did = dept.did;
```

### Example

```sql
SELECT eid, ename, salary, dname
FROM emp
RIGHT OUTER JOIN dept
ON emp.did = dept.did;
```

### Output

| EID | ENAME | SALARY | DNAME |
|----:|--------|--------|--------|
|NULL|NULL|NULL|MECH|
|NULL|NULL|NULL|ECE|
|100|RAM|20000|CSE|
|102|SAM|35000|CSE|
|101|RAJ|25000|IT|
|NULL|NULL|NULL|CIC|

---

## 2.3 Full Outer Join

Returns **all rows from both tables**, whether they match or not.

### Syntax

```sql
SELECT columns
FROM emp
FULL OUTER JOIN dept
ON emp.did = dept.did;
```

### Example

```sql
SELECT eid, ename, salary, dname
FROM emp
FULL OUTER JOIN dept
ON emp.did = dept.did;
```

### Output

| EID | ENAME | SALARY | DNAME |
|----:|--------|--------|--------|
|100|RAM|20000|CSE|
|102|SAM|35000|CSE|
|101|RAJ|25000|IT|
|103|SAJI|40000|NULL|
|NULL|NULL|NULL|ECE|
|NULL|NULL|NULL|MECH|
|NULL|NULL|NULL|CIC|

---

# 3. Natural Join

A **NATURAL JOIN** automatically joins tables based on columns with the **same name**.

> No `ON` clause is required.

### Syntax

```sql
SELECT columns
FROM table1
NATURAL JOIN table2;
```

### Example

```sql
SELECT *
FROM emp
NATURAL JOIN dept;
```

### Output

| DID | EID | ENAME | SALARY | DNAME |
|----:|----:|--------|--------|--------|
|5|100|RAM|20000|CSE|
|12|101|RAJ|25000|IT|
|5|102|SAM|35000|CSE|

---

### HR Schema Example

```sql
SELECT employee_id,
       first_name,
       salary,
       department_name,
       location_id
FROM employees
NATURAL JOIN departments;
```

---

# 4. Equi Join

An **Equi Join** is a type of **Inner Join** that uses the `=` operator.

### Example (Old Syntax)

```sql
SELECT ename, dname
FROM emp, dept
WHERE emp.did = dept.did;
```

### Example (ANSI Syntax)

```sql
SELECT ename, dname
FROM emp
JOIN dept
ON emp.did = dept.did;
```

### Output

| ENAME | DNAME |
|--------|--------|
|RAM|CSE|
|RAJ|IT|
|SAM|CSE|

---

# 5. Non-Equi Join

A **Non-Equi Join** uses operators other than `=` such as:

- `<`
- `>`
- `<=`
- `>=`
- `BETWEEN`

### Example

```sql
SELECT e.employee_id,
       e.salary,
       j.grade_level
FROM employees e
JOIN job_grades j
ON e.salary BETWEEN j.lowest_sal AND j.highest_sal;
```

---

# 6. Self Join

A **Self Join** joins a table with itself.

It is commonly used to display **employee-manager relationships**.

### Syntax

```sql
SELECT e.column,
       m.column
FROM employees e
JOIN employees m
ON e.manager_id = m.employee_id;
```

### Example

```sql
SELECT e.ename AS Employee,
       m.ename AS Manager
FROM employees e
JOIN employees m
ON e.mgr = m.empno;
```

---

# 7. Cross Join

A **Cross Join** returns the Cartesian Product of two tables.

Every row from the first table is combined with every row from the second table.

### Formula

```
Rows in Table1 × Rows in Table2
```

Example:

```
4 × 5 = 20 rows
```

### Syntax

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

---

# Example

Retrieve employees working in the **DALLAS** location.

```sql
SELECT ename,
       job_id,
       did,
       dname
FROM emp
JOIN dept
ON emp.did = dept.did
WHERE dept.loc = 'DALLAS';
```

---

# Difference Between Joins

| Join | Returns |
|------|---------|
| Inner Join | Only matching rows |
| Left Outer Join | All rows from left table + matching rows from right |
| Right Outer Join | All rows from right table + matching rows from left |
| Full Outer Join | All rows from both tables |
| Natural Join | Matches columns with the same name automatically |
| Equi Join | Uses `=` operator |
| Non-Equi Join | Uses `<`, `>`, `BETWEEN`, etc. |
| Self Join | Joins a table with itself |
| Cross Join | Cartesian product (every row with every row) |

---

# Summary

## Join Types

- Inner Join
- Left Outer Join
- Right Outer Join
- Full Outer Join
- Natural Join
- Equi Join
- Non-Equi Join
- Self Join
- Cross Join

---

# Practice Queries

```sql
-- Natural Join
SELECT *
FROM emp
NATURAL JOIN dept;

-- Inner Join
SELECT ename, salary, dname
FROM emp
INNER JOIN dept
ON emp.did = dept.did;

-- Left Outer Join
SELECT *
FROM emp
LEFT OUTER JOIN dept
ON emp.did = dept.did;

-- Right Outer Join
SELECT *
FROM emp
RIGHT OUTER JOIN dept
ON emp.did = dept.did;

-- Full Outer Join
SELECT *
FROM emp
FULL OUTER JOIN dept
ON emp.did = dept.did;

-- Equi Join
SELECT ename, dname
FROM emp
JOIN dept
ON emp.did = dept.did;

-- Self Join
SELECT e.ename AS Employee,
       m.ename AS Manager
FROM employees e
JOIN employees m
ON e.mgr = m.empno;

-- Cross Join
SELECT *
FROM emp
CROSS JOIN dept;
```
