# Oracle PL/SQL Fundamentals - Key Notes

## 1. PL/SQL Blocks & Lexical Units
* **Nested Blocks:** PL/SQL blocks can be nested within the executable (`BEGIN...END`) or exception handling sections.
* **Lexical Units:** The building blocks of PL/SQL include:
  * **Identifiers:** Names given to PL/SQL objects (e.g., variables like `v_fname`).
  * **Delimiters:** Symbols with special syntactic meaning (e.g., `;`, `+`, `-`).
  * **Literals:** Explicit explicit data values (e.g., `'John'`, `428`, `True`). Character and date literals must be enclosed in single quotes.
  * **Comments:** Used for code documentation. Single-line (`--`) and multi-line (`/* ... */`).

## 2. Operators & Data Type Conversion
* **Operators:** Standard operators are supported, including Logical, Arithmetic, Concatenation (`||`), and Exponential (`**`).
* **Data Type Conversion:**
  * **Implicit:** Automatically handled by the PL/SQL compiler when mixed data types are used.
  * **Explicit:** Uses built-in conversion functions for clarity and reliability, such as `TO_CHAR`, `TO_DATE`, `TO_NUMBER`, and `TO_TIMESTAMP`.

## 3. SQL Functions & Sequences in PL/SQL
* **SQL Functions:** Standard single-row functions like `LENGTH()` and `MONTHS_BETWEEN()` can be used directly within PL/SQL expressions.
* **Accessing Sequences:**
  * **Oracle 11g and later:** Sequence values can be assigned directly to variables (e.g., `v_new_id := my_seq.NEXTVAL;`).
  * **Before 11g:** Retrieving a sequence required a SELECT statement (e.g., `SELECT my_seq.NEXTVAL INTO v_new_id FROM Dual;`).

## 4. Data Integrity Constraints
Constraints enforce data rules at the table level and prevent invalid data entry. They can be defined at the column level or table level.
* **NOT NULL:** Ensures the column cannot contain a null value.
* **UNIQUE:** Ensures a column or combination of columns must be unique for all rows in the table.
* **PRIMARY KEY:** Uniquely identifies each row. A primary key constraint automatically enforces uniqueness and ensures no part of the key is null.
* **FOREIGN KEY:** Establishes referential integrity to a primary or unique key in a parent table. Foreign key values must match an existing value in the parent table or be `NULL`.
  * `ON DELETE CASCADE`: Automatically deletes dependent rows in the child table when a parent row is deleted.
  * `ON DELETE SET NULL`: Converts dependent foreign key values to null when the parent row is deleted.
* **CHECK:** Specifies a condition that each row must satisfy (e.g., `status BETWEEN 0 AND 10`). 
  * *Limitation:* CHECK constraints cannot reference dynamic functions (like `SYSDATE`, `USER`) or sequence pseudocolumns (`CURRVAL`, `NEXTVAL`).
