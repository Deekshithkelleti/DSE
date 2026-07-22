Day 2: Advanced DBMS Concepts

## Codd's 12 Rules
* **Rule-1: Information Rule:** Data stored in a database, whether it be user data or metadata, must be a value of some table cell.
* **Rule-2: Guaranteed Access Rule:** Every single value is guaranteed to be accessible logically with a combination of table-name, primary key (row), and attribute-name (column value).
* **Rule-3: Systematic Treatment of NULL Values:** Used when data is missing, data is not known, or data is not applicable.
* **Rule-4: Active Online Catalog:** Data dictionary which can be accessed by authorized users.
* **Rule-5: Comprehensive Data Sub-Language Rule:** A database can only be accessed using a language having linear syntax that supports data definition, data manipulation, and transaction management operations.
* **Rule-6: View Updating Rule:** Any view which can theoretically be updated, must also be updatable by the system.
* **Rule-7: High-Level Insert, Update, and Delete Rule:** This must not be limited to a single row; it must also support union, insertion, and minus operations to yield sets of data records.
* **Rule-8: Physical Data Independence:** Data storage in a database must be independent of applications that access the database.
* **Rule-9: Logical Data Independence:** Logical data in a database must be independent of its user's view (application).
* **Rule-10: Integrity Independence:** The database must be independent of the application that uses it.
* **Rule-11: Distribution Independence:** The end users must not be able to see that the data is distributed over various locations.
* **Rule-12: Non-Subversion Rule:** No security bypass is allowed while using low-level access.

---

## Relational Data Model
This model is simple and has all the properties and capabilities required to process data with storage efficiency.
* **Table:** Stores data in rows and columns.
* **Tuple:** One single row (record).
* **Relation Instance:** Current records in the table.
* **Relation Key:** An attribute that can uniquely identify a row.
* **Relation Schema:** Table structure. *Example: `Student(Name, Age, Id)`*
* **Attribute Domain:** The allowed range of values for an attribute.

---

## Constraints
1. **Key Constraints:** Every table contains at least one key.
   * A key attribute can't have a NULL value.
   * Key values must uniquely identify each row.
   * If a table contains distinct key values, it is known as a **Candidate Key (CK)**.
2. **Domain Constraints:** Every attribute contains a specific range of values.
3. **Referential Integrity Constraints:** Works on the concept of a Foreign Key.
   * **Foreign Key:** A key attribute of a relation that can be referred to in another relation (Primary Key).

---

## Relational Algebra
* It is a procedural query language.
* It takes relations as inputs and gives relations as outputs.
* Performs queries by using operators (Unary, Binary).

### Fundamental Operations:
1. **Select (`σ`)**
   * Selects tuples that satisfy the given predicate from a relation.
   * **Notation:** `σ_p(r)` (where `σ` = selection predicate, `p` = condition, `r` = relation)
   * *Example:* `σ_{subject="database"}(Books)` -> Selects tuples from Books where subject is database.
   * *Example 2:* `σ_{subject="database" AND price="450"}(Books)`
2. **Project (`π`)**
   * Projects columns that satisfy a given predicate.
   * **Notation:** `π_{A1, A2... An}(r)` (where `A1, A2... An` are attribute names of relation `r`)
   * *Example:* `π_{subject, author}(Books)` -> Selects and projects columns named subject and author from the relation Books.
3. **Union (`∪`)**
   * Performs a binary union between two given relations.
   * **Notation:** `r ∪ s = {t | t ∈ r OR t ∈ s}`
   * *Conditions:* * `r` and `s` must have the same number of attributes.
     * Attribute domains must be compatible.
     * Duplicate tuples are automatically eliminated.
   * *Example:* `π_{author}(Books) ∪ π_{author}(Articles)`
4. **Set Difference (`-`)**
   * The result of a set difference operation is tuples which are present in one relation but are not in the second relation.
   * **Notation:** `r - s`
   * *Example:* `π_{author}(Books) - π_{author}(Articles)`
5. **Cartesian Product (`×`)**
   * Combines information of two different relations into one.
   * **Notation:** `r × s = {qt | q ∈ r AND t ∈ s}`
   * *Example:* `σ_{author='tutorialpoint'}(Books × Articles)`
6. **Rename (`ρ`)**
   * Allows us to rename the output relation.
   * **Notation:** `ρ_x(E)` -> Result of expression `E` is saved with the name `x`.

---

## Relational Calculus
* It tells *what* to do but never explains *how* to do it (Non-procedural).
1. **Tuple Relational Calculus (TRC):**
   * Filtering variable ranges over tuples.
   * **Notation:** `{T | Condition}`
   * *Example:* `{T.name | Author(T) AND T.article = 'database'}`
   * *Output:* Returns tuples with 'name' from Author who has written an article on 'database'.
2. **Domain Relational Calculus (DRC):**
   * Filtering variable uses the domain of attributes instead of entire tuple values.
   * **Notation:** `{a1, a2... an | P(a1, a2... an)}` (where `a1... an` = attributes, `P` = formula built by inner attributes)
   * *Example:* `{Article, Page, Subject | ∈ Tutorialpoint ∧ Subject = 'db'}`

---

## Convert ER Model to Relational Model
* **Mapping Entity:** An entity (e.g., `Student`) with attributes (`Name`, `Roll No`, `Class`, `Subject`) becomes a table where attributes become columns.
* **Mapping Relationship:** Entities (e.g., `Student` and `Course`) connected by a relationship (`Enrolled`) with its own attributes (`Marks`, `JoinDate`). The relationship often becomes a junction table linking the primary keys.
* **Mapping Weak Entity Set:** A strong entity (e.g., `Student`) connected via a identifying relationship (`Depends`) to a weak entity (e.g., `Dept`). The weak entity's table includes its partial key and the primary key of the strong entity.
* **Mapping Hierarchical Entities:** A superclass entity (`Person` with `Name`, `Age`, `Gender`) connected via an `ISA` relationship to subclasses (`Student` with `Roll No`, `Teacher` with `EmpID`). Handled by creating tables for the superclass and subclasses, linking them via the primary key.
