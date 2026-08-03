# Using DDL statements to create and manage tables

## 1. Table Structures and Naming Rules:
- Table: Basic unit of storage, composed of rows.
- View: Logically represents subsets of data from one or more tables.
- Sequence: Generates numeric values.
- Index: Improves performance of some queries.
- Synonym: Gives alternative name to an object.

### Oracle Table Structures:
- Tables can be created at any time, even while users using database.
- No need to specify size of table.
- size defined by amt of space allocated to db as a whole.

### Naming Rules:-
- Begin with letter.
- Be 1-30 character long.
- Contain only A-Z, a-z, 0-9, _, $, and #.
- Not be an Oracle server reserved word.
- Not duplicate the name of another object owned by same user.
- We can use quoted identifier to represent name of object.
- If you name schema object using a quoted identifier you must use double quotation marks while refering.
- Quoted identifiers can be reserved word.
- Not case sensitive.
- Referencing another user's Table:- using user name as a prefix to those Tables.

### Default:-
- should be a literal, expression or sql fns are legal values.
- Another col's name or a pseudocol are illegal vall.
- The default datatype must match col datatype.
- Note! In SQL Developer, pressing F5 would run DDL statements. Feedback msgs will be shown on the script output tabbed page.

---

## 2. Table Creation and Data types:-
- To confirm that the table was created, run DESCRIBE command.
- Becox creating a table is a DDL statement, an automatic commit takes place when this stat is executed.

### Data types:-
- Varchar2(size) - Variable-length character data (minimum: 1, maximum 4000).
- char(size) - Fixed-length character data.
- Number(p,s) - Variable-length numeric data.
- Date - Date and time values.
- LONG - Variable-length character data (upto 2GB).
- CLOB - Character data (Up to 4 GB).
- RAW and LONG RAW - Raw binary data.
- BLOB - Binary data (up to 4GB).
- BFILE - Binary data stored in external file (up to 4 GB).
- ROWID - A base 64 number system representing unique address of a row in its table.
- char [(size)] - min=1, max=2000.
- Number [(p,s)] - precision - total no. of decimal digits, scale - no. of digits to right of decimal pt. Precision range - 1 to 38, scale - -84 to 127.
- Date - Date & time values to the nearest second between Jan 1, 4712 B.C., & Dec 31, 9999 A.D.
- TIMESTAMP - Date with fractional sec's.
- INTERVAL YEAR TO MONTH - stored as an interval of years and months.
- INTERVAL DAY TO SECOND - stored as an interval of days, min, secs.
- Varchar2(50) = upto 50 characters capacity.

---

## 3. Violating constraints:-
- When you have constraints in place on cols, an error is returned if you try to violate the constraint rule.
- If we try to update a record tied to integrity Constraint error is returned.

**Example:-**
```sql
Update employees
set department_id=55
where department_id=110
```
- Oracle can't assign an employee to a dept that doesn't exist so update fails.
- Parent does not exist - Trying to insert/update a foreign key that has no matching parent.

### Delete parent Row:
```sql
Delete from Dept
where dept_id=60
```
- If dept 60 deleted, where will John belong? Nowhere. Oracle prevents this.
- ORA-02292 child record found.
- Delete emp first, then del dept.

---

## 4. Create table using subquery:-
- CTAS

**Ex!**
```sql
Create table emp_copy
as
select * from emp;
```
- Oracle creates new table, copies structure, copies data.
- Why CTAS? Instead of Create table & Insert we do both together.
- If cols specifieds, no. of cols must match.

**Ex!**
```sql
Create table emp(id, name)
as
select emp_id, last_name from employee;
```

- Only not null constraint is copied.
- primary key, Foreign key, Unique, check, Default constraints not copied.
- ORA-00998: Must name this exp as col alias.

**Example:-**
```sql
Create table emp80
As
select salary*12 as annual_salary
from Employee
```

**Ex!**
```sql
Create table ord2458 As
select order_id,
       Order_date,
       Order_status,
       customer_id
from Orders
Where Order_id = 2458;
```
- Copies only rows where id=2458.
- If it is an expression, not a col name Oracle does not know what to name this col in new table.

