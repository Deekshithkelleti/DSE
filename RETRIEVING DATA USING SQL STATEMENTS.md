The SELECT Statement
It retrieves information from the database.

Projection: Selecting specific columns in a table that are returned by a query.
Selection: Selecting specific rows in a table that are returned by a query.
Joins: Displaying data from multiple tables using joins.
Basic Query Structures
SELECT * FROM table_name;
SELECT col1, col2... colN FROM table_name;
SELECT DISTINCT col_name FROM table_name;
SELECT col1, col2 FROM table_name WHERE condition;
SELECT col1, col2 FROM table_name GROUP BY col_name HAVING condition;
SELECT * FROM table_name ORDER BY col_name DESC;
Practical Examples
1. Creating a Table (DDL)
CREATE TABLE emp (
    emp_ID INT PRIMARY KEY,
    Ename VARCHAR2(20),
    Dept VARCHAR2(20),
    Address VARCHAR2(20)
);
2. Inserting Data (DML)
Single row insertion:

INSERT INTO emp VALUES (101, 'BOB', 'HR', 'ELURU');
Multiple row insertion (Oracle style):

INSERT ALL
    INTO emp (emp_ID, Ename, Dept, Address) VALUES (102, 'Alice', 'Clerk', 'Guntur')
    INTO emp (emp_ID, Ename, Dept, Address) VALUES (103, 'John', 'Developer', 'Vizag')
SELECT * FROM dual;

COMMIT;
3. Querying Data (DQL)
Select all columns:
SELECT * FROM emp;
Select specific column:
SELECT Ename FROM emp;
Select distinct values:
SELECT DISTINCT Dept FROM emp;
Select with a condition:
SELECT Ename, Address FROM emp WHERE emp_ID = 101;
Grouping and Filtering (Example logic):
SELECT Ename FROM emp
GROUP BY Dept
HAVING emp_ID BETWEEN 101 AND 103;
(Note: Grouping by Dept but selecting Ename directly without an aggregate function may cause errors in some SQL dialects unless Ename is functionally dependent on Dept. This is a conceptual example from notes.)
Arithmetic Expressions
You can use standard arithmetic operators (+, -, *, /) within your queries.

Example Query:

SELECT eid, esalary + 2500 AS New_Salary
FROM emp
WHERE eid = 101;
Handling NULL Values
A NULL value is not equal to '0'.
It represents missing values, unknown, or unavailable data.
Note: [column padding format A20]
To check for NULL values, use IS NULL or IS NOT NULL.

Examples:

SELECT * FROM emp WHERE Ename IS NULL;
SELECT * FROM emp WHERE Ename IS NOT NULL;
