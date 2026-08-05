# PL/SQL Basics: Operators, Constants, and Lexical Units

## Overview

PL/SQL provides various **operators**, **constants**, **literals**, and **lexical units** that help in writing efficient and readable programs.

Understanding these basic concepts is essential before learning advanced PL/SQL programming.

---

# 1. PL/SQL Operators

## Overview

Operators are special symbols used to perform operations on variables, constants, and expressions.

### Types of Operators

| Operator Type | Purpose |
|--------------|---------|
| Arithmetic | Performs mathematical calculations |
| Relational | Compares two values |
| Comparison | Tests values against conditions |
| Logical | Combines multiple conditions |

---

# 1. Arithmetic Operators

## Purpose

Arithmetic operators perform mathematical calculations.

| Operator | Description | Example |
|----------|-------------|---------|
| + | Addition | 10 + 5 = 15 |
| - | Subtraction | 10 - 5 = 5 |
| * | Multiplication | 10 * 5 = 50 |
| / | Division | 10 / 5 = 2 |
| ** | Exponentiation | 2 ** 3 = 8 |

### Example

```sql
DECLARE
    a NUMBER := 20;
    b NUMBER := 5;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Addition : ' || (a + b));
    DBMS_OUTPUT.PUT_LINE('Subtraction : ' || (a - b));
    DBMS_OUTPUT.PUT_LINE('Multiplication : ' || (a * b));
    DBMS_OUTPUT.PUT_LINE('Division : ' || (a / b));
    DBMS_OUTPUT.PUT_LINE('Power : ' || (b ** 2));
END;
/
```

---

# 2. Relational Operators

## Purpose

Relational operators compare two values and return TRUE or FALSE.

| Operator | Description |
|----------|-------------|
| = | Equal to |
| != | Not Equal to |
| <> | Not Equal to |
| ~= | Not Equal to |
| > | Greater Than |
| < | Less Than |
| >= | Greater Than or Equal To |
| <= | Less Than or Equal To |

### Example

```sql
DECLARE
    a NUMBER := 10;
    b NUMBER := 20;
BEGIN
    IF a < b THEN
        DBMS_OUTPUT.PUT_LINE('a is less than b');
    END IF;
END;
/
```

---

# 3. Comparison Operators

## Purpose

Comparison operators compare values using ranges, patterns, or lists.

| Operator | Description |
|----------|-------------|
| LIKE | Pattern matching |
| BETWEEN | Checks a range |
| IN | Checks multiple values |
| IS NULL | Checks for NULL values |

### Example

```sql
SELECT *
FROM emp
WHERE ename LIKE 'S%';
```

### Example

```sql
SELECT *
FROM emp
WHERE sal BETWEEN 2000 AND 5000;
```

### Example

```sql
SELECT *
FROM emp
WHERE deptno IN (10,20);
```

### Example

```sql
SELECT *
FROM emp
WHERE comm IS NULL;
```

---

# 4. Logical Operators

## Purpose

Logical operators combine multiple conditions.

| Operator | Description |
|----------|-------------|
| AND | All conditions must be TRUE |
| OR | At least one condition must be TRUE |
| NOT | Reverses the condition |

### Example

```sql
DECLARE
    age NUMBER := 22;
BEGIN
    IF age >= 18 AND age <= 60 THEN
        DBMS_OUTPUT.PUT_LINE('Eligible');
    END IF;
END;
/
```

---

# Operator Precedence

PL/SQL evaluates operators according to precedence.

| Priority | Operator |
|----------|----------|
| 1 | ** (Exponentiation) |
| 2 | *, / |
| 3 | +, - |
| 4 | Relational Operators |
| 5 | NOT |
| 6 | AND |
| 7 | OR |

### Example

```sql
DECLARE
    result NUMBER;
BEGIN
    result := 2 + 3 * 4;
    DBMS_OUTPUT.PUT_LINE(result);
END;
/
```

### Output

```
14
```

---

# 2. Constants and Literals

## Constants

### Purpose

A constant stores a value that **cannot be changed** after initialization.

### Syntax

```sql
constant_name CONSTANT datatype := value;
```

### Example

```sql
DECLARE
    PI CONSTANT NUMBER := 3.141592;
BEGIN
    DBMS_OUTPUT.PUT_LINE(PI);
END;
/
```

### Characteristics

- Value cannot be modified.
- Must be initialized during declaration.
- Improves code readability.
- Used for fixed values.

---

# Literals

## Overview

A literal is a fixed value written directly in the program.

It is **not represented by an identifier**.

---

## Types of Literals

| Literal Type | Example |
|-------------|---------|
| Numeric | 100, 25.5 |
| Character | 'A' |
| String | 'Tulasi' |
| Boolean | TRUE, FALSE |
| Date | DATE '2026-08-01' |

---

### Numeric Literal

```sql
DECLARE
    num NUMBER := 100;
BEGIN
    DBMS_OUTPUT.PUT_LINE(num);
END;
/
```

---

### Character Literal

```sql
DECLARE
    grade CHAR := 'A';
BEGIN
    DBMS_OUTPUT.PUT_LINE(grade);
END;
/
```

---

### String Literal

```sql
DECLARE
    name VARCHAR2(20) := 'Tulasi';
BEGIN
    DBMS_OUTPUT.PUT_LINE(name);
END;
/
```

---

### Boolean Literal

```sql
DECLARE
    flag BOOLEAN := TRUE;
BEGIN
    IF flag THEN
        DBMS_OUTPUT.PUT_LINE('Success');
    END IF;
END;
/
```

---

### Date Literal

```sql
DECLARE
    d DATE := DATE '2026-08-01';
BEGIN
    DBMS_OUTPUT.PUT_LINE(d);
END;
/
```

---

# 3. Lexical Units

## Overview

Lexical units are the smallest individual elements of a PL/SQL program.

They are the basic building blocks recognized by the PL/SQL compiler.

---

## Types of Lexical Units

| Lexical Unit | Description |
|--------------|-------------|
| Identifiers | Names of PL/SQL objects |
| Delimiters | Symbols used to separate program elements |
| Literals | Fixed values |
| Comments | Notes ignored by the compiler |

---

# Identifiers

## Purpose

Identifiers are names given to PL/SQL objects such as variables, constants, cursors, procedures, and functions.

### Rules for Identifiers

- Must begin with a letter.
- Can contain letters, numbers, _, $, and #.
- Cannot contain spaces.
- Cannot exceed 30 characters.
- Cannot use reserved keywords (unless quoted).

### Valid Identifiers

```sql
emp_name
salary
student1
```

### Quoted Identifiers

```sql
DECLARE
    "begin Date" DATE;
    "end Date" DATE;
    "exception thrown" BOOLEAN DEFAULT TRUE;
BEGIN
    NULL;
END;
/
```

### Variable Assignment

```sql
DECLARE
    v_name VARCHAR2(20);
BEGIN
    v_name := 'John';

    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
```

---

# Comments

## Purpose

Comments are used to explain code.

The PL/SQL compiler ignores comments.

---

## Single-Line Comment

```sql
-- This is a single-line comment

DECLARE
    num NUMBER := 10;
BEGIN
    DBMS_OUTPUT.PUT_LINE(num);
END;
/
```

---

## Multi-Line Comment

```sql
/*
This is a
multi-line comment
*/

DECLARE
    name VARCHAR2(20) := 'Tulasi';
BEGIN
    DBMS_OUTPUT.PUT_LINE(name);
END;
/
```
