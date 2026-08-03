# SQL Notes: Joins and Advanced Subqueries

## 1. Relational Algebra & Joins
In databases, data is usually stored in multiple tables. Joins combine two or more rows based on related columns.

* **Inner Join:** Returns only rows that have a matching value in both tables.
* **Equi Join:** Uses the equality operator. Inner join can be an equi join when the condition uses `=`.
* **Left Outer Join:** All rows from the left table + matching rows from the right table.
* **Right Outer Join:** All rows from the right table + matching rows from the left table.
* **Full Outer Join:** All rows from both tables. Included with nulls.
* **Cross Join:** Every row from table A × every row from table B (Cartesian product).
* **Self Join:** Joining a table to itself.

## 2. Advanced SQL: Subqueries
Using subqueries to solve queries: You can write subqueries in the `WHERE` clause of another SQL statement to obtain values based on an unknown conditional value.

> **Example Problem:** Find out who earns a salary greater than Abel's salary.
> To solve this, two queries are needed: *how much Abel earns*, and *who earns greater than Abel*. We solve this by combining 2 queries; placing one query inside another.

* The **inner query (subquery)** gives a value that is used by the **outer query (main query)**.
* The subquery executes before the main query; its result is used by the main query.

### Syntax
```sql
SELECT select_list
FROM table
WHERE expr operator
    (SELECT select_list
     FROM table);
```

Useful when you need to select rows from a table with a condition that depends on data in the table itself. You can use a subquery in SQL clauses such as: `WHERE`, `HAVING`, `FROM`.

* **Comparison conditions for 2 categories:**
    * Single row: `=`, `>`, `>=`, `<`, `<=`, `<>`
    * Multiple row: `IN`, `ANY`, `ALL`, `EXISTS`

### Example:
```sql
SELECT cust_fname || ' ' || cust_lname
FROM customers
WHERE credit_limit >
    (SELECT credit_limit
     FROM customers
     WHERE cust_name = 'Abel');
```

*Tip:* Place subqueries on the right side for readability, although they can be placed on either side of the comparison operator.

* **Single row subqueries:** returns only 1 row from the inner select.
* **Multiple row subqueries:** returns > 1 row from the inner select.
* There are also multiple column subqueries.

## 3. Single Row Subqueries
Returns a single row result and uses a single row operator.

**Example:** Display the employee whose Job ID is the same as that of employee 141.
```sql
SELECT last_name, job_id
FROM emp
WHERE job_id =
    (SELECT job_id
     FROM emp
     WHERE emp_id = 141);
```

**Example with multiple inner blocks:**
```sql
SELECT cust_id, cust_fname AS "last name"
FROM customers
WHERE cust_id =
    (SELECT cust_id FROM customers WHERE cust_fname = 'meenakshi')
AND credit_limit =
    (SELECT credit_limit FROM customers WHERE cust_fname = 'meenakshi');
```
Here there are 3 blocks - a Main query block & 2 inner blocks. Both inner queries result in single values only (108, 700). The inner & outer queries can get data from different tables.

### Using Group Functions in a Subquery
Show order details where order total is minimum:
```sql
SELECT order_id, order_mode, order_total
FROM orders
WHERE order_total =
    (SELECT MIN(order_total)
     FROM orders);
```

## 4. HAVING Clause with Subqueries
Parentheses tell Oracle it is a subquery. Because when a range of aggregate functions arrives, we use `HAVING`.

```sql
SELECT dept_id, MIN(salary)
FROM emp
GROUP BY dept_id
HAVING MIN(salary) >
    (SELECT MIN(salary)
     FROM emp
     WHERE dept_id = 50);
```

**Example:**
```sql
SELECT job_id, AVG(salary)
FROM employee
GROUP BY job_id
HAVING AVG(salary) =
    (SELECT MIN(AVG(salary))
     FROM employees
     GROUP BY job_id);
```

## 5. Common Errors with Subqueries
* A common error with subqueries occurs when more than 1 row is returned for a single row subquery.
* No rows returned by the inner query.

```sql
SELECT order_id, order_mode, order_total
FROM orders
WHERE order_mode =
    (SELECT order_mode
     FROM orders
     WHERE order_id = 1000);
```
If `order_id = 1000` does not exist, the inner query returns null. The outer query evaluates to "no rows selected".
**What to do?** Check for the condition where `order_id` is NULL, use `NVL`, or use `EXISTS` to check for NULL.

## 6. Multiple Row Subqueries
Returns more than 1 row. Uses multiple row comparison operators.

* **IN:** equal to any member in the list.
* **ANY:** Must be preceded by `=`, `!=`, `>`, `<`, `<=`, `>=`. Compares a value to each value in a list. Evaluates as false if the query returns no rows.
* **ALL:** Must be preceded by `=`, `!=`, `>`, `<`, `<=`, `>=`. Compares a value to every value in a list returned by a query. Evaluates to true if the query returns no rows.

**Queries that return > 1 row:**
```sql
SELECT last_name, sal, dept_id
FROM employees
WHERE sal IN
    (SELECT MIN(salary)
     FROM emp
     GROUP BY dept_id);
```

### The ANY Operator
```sql
SELECT emp_id, last_name, job_id, salary
FROM emp
WHERE sal < ANY
    (SELECT sal
     FROM employees
     WHERE job_id = 'IT_PROG')
AND job_id <> 'IT_PROG';
```
* `< ANY` : Less than the maximum
* `> ANY` : More than the minimum
* `= ANY` : Equivalent to `IN`

### The ALL Operator
* `> ALL` : More than the maximum
* `< ALL` : Less than the minimum

## 7. The EXISTS Operator
Used where the query result depends on whether or not certain rows exist in a table. It evaluates to `TRUE` if the subquery returns at least one row.

* **EXISTS:** Does subquery return at least 1 row? If Yes -> True. If No -> False.
* **NOT EXISTS:** If it returns False -> True.

```sql
SELECT *
FROM orders
WHERE NOT EXISTS
    (SELECT *
     FROM order_items
     WHERE order_items.order_id = orders.order_id);
```
This returns rows (e.g., 102, 104) where the condition doesn't exist; it returns true where no rows are selected in the inner query.

## 8. Null Values in Subqueries
```sql
SELECT emp.last_name
FROM employees emp
WHERE emp.employee_id NOT IN
    (SELECT mgr.manager_id
     FROM employees mgr);
```

If one of the values returned by the inner query is a null value, the entire `NOT IN` query returns no rows.
* `NOT IN` is equivalent to `<> ALL`.
* `IN` is equivalent to `= ANY`.

If the subquery gets all manager IDs (e.g., 100, 101, 102, NULL), the outer query becomes `WHERE employee_id NOT IN (100, 101, 102, NULL)`. If you are trying to "show employees who are not managers", this will result in "0 rows selected" because of the NULL value.

**Solution: Remove NULLs**
```sql
SELECT emp.last_name
FROM employees emp
WHERE emp.employee_id NOT IN
    (SELECT manager_id
     FROM employees
     WHERE manager_id IS NOT NULL);
```
