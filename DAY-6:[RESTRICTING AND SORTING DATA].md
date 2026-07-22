# Restricting and Sorting Data in SQL

## Sorting Data

Sorting is used to arrange the output of a query.

### Types of Sorting

- **ASC (Ascending)** → Default order
- **DESC (Descending)**

### Syntax

```sql
SELECT * FROM table_name
ORDER BY column_name ASC;
```

```sql
SELECT * FROM table_name
ORDER BY column_name DESC;
```

---

# Restricting Data

The **WHERE** clause is used to filter records based on conditions.

## Comparison (Relational) Operators

| Operator | Meaning |
|----------|---------|
| = | Equal to |
| > | Greater than |
| < | Less than |
| >= | Greater than or Equal to |
| <= | Less than or Equal to |

---

## Examples

### 1. Equal To (=)

```sql
SELECT * FROM employee
WHERE address='CSE';
```

**Output**

| EID | ENAME | SALARY | ADDRESS |
|-----|--------|---------|---------|
|100|Arun|20000|CSE|

---

### 2. Greater Than (>)

```sql
SELECT * FROM employee
WHERE salary > 30000;
```

---

### 3. AND Operator

```sql
SELECT * FROM employee
WHERE salary > 30000
AND address='IT';
```

---

### 4. OR Operator

```sql
SELECT * FROM employee
WHERE salary > 30000
OR address='IT';
```

---

## NULL Values

### IS NULL

```sql
SELECT * FROM employee
WHERE salary IS NULL;
```

### IS NOT NULL

```sql
SELECT * FROM employee
WHERE salary IS NOT NULL;
```

---

## DISTINCT

Removes duplicate values.

```sql
SELECT DISTINCT salary
FROM employee;
```

---

## Case Sensitivity

```sql
SELECT eid, salary
FROM employee
WHERE ename='arjun';
```

**Output**

```
No rows selected
```

Correct:

```sql
SELECT eid, salary
FROM employee
WHERE ename='Arjun';
```

---

# Column Aliases (AS)

Rename column headings.

```sql
SELECT
eid AS "Employee Id",
ename AS "Employee Name"
FROM employee;
```

---

## Formatting Column Width

```sql
COLUMN "Employee Name" FORMAT A20;
```

---

# Concatenation Operator (||)

Combine multiple columns into one output.

### Example 1

```sql
SELECT
ename || ', ' || address AS "Employee Details"
FROM employee;
```

Output

```
Arun, CSE
Arjun, ECE
Ram, IT
Sujan, AIML
Sujan,
Shlok, AID
```

---

### Example 2

```sql
SELECT
eid || ', ' || ename || ', ' || address
AS "Employee Details"
FROM employee;
```

Output

```
100, Arun, CSE
101, Arjun, ECE
102, Ram, IT
103, Sujan, AIML
104, Sujan,
105, Shlok, AID
```

---

# IN Operator

Returns rows matching any value in the list.

```sql
SELECT eid, ename
FROM employee
WHERE address IN ('CSE','IT');
```

---

## NOT IN

```sql
SELECT eid, ename, address
FROM employee
WHERE address NOT IN ('CSE','IT');
```

---

# BETWEEN Operator

Returns values within a range (inclusive).

```sql
SELECT eid, ename, salary
FROM employee
WHERE salary BETWEEN 25000 AND 45000;
```

Another example:

```sql
SELECT eid, ename, salary
FROM employee
WHERE salary BETWEEN 35000 AND 45000;
```

---

# Substitution Variable (&)

Allows the user to enter a value at runtime.

```sql
SELECT eid, ename, salary
FROM employee
WHERE salary=&salary;
```

Example

```
Enter value for salary: 35000
```

---

# ORDER BY Clause

Sorts query results.

## Ascending Order (Default)

```sql
SELECT *
FROM employee
ORDER BY address;
```

---

## Descending Order

```sql
SELECT *
FROM employee
ORDER BY address DESC;
```

---

## ORDER BY Column Position

Instead of column name, column number can be used.

```sql
SELECT *
FROM employee
ORDER BY 4;
```

Descending:

```sql
SELECT *
FROM employee
ORDER BY 4 DESC;
```

---

## Dynamic Sorting Using Substitution Variable

```sql
SELECT *
FROM employee
ORDER BY &ColName;
```

Example

```
Enter value for ColName:
salary
```

Descending:

```sql
SELECT *
FROM employee
ORDER BY &ColName DESC;
```

---

## Filtering and Sorting Together

```sql
SELECT *
FROM employee
WHERE salary = 45000
ORDER BY &ColName DESC;
```

Example

```
Enter value for ColName:
address
```

---

# Summary

### Restricting Data

- WHERE
- AND
- OR
- IN
- NOT IN
- BETWEEN
- IS NULL
- IS NOT NULL
- DISTINCT

### Sorting Data

- ORDER BY
- ASC (Default)
- DESC
- ORDER BY Column Position
- ORDER BY Using Substitution Variable (&)

---

## Practice Queries

```sql
-- Employees working in IT
SELECT * FROM employee
WHERE address='IT';

-- Employees with salary greater than 30000
SELECT * FROM employee
WHERE salary > 30000;

-- Distinct salaries
SELECT DISTINCT salary
FROM employee;

-- Employees between salary range
SELECT *
FROM employee
WHERE salary BETWEEN 25000 AND 45000;

-- Employees ordered by salary
SELECT *
FROM employee
ORDER BY salary;

-- Employees ordered by salary descending
SELECT *
FROM employee
ORDER BY salary DESC;
```
