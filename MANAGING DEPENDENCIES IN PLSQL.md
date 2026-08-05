# Managing Dependencies in Oracle PL/SQL

This repository contains notes and guidelines on how Oracle Database manages dependencies among schema objects, specifically focusing on PL/SQL program units. It covers the types of dependencies, how they are tracked, and how invalidation and recompilation occur.

---

## 📋 Table of Contents
1. [Overview of Schema Object Dependencies](#1-overview-of-schema-object-dependencies)
2. [Direct and Indirect Dependencies](#2-direct-and-indirect-dependencies)
3. [Querying Direct Object Dependencies](#3-querying-direct-object-dependencies)
4. [Invalidation of Dependent Objects](#4-invalidation-of-dependent-objects)
5. [Fine-Grained Dependency Management](#5-fine-grained-dependency-management)
6. [Guidelines for Reducing Invalidation](#6-guidelines-for-reducing-invalidation)
7. [Recompiling PL/SQL Program Units](#7-recompiling-plsql-program units)

---

## 1. Overview of Schema Object Dependencies

Some schema objects reference other objects in their definitions. For example, a view is defined by a query that references tables or other views.
* If the definition of object A references object B, then A is a **dependent object** (with respect to B) and B is a **referenced object** (with respect to A).
* **Dependency Issues:**
  * If you alter the definition of a referenced object, dependent objects may or may not continue to work.
  * Oracle automatically records dependencies among objects. To manage dependencies, all schema objects have a status (valid or invalid) recorded in the data dictionary.
  * If the status is `VALID`, the object has been compiled and can be immediately used.
  * If the status is `INVALID`, the object must be compiled before use.

### Object Type Dependencies
| Object Type | Can Be Dependent or Referenced |
| :--- | :--- |
| Package body | Dependent only |
| Package specification | Both |
| Sequence | Referenced only |
| Subprogram | Both |
| Synonym | Both |
| Table | Both |
| Trigger | Both |
| User-defined object | Both |
| User-defined collection | Both |
| View | Both |

---

## 2. Direct and Indirect Dependencies

* **Direct Dependency:** A dependent object directly references another object (e.g., Procedure A references Procedure B).
* **Indirect Dependency:** A dependent object references another object through an intermediate object (e.g., Procedure A references Procedure B, and Procedure B references View A. Therefore, Procedure A has an indirect dependency on View A).

**Managing Local Dependencies:**
When dependent objects are on the same node in the same database, the Oracle server automatically manages all local dependencies using the database's internal "depends-on" table. When a referenced object is modified, dependent objects are sometimes invalidated and automatically recompiled the next time they are called.

---

## 3. Querying Direct Object Dependencies

You can determine which objects recompile automatically by querying the data dictionary views.

* **`USER_DEPENDENCIES` View:** Displays direct dependencies from the user's schema.
* **`ALL_DEPENDENCIES` and `DBA_DEPENDENCIES`:** Contain an additional `OWNER` column.

**Key Columns in `USER_DEPENDENCIES`:**
* `NAME`: Name of the dependent object.
* `TYPE`: Type of the dependent object (PROCEDURE, FUNCTION, PACKAGE, PACKAGE BODY, TRIGGER, or VIEW).
* `REFERENCED_OWNER`: Schema of the referenced object.
* `REFERENCED_NAME`: Name of the referenced object.
* `REFERENCED_TYPE`: Type of the referenced object.
* `REFERENCED_LINK_NAME`: Database link used to access the referenced object.

### Displaying Dependencies Using DEPTREE and IDEPTREE
You can display a tabular representation of dependent objects by querying the `DEPTREE` view or an indented representation using the `IDEPTREE` view.
1. Run the `utldtree.sql` script to create the necessary objects.
2. Populate the `DEPTREE_TEMPTAB` table using the `DEPTREE_FILL` procedure.
3. Query the `DEPTREE` or `IDEPTREE` views.

---

## 4. Invalidation of Dependent Objects

Every database object has a status value (`VALID` or `INVALID`).
* If Object A depends on Object B, which depends on Object C, then A is a direct dependent of B, B is a direct dependent of C, and A is an indirect dependent of C.
* Direct dependents are invalidated only by changes to the referenced object that affect them.
* Indirect dependents can be invalidated by changes to the referenced object that do not affect them (cascading invalidation).

### Schema Object Change That Invalidates Some Dependents (Example)
If an `ALTER TABLE` statement modifies a column size, a view that selects that column will become `INVALID`. However, another view that does not select the modified column may remain `VALID`.

---

## 5. Fine-Grained Dependency Management

Starting with Oracle Database 11g, dependencies are tracked at the level of the element within the unit.
* Dependency of a single-table view on its base table is tracked at the column level.
* Dependency of a PL/SQL program unit on other PL/SQL program units, tables, or views is tracked with more precision.

This reduces the invalidation of dependent objects in response to changes to the objects on which they depend, increasing application availability.

---

## 6. Guidelines for Reducing Invalidation

* **Add new items to the end of a package:** When adding new items to a package specification, add them to the end to preserve the slot numbers and entry-point numbers of existing top-level elements.
* **Reference each table through a view:** This helps isolate procedures from changes to the underlying tables.

### Object Revalidation
* The compiler cannot automatically revalidate an object that compiled with errors.
* Recompilation occurs automatically when an object is referenced; it does not require explicit user action.
* The object is `COMPILED WITH ERRORS`, `UNAUTHORIZED`, or `INVALID` if it is not valid.

---

## 7. Recompiling PL/SQL Program Units

You can manually recompile a PL/SQL object, which forces the database to recompile the object and all of its dependents.

**Recompilation Statements:**
* `ALTER PROCEDURE [schema.]procedure_name COMPILE;`
* `ALTER FUNCTION [schema.]function_name COMPILE;`
* `ALTER PACKAGE [schema.]package_name COMPILE [PACKAGE | SPECIFICATION | BODY];`
* `ALTER TRIGGER trigger_name COMPILE [DEBUG];`

### Successful vs. Unsuccessful Recompilation
* **Successful:** If the recompilation is successful, the object becomes valid.
* **Unsuccessful:** Sometimes recompilation of dependent procedures is unsuccessful (e.g., when a referenced table is dropped).
  * The object remains invalid if the referenced object is dropped or renamed.
  * The object remains invalid if the data type of a referenced column is changed.
