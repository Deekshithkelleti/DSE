# Reported Aggregated Data Using The Group Functions.

## LIKE Operator

The **LIKE** operator is used to search for a specified pattern in a column.

### Wildcards Used with LIKE

| Wildcard | Meaning |
|----------|---------|
| `%` | Matches **zero or more characters** |
| `_` | Matches **exactly one character** |

---

## Sample Employee Data

| EID | ENAME | SALARY | ADDRESS |
|-----|--------|---------|---------|
|100|John|30000|CSE|
|101|James|35000|CSE|
|102|Lohith|45000|IT|
|103|Praneeth|35000|AIDS|

---

# LIKE Examples

## 1. Names Starting with 'J'

```sql
SELECT ename
FROM employee
WHERE ename LIKE 'J%';
```

**Output**

```
John
James
```

---

## 2. Names Ending with 'h'

```sql
SELECT ename
FROM employee
WHERE ename LIKE '%h';
```

**Output**

```
Lohith
Praneeth
```

---

## 3. Names Ending with 'n'

```sql
SELECT ename
FROM employee
WHERE ename LIKE '%n';
```

**Output**

```
John
```

---

## 4. Names Matching 'J__n'

- Starts with **J**
- Ends with **n**
- Contains exactly **2 characters** in between

```sql
SELECT ename
FROM employee
WHERE ename LIKE 'J__n';
```

**Output**

```
John
```

---

## 5. Exactly Three Characters Starting with J

```sql
SELECT ename
FROM employee
WHERE ename LIKE 'J__';
```

**Output**

```
No rows selected
```

---

## 6. Exactly Four Characters Starting with J

```sql
SELECT ename
FROM employee
WHERE ename LIKE 'J___';
```

**Output**

```
John
```

---

## 7. Exactly Six Characters

```sql
SELECT ename
FROM employee
WHERE ename LIKE '______';
```

**Output**

```
Lohith
```

---

## 8. Names Starting with P

```sql
SELECT ename
FROM employee
WHERE ename LIKE 'P%';
```

**Output**

```
Praneeth
```

---

## 9. Names Containing 'ne'

```sql
SELECT ename
FROM employee
WHERE ename LIKE '%ne%';
```

**Output**

```
Praneeth
```

---

## 10. Second Character is 'o'

```sql
SELECT ename
FROM employee
WHERE ename LIKE '_o%';
```

**Output**

```
John
Lohith
```

---

# Aggregate Functions

Aggregate functions perform calculations on a group of rows and return a **single value**.

## Aggregate Functions

| Function | Description |
|----------|-------------|
| COUNT() | Counts the number of rows |
| SUM() | Returns the total of numeric values |
| AVG() | Returns the average value |
| MIN() | Returns the minimum value |
| MAX() | Returns the maximum value |

---

# COUNT()

Returns the total number of rows.

```sql
SELECT COUNT(*)
FROM employee;
```

**Output**

```
4
```

---

# SUM()

Returns the total salary.

```sql
SELECT SUM(salary) AS Total
FROM employee;
```

**Output**

```
145000
```

---

# AVG()

Returns the average salary.

```sql
SELECT AVG(salary) AS Average_Salary
FROM employee;
```

**Output**

```
36250
```

---

# Multiple Aggregate Functions

```sql
SELECT
COUNT(*) AS Total_Employees,
SUM(salary) AS Total_Salary,
AVG(salary) AS Average_Salary
FROM employee;
```

**Output**

| Total Employees | Total Salary | Average Salary |
|----------------:|-------------:|---------------:|
|4|145000|36250|

---

# MIN() and MAX()

```sql
SELECT
MIN(salary) AS Min_Salary,
MAX(salary) AS Max_Salary
FROM employee;
```

**Output**

| Min Salary | Max Salary |
|------------|------------|
|30000|45000|

---

# GROUP BY Clause

The **GROUP BY** clause groups rows that have the same values into summary rows.

### Syntax

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

### Example

```sql
SELECT address,
COUNT(*) AS Total
FROM employee
GROUP BY address;
```

**Output**

| ADDRESS | TOTAL |
|---------|------:|
|CSE|2|
|IT|1|
|AIDS|1|

---

# HAVING Clause

The **HAVING** clause filters grouped records after the `GROUP BY` clause.

### Syntax

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

### Example

```sql
SELECT address,
COUNT(*) AS Total_Employees
FROM employee
GROUP BY address
HAVING COUNT(*) > 1;
```

**Output**

| ADDRESS | TOTAL_EMPLOYEES |
|---------|----------------:|
|CSE|2|

---

# Difference Between WHERE and HAVING

| WHERE | HAVING |
|--------|---------|
| Filters rows before grouping | Filters groups after grouping |
| Cannot use aggregate functions | Can use aggregate functions |
| Used before `GROUP BY` | Used after `GROUP BY` |

---

# Summary

## LIKE Wildcards

| Wildcard | Meaning |
|----------|---------|
| `%` | Zero or more characters |
| `_` | Exactly one character |

### Common LIKE Patterns

| Pattern | Meaning |
|----------|---------|
| `'J%'` | Starts with J |
| `'%h'` | Ends with h |
| `'%ne%'` | Contains "ne" |
| `'J__n'` | J + 2 characters + n |
| `'______'` | Exactly 6 characters |
| `'_o%'` | Second character is o |

---

## Aggregate Functions

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`

---

## GROUP BY & HAVING

- `GROUP BY` → Groups rows based on a column.
- `HAVING` → Filters grouped results.

---

# Practice Queries

```sql
-- Names starting with J
SELECT ename
FROM employee
WHERE ename LIKE 'J%';

-- Names ending with h
SELECT ename
FROM employee
WHERE ename LIKE '%h';

-- Names containing "ne"
SELECT ename
FROM employee
WHERE ename LIKE '%ne%';

-- Total number of employees
SELECT COUNT(*)
FROM employee;

-- Total salary
SELECT SUM(salary)
FROM employee;

-- Average salary
SELECT AVG(salary)
FROM employee;

-- Highest and lowest salary
SELECT MIN(salary), MAX(salary)
FROM employee;

-- Number of employees in each department
SELECT address, COUNT(*)
FROM employee
GROUP BY address;

-- Departments having more than one employee
SELECT address, COUNT(*)
FROM employee
GROUP BY address
HAVING COUNT(*) > 1;
```
