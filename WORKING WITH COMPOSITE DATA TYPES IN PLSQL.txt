# PL/SQL Composite Data Types

## Introduction
Composite data types in PL/SQL allow you to store multiple values together as a single unit. They make programs more organized, efficient, and easier to manage.

PL/SQL provides **three types of composite data types**:

1. Records
2. Collections
3. REF Cursors

---

# 1. Records

A **Record** is a composite data type that groups related data items of different data types into a single unit.

A record works like a row in a table where each field can have a different datatype.

### Advantages
- Stores related data together.
- Makes code easier to read.
- Can hold different datatypes.
- Used to represent a complete row of data.

---

## User-Defined Record

You can create your own record using the **RECORD** keyword.

### Syntax

```sql
TYPE record_name IS RECORD
(
    field1 datatype,
    field2 datatype,
    field3 datatype
);

record_variable record_name;
```

### Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    TYPE emp_record IS RECORD
    (
        v_id NUMBER,
        v_name VARCHAR2(30),
        v_sal NUMBER
    );

    emp1 emp_record;

BEGIN
    emp1.v_id := 101;
    emp1.v_name := 'Rahul';
    emp1.v_sal := 300000;

    DBMS_OUTPUT.PUT_LINE('Employee Name : ' || emp1.v_name);
END;
/
```

### Output

```
Employee Name : Rahul
```

### Explanation

- A record named **emp_record** is created.
- It contains three fields:
  - Employee ID
  - Employee Name
  - Salary
- Values are assigned using the dot (`.`) operator.
- The employee name is displayed.

---

# Table-Based Record (%ROWTYPE)

`%ROWTYPE` is a predefined attribute that creates a record with the same structure as a table or cursor.

Instead of declaring every field manually, PL/SQL automatically creates fields corresponding to every column in the table.

---

## Advantages of %ROWTYPE

- No need to declare each column separately.
- Automatically adapts if table structure changes.
- Reduces coding effort.
- Useful for fetching complete rows.

---

## Syntax

```sql
record_variable table_name%ROWTYPE;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_emp employee%ROWTYPE;

BEGIN
    SELECT *
    INTO v_emp
    FROM employee
    WHERE employee_id = 200;

    DBMS_OUTPUT.PUT_LINE(v_emp.first_name);
END;
/
```

### Explanation

- `v_emp` stores an entire row from the **employee** table.
- `SELECT * INTO` fetches one row.
- Individual columns are accessed using the dot (`.`) operator.

Example:

```sql
v_emp.first_name
v_emp.salary
v_emp.department_id
```

---

# 2. Collections

Collections store **multiple values of the same datatype**.

Unlike records, every element in a collection must have the same datatype.

Collections are useful when handling multiple values together.

---

## Types of Collections

PL/SQL supports three collection types:

1. Associative Array
2. Nested Table
3. VARRAY

---

# A. Associative Array

An Associative Array stores elements as **key-value pairs**.

The index can be:

- Integer
- String

The size grows dynamically.

---

## Syntax

```sql
TYPE type_name IS TABLE OF datatype
INDEX BY PLS_INTEGER;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    TYPE marks IS TABLE OF NUMBER
    INDEX BY PLS_INTEGER;

    student marks;

BEGIN
    student(1) := 90;
    student(2) := 85;

    DBMS_OUTPUT.PUT_LINE(student(1));
END;
/
```

### Output

```
90
```

### Explanation

- An associative array named **student** is created.
- Keys:
  - 1
  - 2
- Values:
  - 90
  - 85
- The first value is printed.

---

# B. Nested Table

A Nested Table is an unordered collection.

Its size increases dynamically.

It can also be stored inside database tables.

---

## Syntax

```sql
TYPE type_name IS TABLE OF datatype;
```

---

## Features

- Dynamic size
- No maximum limit
- Elements can be deleted
- Suitable for database storage

---

# C. VARRAY (Variable Size Array)

A VARRAY is an ordered collection.

A maximum size must be specified during declaration.

---

## Syntax

```sql
TYPE type_name IS VARRAY(max_size)
OF datatype;
```

---

## Example

```sql
DECLARE
    TYPE subjects IS VARRAY(5) OF VARCHAR2(20);

    sub subjects := subjects('Java','SQL','HTML');

BEGIN
    DBMS_OUTPUT.PUT_LINE(sub(1));
END;
/
```

### Output

```
Java
```

---

## Difference Between Collection Types

| Feature | Associative Array | Nested Table | VARRAY |
|----------|-------------------|--------------|---------|
| Ordered | No | No | Yes |
| Dynamic Size | Yes | Yes | Limited |
| Maximum Size | No | No | Yes |
| Stored in Database | No | Yes | Yes |
| Indexed By | Integer/String | Integer | Integer |

---

# 3. REF CURSORS

A **REF CURSOR** is a cursor variable.

It points to the result set of a SQL query.

Unlike explicit cursors, REF CURSORS are dynamic because they can be opened for different SQL queries during execution.

---

## Advantages

- Dynamic queries
- Returns different result sets
- Used in stored procedures
- Commonly used between applications and Oracle Database

---

## Syntax

```sql
TYPE emp_cur IS REF CURSOR;

c emp_cur;
```

---

## Example

```sql
DECLARE
    TYPE emp_cur IS REF CURSOR;

    c emp_cur;
BEGIN
    OPEN c FOR
    SELECT *
    FROM employee;

    CLOSE c;
END;
/
```

### Explanation

- A REF CURSOR type is declared.
- Cursor variable `c` is created.
- Cursor is opened for a query.
- Cursor is closed after use.

---
