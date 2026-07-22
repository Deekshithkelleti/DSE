# Using Single Row Functions to Customize the Output

## Functions in SQL

Functions are used to perform operations on data and return a value.

### Types of Functions

1. **Single Row (Scalar) Functions**
2. **Group (Aggregate / Multi Row) Functions**

---

# Types of Single Row Functions

- Character Functions
- Numeric Functions
- Date Functions
- Conversion Functions
- General Functions

---

# 1. Character Functions

Character functions manipulate string values.

## UPPER()

Converts all characters to uppercase.

```sql
SELECT UPPER('aruna sri') AS Name
FROM dual;
```

**Output**

```
ARUNA SRI
```

---

## LOWER()

Converts all characters to lowercase.

```sql
SELECT LOWER('ARUNA SRI') AS Name
FROM dual;
```

**Output**

```
aruna sri
```

---

## INITCAP()

Converts the first letter of each word to uppercase.

```sql
SELECT INITCAP('aruna sri') AS Name
FROM dual;
```

**Output**

```
Aruna Sri
```

---

## LENGTH()

Returns the number of characters.

```sql
SELECT LENGTH('aruna sri') AS Name_of_the_length
FROM dual;
```

**Output**

```
9
```

---

## SUBSTR()

Extracts a substring from a string.

```sql
SELECT SUBSTR('aruna sri', 3, 8) AS Sub_String
FROM dual;
```

**Output**

```
una sri
```

---

## INSTR()

Returns the position of a character or substring.

```sql
SELECT INSTR('aruna sri', 's') AS In_String
FROM dual;
```

**Output**

```
7
```

---

## CONCAT()

Combines two strings.

```sql
SELECT CONCAT('aruna sri', ' sumaya chandran') AS FullName
FROM dual;
```

**Output**

```
aruna sri sumaya chandran
```

---

## REPLACE()

Replaces characters within a string.

```sql
SELECT REPLACE('ARUNA', 'A', 'X')
FROM dual;
```

**Output**

```
XRUNX
```

---

## LPAD()

Pads the left side of a string.

```sql
SELECT LPAD('Oracle', 10, '*') AS Padding
FROM dual;
```

**Output**

```
****Oracle
```

---

## RPAD()

Pads the right side of a string.

```sql
SELECT RPAD('Oracle', 10, '*') AS Padding
FROM dual;
```

**Output**

```
Oracle****
```

---

## TRIM()

Removes leading and trailing spaces.

```sql
SELECT TRIM('     Oracle    ')
FROM dual;
```

**Output**

```
Oracle
```

---

## LENGTH with Spaces

```sql
SELECT LENGTH('     Oracle    ')
FROM dual;
```

**Output**

```
15
```

---

# 2. Numeric Functions

Numeric functions perform mathematical operations.

## ROUND()

Rounds a number to specified decimal places.

```sql
SELECT ROUND(1567.89345, 2) AS Numbers
FROM dual;
```

**Output**

```
1567.89
```

---

## MOD()

Returns the remainder after division.

```sql
SELECT MOD(20,4)
FROM dual;
```

**Output**

```
0
```

```sql
SELECT MOD(20,7)
FROM dual;
```

**Output**

```
6
```

---

## CEIL()

Returns the smallest integer greater than or equal to the number.

```sql
SELECT CEIL(23.2)
FROM dual;
```

**Output**

```
24
```

---

## FLOOR()

Returns the largest integer less than or equal to the number.

```sql
SELECT FLOOR(23.2)
FROM dual;
```

**Output**

```
23
```

---

## ABS()

Returns the absolute value.

```sql
SELECT ABS(-23)
FROM dual;
```

**Output**

```
23
```

---

## POWER()

Returns a number raised to a power.

```sql
SELECT POWER(2,4)
FROM dual;
```

**Output**

```
16
```

---

## SQRT()

Returns the square root.

```sql
SELECT SQRT(16)
FROM dual;
```

**Output**

```
4
```

---

# 3. Date Functions

Date functions operate on date values.

## SYSDATE

Returns the current system date.

```sql
SELECT SYSDATE
FROM dual;
```

---

## CURRENT_DATE

Returns the current session date.

```sql
SELECT CURRENT_DATE
FROM dual;
```

---

## ADD_MONTHS()

Adds months to a date.

```sql
SELECT ADD_MONTHS(SYSDATE, 5)
FROM dual;
```

---

## NEXT_DAY()

Returns the next specified weekday.

```sql
SELECT NEXT_DAY(SYSDATE, 'MONDAY')
FROM dual;
```

---

## LAST_DAY()

Returns the last day of the month.

```sql
SELECT LAST_DAY(SYSDATE)
FROM dual;
```

---

## ROUND(Date)

Rounds the date.

```sql
SELECT ROUND(SYSDATE)
FROM dual;
```

---

## TRUNC(Date)

Removes the time portion.

```sql
SELECT TRUNC(SYSDATE)
FROM dual;
```

---

# 4. Conversion Functions

Conversion functions convert data from one datatype to another.

| Function | Conversion |
|----------|------------|
| TO_CHAR() | Number/Date → Character |
| TO_NUMBER() | Character → Number |
| TO_DATE() | Character → Date |

---

## TO_NUMBER()

```sql
SELECT TO_NUMBER('1989')
FROM dual;
```

**Output**

```
1989
```

---

## TO_DATE()

```sql
SELECT TO_DATE('03-aug-1989','dd-mon-yyyy')
FROM dual;
```

---

Using full month name:

```sql
SELECT TO_DATE('03-august-1989','dd-month-yyyy')
FROM dual;
```

---

## TO_CHAR()

Convert a date into a formatted string.

```sql
SELECT TO_CHAR(SYSDATE,'dd-mon-year')
FROM dual;
```

**Output**

```
09-jul-twenty twenty-six
```

---

```sql
SELECT TO_CHAR(SYSDATE,'dd-mon-yy')
FROM dual;
```

**Output**

```
09-jul-26
```

---

```sql
SELECT TO_CHAR(SYSDATE,'dd-month-yy')
FROM dual;
```

**Output**

```
09-july-26
```

---

# Summary

## Character Functions

- UPPER()
- LOWER()
- INITCAP()
- LENGTH()
- SUBSTR()
- INSTR()
- CONCAT()
- REPLACE()
- LPAD()
- RPAD()
- TRIM()

---

## Numeric Functions

- ROUND()
- MOD()
- CEIL()
- FLOOR()
- ABS()
- POWER()
- SQRT()

---

## Date Functions

- SYSDATE
- CURRENT_DATE
- ADD_MONTHS()
- NEXT_DAY()
- LAST_DAY()
- ROUND(Date)
- TRUNC(Date)

---

## Conversion Functions

- TO_CHAR()
- TO_NUMBER()
- TO_DATE()

---

# Practice Queries

```sql
-- Convert text to uppercase
SELECT UPPER('oracle sql')
FROM dual;

-- Find length of a string
SELECT LENGTH('Database')
FROM dual;

-- Round a decimal number
SELECT ROUND(45.6789, 2)
FROM dual;

-- Find remainder
SELECT MOD(50, 6)
FROM dual;

-- Current system date
SELECT SYSDATE
FROM dual;

-- Add 12 months
SELECT ADD_MONTHS(SYSDATE, 12)
FROM dual;

-- Convert string to number
SELECT TO_NUMBER('12345')
FROM dual;

-- Convert string to date
SELECT TO_DATE('15-aug-2026','dd-mon-yyyy')
FROM dual;

-- Display current date in custom format
SELECT TO_CHAR(SYSDATE,'dd-mon-yyyy')
FROM dual;
```
