# Writing Control Structures in PL/SQL

## PL/SQL Control Statements

### Overview
Control statements in PL/SQL are used to control the flow of program execution based on conditions. They allow a program to make decisions and execute different blocks of code depending on whether a condition is **TRUE** or **FALSE**.

The primary control statements in PL/SQL are:
- IF Statement
- IF-THEN-ELSE Statement
- IF-THEN-ELSIF Statement
- CASE Expression

### Types of Control Statements
| Control Statement | Purpose |
|-------------------|---------|
| IF | Executes a block if a condition is TRUE |
| IF-THEN-ELSE | Executes one block if TRUE and another if FALSE |
| IF-THEN-ELSIF | Checks multiple conditions sequentially |
| CASE | Selects one of several alternatives based on a value |

#### 1. IF Statement
The **IF** statement executes a block of code only when the specified condition evaluates to **TRUE**.
```sql
IF condition THEN
    statement1;
    statement2;
    ...
END IF;
```

#### 2. IF-THEN-ELSE Statement
The **IF-THEN-ELSE** statement executes one block when the condition is TRUE and another block when the condition is FALSE.
```sql
IF condition THEN
    statements;
ELSE
    statements;
END IF;
```

#### 3. IF-THEN-ELSIF Statement
The **IF-THEN-ELSIF** statement is used when multiple conditions need to be evaluated one after another. The first condition that evaluates to TRUE is executed.
```sql
IF condition1 THEN
    statements;
ELSIF condition2 THEN
    statements;
ELSE
    statements;
END IF;
```

#### 4. CASE Expression
A **CASE** expression evaluates a single variable or expression against multiple possible values and returns the corresponding result for the first matching condition.
```sql
CASE expression
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ...
    ELSE default_result
END;
```

---

## PL/SQL Iterative Statements (Loops)

### Overview
Iterative statements (Loops) in PL/SQL are used to execute a block of code repeatedly until a specified condition is met.

The main types of loops in PL/SQL are:
- Simple LOOP
- WHILE LOOP
- FOR LOOP (Numeric FOR LOOP, Cursor FOR LOOP)

### Types of Loops
| Loop | Purpose |
|------|---------|
| Simple LOOP | Repeats until an EXIT statement is encountered |
| WHILE LOOP | Repeats while a condition is TRUE |
| Numeric FOR LOOP | Executes for a fixed range of numbers |
| Cursor FOR LOOP | Iterates through rows returned by a cursor |

#### 1. Simple LOOP
A **Simple LOOP** repeatedly executes a block of statements until an **EXIT** statement is encountered. The condition is checked **inside** the loop.
```sql
LOOP
    statements;
    EXIT WHEN condition;
END LOOP;
```

#### 2. WHILE LOOP
A **WHILE LOOP** executes a block of statements **as long as** the specified condition remains TRUE. The condition is checked **before** each iteration.
```sql
WHILE condition LOOP
    statements;
END LOOP;
```

#### 3. FOR LOOP
A **FOR LOOP** executes a block of statements a fixed number of times.

**Numeric FOR LOOP:**
```sql
FOR counter IN lower..upper LOOP
    statements;
END LOOP;
```

**Cursor FOR LOOP:**
```sql
DECLARE
    CURSOR cursor_name IS
        SELECT columns FROM table;
BEGIN
    FOR record IN cursor_name LOOP
        statements;
    END LOOP;
END;
```
