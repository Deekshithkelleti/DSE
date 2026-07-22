# Day 3: Keys, Functional Dependencies, and Normalization

## Types of Keys
There are 7 main types of keys:
1. Super Key
2. Candidate Key
3. Primary Key
4. Foreign Key
5. Composite Key
6. Compound Key
7. Surrogate Key

### 1) Super Key
* It includes all possible keys in a table.
* They can exist both individually & in combinations.
* *Example Attributes:* `SID`, `REGISTRATION_ID`, `SEMAIL`
* *Super Keys:* `{SID, REGISTRATION_ID}`, `{SID, SEMAIL}`, `{SID, REGISTRATION_ID, SEMAIL}`

### 2) Candidate Key
* They are the smallest group of attributes that can uniquely identify rows.
* *Example:* From the Super Keys above, the Candidate Keys (CK) could be `{SID}`, `{SEMAIL}`, `{REGISTRATION_ID}`.

### 3) Primary Key (PK)
* Uniquely identifies a row in a table.
* *Example PK:* `{REGISTRATION_ID}`

### 4) Foreign Key (FK)
* Connects two tables and forms referential integrity.
* It can be an attribute in one table that refers to the primary key of another.
* *Example:* * `Branch` table: `Branch code` (PK), `Branch Name`
  * `Student` table: `Sname`, `Branch code` (FK) -> refers to `Branch code` in `Branch` table.

### 5) Composite Key
* It consists of multiple attributes that, together, uniquely identify a row.
* *Example Composite Key:* `{SEMAIL, SID}`, `{REG_ID, SEMAIL}`

### 6) Compound Key
* Here, at least one attribute is a foreign key.
* *Example Compound Key:* `{SBRANCH (FK), SID}`

### 7) Surrogate Key
* It is an artificially created key created when there is no natural attribute that can act as a primary key.
* *Example:* If a student table didn't have a unique attribute, we could create a new column and use it as a surrogate key.

---

## Functional Dependency (FD)
* FD is a set of constraints between two attributes in a relation.
* It specifies that the value of one attribute or set of attributes uniquely determines the value of another attribute.
* **Notation:** `X -> Y`
  * `X` -> Determinant
  * `Y` -> Dependent variable (attribute)
* *Example:* `Student_ID -> Student_Name, Department`

### Armstrong's Axioms
* These are a set of inference rules used to derive all possible functional dependencies from a given set of functional dependencies.
* The complete set of derived dependencies is called the **closure** of functional dependencies, denoted by `F+`.

1. **Reflexive Rule:** If `β ⊆ α`, then `α -> β`.
   * *Example:* `(A, B) -> A`, `(A, B, C) -> (A, C)`
2. **Augmentation Rule:** If `A -> B`, then for any attribute set `Y`, `AY -> BY`.
   * *Example:* If `A -> B`, then `AC -> BC`.
3. **Transitivity Rule:** If `A -> B` and `B -> C`, then `A -> C`.
   * *Example:* `Std_ID -> Dept` and `Dept -> HOD`. Therefore, `Std_ID -> HOD`.

### Types of Functional Dependencies
1. **Trivial Functional Dependency:** A functional dependency `X -> Y` is called trivial if `Y` is a subset of `X` (`Y ⊆ X`).
   * *Example:* `(A, B) -> A` or `(Std_ID, Name) -> Name`
2. **Non-Trivial Functional Dependency:** A functional dependency `X -> Y` is called non-trivial if `Y` is not a subset of `X` (`Y ⊄ X`).
3. **Completely Non-Trivial Functional Dependency:** A functional dependency `X -> Y` is completely non-trivial if `X ∩ Y = Φ` (empty set). There are no common attributes between `X` and `Y`.

---

## Normalization
* It is a method to remove all anomalies and bring the database to a consistent state.

### 1. First Normal Form (1NF)
* A table is in 1NF if every column contains only atomic (single) values.
* There are no repeating groups or multi-valued attributes.
* Each row should be uniquely identifiable using a PK.
* *Example:* If a row has `Skills` as `Java, SQL`, it must be split into multiple rows so each cell holds only one skill.

### 2. Second Normal Form (2NF)
* It is already in 1NF.
* It has **no partial dependency**.
* **Partial Dependency:** Occurs when a non-key attribute depends only on part of a composite PK instead of the whole key.
* *Example:* * Table with PK `(RollNo, Subject)` and attribute `Std_Name`. `Std_Name` only depends on `RollNo` (partial dependency).
  * *After 2NF:* Split into `Student Table (RollNo, Std_Name)` and `Std_Subject_Table (RollNo, Subject)`.

### 3. Third Normal Form (3NF)
* It is already in 2NF.
* There is **no transitive dependency**.
* **Transitive Dependency:** Occurs when a non-key attribute depends on another non-key attribute instead of directly depending on the PK.
* *Example:* * Table: `Emp_ID` (PK), `Dept`, `Dept_loc`. (`Dept_loc` depends on `Dept`, not directly on `Emp_ID`).
  * *After 3NF:* Split into `Emp Table (Emp_ID, Dept)` and `Dept Table (Dept, Dept_loc)`.

### 4. Boyce-Codd Normal Form (BCNF)
* It is already in 3NF.
* For every functional dependency (`X -> Y`), `X` **must be a candidate key** (or super key).
* *Example:* * Table: `Std_ID`, `Course`, `Faculty`. Functional dependency: `Course -> Faculty`.
  * *After BCNF:* Split into `Course Table (Course, Faculty)` and `Std_Course Table (Std_ID, Course)`.
