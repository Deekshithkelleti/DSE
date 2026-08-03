# Using DDL statements to create and manage tables

## 1. Table Structures and Naming Rules

*   **Table:** Basic unit of storage, composed of rows.
*   **View:** Logically represents subsets of data from one or more tables.
*   **Sequence:** Generates numeric values.
*   **Index:** Improves performance of some queries.
*   **Synonym:** Gives an alternative name to an object.

### Oracle Table Structures
*   Tables can be created at any time, even while users are using the database.
*   No need to specify the size of the table.
*   Size is defined by the amount of space allocated to the database as a whole.

### Naming Rules
*   Must begin with a letter.
*   Must be 1-30 characters long.
*   Must contain only `A-Z`, `a-z`, `0-9`, `_`, `$`, and `#`.
*   Must not duplicate the name of another object owned by the same user.
*   Must not be an Oracle server reserved word.
*   We can use quoted identifiers to represent the name of an object.
*   If you name a schema object using a quoted identifier, you must use double quotation marks while referring to it.
*   Quoted identifiers can be reserved words.
*   Not case sensitive.

**Referencing another user's table:** Using user name as a prefix to those tables.

### Default Values
*   Should be a literal, expression, or SQL functions (these are legal values).
*   Another column's name or a pseudocolumn are illegal values.
*   The default data type must match the column datatype.

> **Note:** In SQL Developer, pressing `F5` would run DDL statements. Feedback messages will be shown on the script output tabbed page.

---

## 2. Table Creation and Data Types

*   To confirm that the table was created, run the `DESCRIBE` command.
*   Because creating a table is a DDL statement, an automatic commit takes place when this statement is executed.

### Data Types
*   `VARCHAR2(size)`: Variable-length character data (minimum: 1, maximum: 4000).
*   `CHAR(size)`: Fixed-length character data.
*   `NUMBER(p, s)`: Variable-length numeric data.
*   `DATE`: Date and time values.
*   `LONG`: Variable-length character data (up to 2GB).
*   `CLOB`: Character data (up to 4 GB).
*   `RAW` and `LONG RAW`: Raw binary data.
*   `BLOB`: Binary data (up to 4GB).
*   `BFILE`: Binary data stored in external file (up to 4 GB).
*   `ROWID`: A base 64 number system representing the unique address of a row in its table.
*   `CHAR [(size)]`: min = 1, max = 2000.
*   `NUMBER [(p,s)]`: precision - total no. of decimal digits, scale - no. of digits to the right of the decimal point. Precision range: 1 to 38, scale: -84 to 127.
*   `DATE`: Date & time values to the nearest second between Jan 1, 4712 B.C., & Dec 31, 9999 A.D.
*   `TIMESTAMP`: Date with fractional seconds.
*   `INTERVAL YEAR TO MONTH`: Stored as an interval of years and months.
*   `INTERVAL DAY TO SECOND`: Stored as an interval of days, minutes, seconds.
*   `VARCHAR2(50)`: Up to 50 characters capacity.

---

## 3. Violating Constraints
When you have constraints in place on columns, an error is returned if you try to violate the constraint rule. If we try to update a record tied to an integrity constraint, an error is returned.

**Example:**
```sql
UPDATE employees
SET department_id = 55
WHERE department_id = 110;
```
*Oracle can't assign an employee to a department that doesn't exist, so the update fails. (Parent does not exist - Trying to insert/update a foreign key that has no matching parent).*

### Delete Parent Row
```sql
DELETE FROM Dept
WHERE dept_id = 60;
```
*(If Dept 60 is deleted, where will employees like John or Sam belong? Nowhere. Oracle prevents this.)*

**Error:** `ORA-02292: child record found.`
*Solution:* Delete the employee (child) first, then delete the department (parent).

---

## 4. Create Table Using Subquery (CTAS)

```sql
CREATE TABLE emp_copy AS
SELECT * FROM emp;
```
*   Oracle creates a new table, copies the structure, and copies the data.
*   **Why CTAS?** Instead of `CREATE TABLE` and `INSERT`, we do both together.
*   If columns are specified, the number of columns must match.

```sql
CREATE TABLE emp(id, name) AS
SELECT emp_id, last_name FROM employee;
```

### Constraint Copying in CTAS
*   **Only** the `NOT NULL` constraint is copied.
*   Primary key, Foreign key, Unique, Check, and Default constraints are **not copied**.

### ORA-00998 Error
"Must name this expression with a column alias." This happens when you use an expression in a CTAS statement without an alias.

**Incorrect:**
```sql
CREATE TABLE emp80 AS
SELECT salary * 12
FROM employee;
```

**Correct:**
```sql
CREATE TABLE emp80 AS
SELECT salary * 12 AS annual_salary
FROM employee;
```