---

## 5. Including Constraints:-
- Enforces rules at table level. prevents deletion of table if there are dependencies.
- Prevents invalid data entry.
- Constraint Types: NOT NULL, UNIQUE, PK, FK, CHECK.

### Constraint Description:
- NOT NULL: specifies col cannot contain a null val.
- UNIQUE: specifies a col or combo of cols whose vals must be unique for all rows in table.
- PK: Uniquely identifies each row in table.
- FK: Enforces a referential integrity b/w col & a col of referenced table such that vals in 1 table match vals in another table.
- CHECK: specifies a condition that must be true.

- A constraint can be named or Oracle server generates one using SYS_Cn format.
- Constraint can be created during table creation and also after table creation.
- Constraint defined at table level or column level.
- Constraint viewed in data dictionary.
- In sys_Cn -> n is integer so constraint name is unique.
- Constraint name cannot be same as the object owned by another user.

### Defining Constraints:-
1. **Column level constraint** -> written immediately after column definition. Used for 1 col only.
**Ex!**
```sql
Create table student (id NUMBER PRIMARY KEY,
EName varchar2(20) NOT NULL);
```

2. **Table Level Constraint** -> written at end of create table statement.
- Used for 1 or more cols.
**Ex!**
```sql
Create table student (sid INT, Ename varchar2(20),
Constraint pk_student PRIMARY KEY(sid));
```
- NOT NULL defined only at column level.

### DEFAULT expression:-
- If no value given Oracle automatically uses Default value.
**Ex!**
```sql
Create table emp ( Age INT default 0);
```

---

## 6. Constraints Deep Dive:-

### NOT NULL:
- Ensures that a col cannot store NULL values.
- A PK automatically has a NOT NULL constraint.
- If tried to insert null vals -> ORA-01400

### UNIQUE:
- ensures duplicates are not allowed in col or combo of cols.
- Every val must be different.
- Allows null vals unless not null constraint specified.
- Allows null vals because oracle treats NULL as an unknown value.
- If you don't want null, email varchar2(50) UNIQUE NOT NULL.
- Why table level? Mainly used when you want a composite unique key (multiple cols together must be unique).

**Ex!**
```sql
Create table stud (
   rollno INT,
   section CHAR(1),
   Constraint unique_stud UNIQUE(rollno, section)
);
```
- Oracle automatically enforces a unique index to enforce unique constraint.

### Primary key:
- Uniquely identifies each row no duplicates, no nulls, only 1 PK per table.

### Foreign Key:
- Creates relationship b/w 2 tables. Refers to PK of another table.

**Ex! Parent Table**
```text
Dept        EMP
10 HR       101 10
20 Sales    102 20
30 IT
```
- Insert into emp values (103, 30) -> Allowed
- Insert into emp values (104, 50) -> fails, does not exist in parent table.

**Col level:**
```sql
Create table employee(
   emp_id Number,
   dept_id Number references department(dept_id)
);
```

**Table level:**
```sql
Create table emp(
   emp_id INT,
   dept_id INT,
   Constraint fk_dept
   Foreign key(dept_id) References department(dept_id)
);
```

### Check:-
- A check constraint limits the values that can be stored in a column.
- Only values satisfying the condition are allowed.
- Age INT Check (age >=18)

---

## 7. ALTER AND DROP:-
- keeping a table in read-only mode, which prevents DDL or DML changes during table maintenance.
- Put the table back into read/write mode.

**Example:-**
```sql
Alter table emp read only;
Alter table emp read write;
```

- Drop table table_name; -> Moves table to recycle bin
- Drop table emp Purge; -> table is permanently deleted.

### Flashback:-
- If you dropped table without purge, you can recover using this.
```sql
Flashback table emp to before drop;
```

### ALTER COMMANDS:-
```sql
ALTER table job_role
Rename column role_name to Job_title;

ALTER Table job_role
Rename To job_title;
