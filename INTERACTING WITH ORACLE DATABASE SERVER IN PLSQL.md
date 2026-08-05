# Interacting with Oracle Database Server in PL/SQL

## SQL Statements in PL/SQL
*   **Retrieve Data**: Use the `SELECT` command to retrieve a single row from the database.
*   **Manipulate Data**: Use DML commands (`INSERT`, `UPDATE`, `DELETE`, `MERGE`) to modify database rows.
*   **Transaction Control**: Manage transactions using `COMMIT`, `ROLLBACK`, or `SAVEPOINT` commands.

## SELECT Statements in PL/SQL
*   The `INTO` clause is **mandatory**.
*   Queries must return **only one row**.
*   **Syntax:**
    ```sql
    SELECT select_list
    INTO {variable_name[, variable_name]... | record_name}
    FROM table
    [WHERE condition];
    ```

## Naming Conventions and Ambiguity
*   Use distinct naming conventions for PL/SQL variables to avoid ambiguity in `WHERE` clauses.
*   **Avoid** using database column names as identifiers.
*   If a naming conflict occurs, PL/SQL prioritizes checking the database table for a column matching the name first.

## SQL Cursors
A cursor is a pointer to a private memory area allocated by the Oracle Server, used to process `SELECT` statement result sets.

*   **Implicit Cursors**: Created and managed automatically by the Oracle Server for all DML statements and single-row `SELECT` statements.
*   **Explicit Cursors**: Declared and managed explicitly by the programmer to handle queries returning multiple rows.

### SQL Cursor Attributes for Implicit Cursors
You can use cursor attributes to evaluate the outcome of SQL statements. Attributes are prefixed with `SQL`.

*   `SQL%FOUND`: Returns `TRUE` if the most recent SQL statement affected at least one row.
*   `SQL%NOTFOUND`: Returns `TRUE` if the most recent SQL statement did not affect any rows.
*   `SQL%ROWCOUNT`: Returns an integer representing the number of rows affected by the most recent SQL statement.

## Data Manipulation (DML) in PL/SQL
*   **INSERT**: Adds new rows to a table.
*   **UPDATE**: Modifies existing rows in a table.
*   **DELETE**: Removes rows from a table.
*   **MERGE**: Selects and updates or inserts rows in one table based on data from another table, depending on an equijoin condition.
