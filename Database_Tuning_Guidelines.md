# Database Tuning: Guidelines for Tuning SQL Queries (TM6)

## 1. What is SQL tuning?
SQL tuning means improving a SQL query so that it executes faster and uses fewer database resources. 
For example, if a table has 1 million rows, `SELECT * FROM employees;` asks Oracle to retrieve everything. 
A better query might be:
```sql
SELECT employee_id, first_name, salary
FROM employees
WHERE department_id = 10;
```
You're retrieving only what you actually need. 
**The basic idea is:** Less unnecessary work → faster query → better performance.

The module mainly covers:
* Writing efficient queries
* Avoiding unnecessary re-parsing
* Using operators/functions properly
* Using appropriate clauses
* Using indexes properly
* Writing efficient joins and subqueries

## 2. Avoid Re-parsing ⭐
When Oracle receives a SQL statement, it doesn't immediately execute it. It first has to parse it:
**SQL statement → PARSE → Execution plan → EXECUTE**

**What is parsing?**
Oracle checks things such as:
* Is the SQL syntax correct?
* Do the tables and columns exist?
* Does the user have permission?
* What execution plan should be used?

This takes processing time. If you repeatedly execute almost identical queries with different literal values, Oracle may need additional parsing.

## 3. SQL must be identical for sharing
SQL cannot be shared unless it is absolutely identical. Meaning Oracle can reuse a parsed SQL statement when the statement is the same.

**Formatting guidelines:**
* Use the same case.
* Put SQL verbs on new lines.
* Use single spaces.
* Avoid unnecessary differences in the SQL text.

**Main idea:** Identical SQL → Oracle can reuse it → less parsing → better performance.

## 4. Bind Variables ⭐⭐⭐
Instead of putting different literal values directly into SQL:
```sql
SELECT salary FROM employees WHERE employee_id = 101;
SELECT salary FROM employees WHERE employee_id = 102;
```
You can use a bind variable:
```sql
SELECT salary FROM employees WHERE employee_id = :emp_id;
```
Now `:emp_id` can contain 101, 102, etc., but the SQL structure remains the same. 

**Why is this good?**
Oracle can reuse the SQL statement instead of treating every literal value as a completely different statement, leading to less parsing work and better performance.

## 5. CURSOR_SHARING
* **CURSOR_SHARING = FORCE:** Oracle tries to replace literal values with bind variables so similar SQL statements can share.
* **CURSOR_SHARING = SIMILAR:** Historically, Oracle could share statements while considering whether different literal values might require different execution plans.

## 6. Stored Programs / Pin Objects
* **Stored programs:** Procedures, Functions, and Packages can be stored inside the database. Instead of repeatedly sending large pieces of logic from the application, the logic can reside in the database.
* **Pin objects:** Oracle can keep frequently used objects in memory so they don't have to be repeatedly loaded.

## 7. Avoid ALTER / ANALYZE when unnecessary
Avoid `ALTER`, `ANALYZE` — they can invalidate previously prepared SQL or dependent objects. During tuning, avoid unnecessary changes that force Oracle to redo work.

