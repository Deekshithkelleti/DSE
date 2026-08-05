# PL/SQL

PL/SQL is a completely portable, high-performance transaction-processing language.

## 1. Declarations:
- This section starts with the keyword `DECLARE`.
- defines all variables, cursors, subprograms and so on.

## 2. Executable commands:
- enclosed between the keyword `BEGIN` and `END`.
- It should have at least one executable line of code, which may be just a `NULL COMMAND` to indicate that nothing should be executed.

## 3. Exception handling:
- Starts with keyword `EXCEPTION`.
- optional sec contains exceptions that handle errors.

### Basic Syntax:
```sql
DECLARE
    <declaration section>
BEGIN
    <executable commands>
EXCEPTION
    <exception handling>
END;
```

**Example:-**
```sql
DECLARE
    message varchar2(20):= 'Hello, world!';
BEGIN
    dbms_output.put_line(message);
END;
/
```

---

# PL/SQL Data Types:-

- a data type specifies what kind of data a variable can store.

## Categories:
1. **Scalar**: stores single value.
   - Eg: `NUMBER`, `DATE`, `BOOLEAN`
2. **LOB (Large Object)**: store very large data
   - Eg: `BLOB`, `CLOB`
3. **Composite**: stores multiple values.
   - Eg: records, collections.
4. **Reference**: stores address/reference of another object.
   - Eg: `REF CURSOR`.

---

## Scalar

### Numeric data type:
`NUMBER`, `PLS_INTEGER`, `INTEGER`, `BINARY_INTEGER`, `FLOAT`, `REAL`, `DOUBLE PRECISION`, `DECIMAL`.

**Eg!**
```sql
DECLARE
    num1 INTEGER;
    num2 NUMBER(5,2);
    num3 REAL;
BEGIN
    num1 := 100;
    num2 := 99.99;
    num3 := 25.6;
    DBMS_OUTPUT.PUT_LINE(num1);
END;
/
```

### Character Data type:-
`CHAR`, `VARCHAR2`, `NCHAR`, `NVARCHAR2`, `RAW`, `LONG`, `LONG RAW`, `ROWID`, `UROWID`

**Example:-**
```sql
DECLARE
    name VARCHAR2(20);
BEGIN
    name := 'Tulasi';
    DBMS_OUTPUT.PUT_LINE(name);
END;
/
```

### Boolean data type:-
`TRUE`, `FALSE`, `NULL`

**Example!**
```sql
DECLARE
    flag BOOLEAN;
BEGIN
    flag := TRUE;
    if flag then
        DBMS_OUTPUT.PUT_LINE('Success');
    END if;
END;
/
```

### Date time data type:
`DATE` (stores yr, month, Day, hr, min, sec)

**Ex!**
```sql
DECLARE
    today DATE;
BEGIN
    today := SYSDATE;
    DBMS_OUTPUT.PUT_LINE(today);
END;
/
```

---

## Large Object data types:-
- `BLOB` -> binary files (images, videos)
- `CLOB` -> large text
- `NCLOB` -> unicode text
- `BFILE` -> External file outside database.

## User defined Subtypes:
- a subtype is a subset of another datatype.
**Syntax:** `SUBTYPE subtype_name IS datatype;`

**Ex!**
```sql
DECLARE
    SUBTYPE name IS CHAR(20);
    student_name name;
BEGIN
    student_name := 'Tulasi';
    DBMS_OUTPUT.PUT_LINE(student_name);
END;
/
```

## NULL in PL/SQL
- NULL means, unknown/missing value.
- Null is not -> 0, empty string, '\0'.

**Ex!**
```sql
DECLARE
    age NUMBER;
BEGIN
    age := NULL;
END;
/
```


