# Data Manipulation Language (DML) Operations in SQL

## Overview

Data Manipulation Language (DML) is a set of SQL commands used to insert, update, delete, and retrieve data from database tables.

Unlike DDL, DML commands work with the **data stored inside the tables** without changing the table structure.

### Characteristics

- Used to manipulate data in database tables.
- Does not modify the database schema.
- Supports transactions.
- Changes can be committed or rolled back before COMMIT.
- Used for day-to-day database operations.

---

# Types of DML Operations

| Operation | Purpose |
|-----------|---------|
| INSERT | Adds new records into a table. |
| UPDATE | Modifies existing records. |
| DELETE | Removes records from a table. |
| SELECT* | Retrieves records from one or more tables. |

> **Note:** SELECT is officially a **DQL (Data Query Language)** command, but it is commonly studied along with DML because it works with table data.

---

# Sample Table

```sql
CREATE TABLE employee (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(30),
    salary NUMBER(8,2),
    department VARCHAR2(20)
);
```

---

# 1. INSERT

## Purpose

Adds one or more new records into a table.

### Syntax

```sql
INSERT INTO table_name
VALUES(value1, value2, ...);
```

### Example

```sql
INSERT INTO employee
VALUES(101, 'John', 45000, 'HR');
```

### Insert into Specific Columns

```sql
INSERT INTO employee(emp_id, emp_name, salary)
VALUES(102, 'David', 50000);
```

### Result

New rows are inserted into the table.

---

# 2. UPDATE

## Purpose

Modifies existing records in a table.

### Syntax

```sql
UPDATE table_name
SET column_name = value
WHERE condition;
```

### Example

```sql
UPDATE employee
SET salary = 55000
WHERE emp_id = 101;
```

### Update Multiple Columns

```sql
UPDATE employee
SET salary = 60000,
    department = 'Finance'
WHERE emp_id = 102;
```

### Result

The specified records are updated.

---

# 3. DELETE

## Purpose

Deletes one or more records from a table.

### Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

### Example

```sql
DELETE FROM employee
WHERE emp_id = 101;
```

### Delete All Records

```sql
DELETE FROM employee;
```

### Result

Selected records (or all records) are removed, but the table structure remains.

---

# 4. SELECT (DQL)

## Purpose

Retrieves data from one or more tables.

### Syntax

```sql
SELECT column_name
FROM table_name;
```

### Display All Records

```sql
SELECT *
FROM employee;
```

### Display Selected Columns

```sql
SELECT emp_name, salary
FROM employee;
```

### Display Records Using WHERE

```sql
SELECT *
FROM employee
WHERE salary > 50000;
```

### Sort Records

```sql
SELECT *
FROM employee
ORDER BY salary DESC;
```

### Display Unique Values

```sql
SELECT DISTINCT department
FROM employee;
```

### Result

Displays the requested records from the table.

---

# Complete Example

## Step 1: Insert Records

```sql
INSERT INTO employee
VALUES(101,'John',45000,'HR');

INSERT INTO employee
VALUES(102,'David',50000,'IT');

INSERT INTO employee
VALUES(103,'Alice',60000,'Finance');
```

---

## Step 2: Display Records

```sql
SELECT *
FROM employee;
```

---

## Step 3: Update Salary

```sql
UPDATE employee
SET salary = 65000
WHERE emp_id = 103;
```

---

## Step 4: Display Updated Records

```sql
SELECT *
FROM employee;
```

---

## Step 5: Delete a Record

```sql
DELETE FROM employee
WHERE emp_id = 102;
```

---

## Step 6: Display Final Records

```sql
SELECT *
FROM employee;
```

---

# Difference Between DML Operations

| Operation | Purpose | WHERE Clause | Affects Structure |
|-----------|---------|--------------|-------------------|
| INSERT | Adds new rows | No | No |
| UPDATE | Modifies existing rows | Yes (Recommended) | No |
| DELETE | Removes rows | Yes (Recommended) | No |
| SELECT | Retrieves rows | Optional | No |

---

# Difference Between DML and DDL

| Feature | DML | DDL |
|----------|-----|-----|
| Purpose | Manipulates data | Defines database structure |
| Changes | Data | Structure |
| Auto Commit | No | Yes |
| Rollback | Possible before COMMIT | Generally Not Possible |
| Examples | INSERT, UPDATE, DELETE | CREATE, ALTER, DROP |