## 8 & 9. Table Aliases and Prefixing Column Names ⭐
Use aliases to make joins clear and avoid errors like `ORA-00918: column ambiguously defined`.
```sql
SELECT e.department_id, e.first_name, d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

## 10. Use production-like data while tuning
Make sure that a reasonable subset of production data is used during development, testing, and tuning. A query tested on 100 rows might look fast, but could be very slow on 10 million rows. Test with representative data.

## 11. Accomplish as much as possible with a single call
Instead of making many separate database requests, try to retrieve the required information in fewer calls.
```sql
-- Better to combine into one query:
SELECT first_name, salary FROM employees;
```

## 12 & 13. Use predicates to limit rows & Avoid SELECT * ⭐
* **Predicates:** A condition that filters rows (e.g., `WHERE department_id = 10`). It tells Oracle: "I only need these rows."
* **Avoid SELECT *:** Select only the columns you actually need to reduce data, memory, network transfer, and disk work.

## 14. Index-only access
Retrieving very few columns can encourage index-only access. If your query only needs columns available from an index, Oracle may be able to obtain the required information directly from the index without reading the entire table.

## 15 & 16. Operations requiring sorts & CREATE INDEX
* **Sort operations:** `DISTINCT`, `SET operators`, `GROUP BY`, and `ORDER BY` may require Oracle to sort or process data. Don't use them unless actually needed.
* **CREATE INDEX:** Always results in a sort and consumes resources (storage, maintenance). Don't randomly create indexes for every column.

## 17. UNION vs UNION ALL ⭐
* **UNION:** Removes duplicates, requiring more processing (sorting).
* **UNION ALL:** Keeps duplicates, generally faster as it skips the duplicate-removal step.

## 18 & 19. ORDER BY & GROUP BY
* **ORDER BY:** May be faster if columns are indexed, but only use it when you actually need sorted output.
* **GROUP BY:** Specify only columns that need to be grouped to prevent Oracle from processing or sorting unnecessary information.

## 20. Indexes for frequently used columns ⭐
Consider indexes for columns frequently used in `WHERE`, `GROUP BY`, `JOIN`, `ORDER BY`, and `SELECT DISTINCT`.
*But remember:* Indexes are not automatically faster and consume storage and maintenance effort during DML operations.

## 21. Same data type and length
When comparing a column to a host variable, use the same data type and length to avoid implicit data conversions that can prevent efficient use of indexes.

## 22 - 25. Joins ⭐⭐⭐
* Response time is determined mostly by the number of rows participating in the join.
* **Always provide an accurate JOIN condition.**
* **Never use a JOIN without a predicate** to avoid accidental Cartesian products.
* **Join on indexed columns** for better efficiency.

## 26. Use JOINs over subqueries
When a join can clearly and efficiently solve the problem, consider using a join over complicated nested subqueries.

## 27. Avoid <> on indexed columns
Instead of `WHERE amount <> 0`, consider `WHERE amount > 0` (if applicable). `<>` means "not equal" and can match a large portion of the table, making it difficult for Oracle to use an index efficiently.

## 28. NOT EXISTS instead of NOT IN
Consider `NOT EXISTS` instead of `NOT IN` for anti-join logic, as `NOT IN` can have tricky behavior when NULL values are involved.

## 29 & 30. Full Table Scans & Hints
* If a query is going to read most of the records in a table (e.g., > 60%), a **full table scan** is often better than using an index.
* **`/*+ FULL(table) */` hint:** Tells the Oracle optimizer to prefer a full table scan.

## 31. Combine multiple subqueries
If multiple subqueries access the same source and can logically be combined, try to combine them to reduce unnecessary database work.
```sql
WHERE (emp_cat, emp_range) = (
    SELECT MAX(category), MAX(sal_range)
    FROM emp_categories
)
```

## 32 & 33. WHERE vs HAVING ⭐⭐⭐
* **WHERE:** Filters individual rows *before* `GROUP BY`.
* **HAVING:** Filters groups *after* `GROUP BY`.
* **Rule:** Avoid `HAVING` when `WHERE` can do the job. Eliminating rows before grouping means Oracle has fewer rows to process.

## 34. Tune views too
Don't forget to tune views. A view is a stored query; if the underlying query is inefficient, using the view will also be inefficient.

---

## 🧠 Entire topic in one picture

```text
                 SQL TUNING
                     │
       ┌─────────────┴─────────────┐
       │                           │
  Avoid re-parsing            Efficient SQL
       │                           │
   Bind variables          ┌───────┼────────┐
   Same SQL                │       │        │
   Cursor sharing        WHERE   JOIN    SELECT
                          │       │        │
                       Filter   Proper    Avoid *
                       early    condition
                          │       │
                       GROUP BY  INDEX
                          │       │
                       HAVING   ORDER BY
                          │
                    Filter groups
```

## ⭐ What you should REALLY understand

| Concept | Simple Meaning |
| :--- | :--- |
| **SQL tuning** | Make SQL faster |
| **Parsing** | Oracle prepares/checks SQL before execution |
| **Re-parsing** | Avoid doing that preparation unnecessarily |
| **Bind variables** | Keep SQL structure same while changing values |
| **SELECT *** | Avoid retrieving unnecessary columns |
| **WHERE** | Filter rows early |
| **HAVING** | Filter groups |
| **Index** | Helps find/process certain data efficiently |
| **JOIN predicate** | Tells Oracle how tables are related |
| **UNION ALL** | Doesn't remove duplicates, so generally less work than UNION |

**The biggest practical rule:** Don't make Oracle process data that you don't actually need.
