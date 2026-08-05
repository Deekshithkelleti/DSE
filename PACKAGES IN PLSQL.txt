# PL/SQL Packages

Packages are stored libraries in the database which allow us to group related PL/SQL objects under one name. They are permanently stored in the database schema.

A package can include:
* Stored procedures
* PL/SQL functions
* Database cursors
* Type declarations
* Variables

---

## 🏗️ Package Architecture

A PL/SQL package is divided into two main parts:

### 1. The Package Specification (Header)
- Responsible for the declaration of all elements.
- Acts as the public interface for the package.

**Syntax:**
```plsql
CREATE OR REPLACE PACKAGE Pkg_name IS
  -- Declaration of all package elements...
END [Pkg_name];
```

### 2. The Package Body
- Contains the actual structure and implementation of the package elements.
- Can also contain private package elements not declared in the specification.

**Syntax:**
```plsql
CREATE OR REPLACE PACKAGE BODY Pkg_name IS
  -- variable declaration;
  -- type declaration;
BEGIN
  -- implementation
END [Pkg_name];
```

---

## 💻 Example Implementation

### Step 1: Create the Package Specification
```plsql
CREATE OR REPLACE PACKAGE Pkg_demo IS
  FUNCTION Print_string RETURN VARCHAR2;
  PROCEDURE proc_superhero (f_name VARCHAR2, l_name VARCHAR2);
END Pkg_demo;
```

### Step 2: Create the Package Body
```plsql
CREATE OR REPLACE PACKAGE BODY Pkg_demo IS
  
  -- Function Implementation
  FUNCTION Print_string RETURN VARCHAR2 IS
  BEGIN
    RETURN 'google.com';
  END Print_string;
  
  -- Procedure Implementation
  PROCEDURE proc_superhero (f_name VARCHAR2, l_name VARCHAR2) IS
  BEGIN
    INSERT INTO new_superheroes (f_name, l_name)
    VALUES (f_name, l_name);
  END;
  
END Pkg_demo;
```

### Step 3: Executing Package Elements

**Executing the Function:**
```plsql
SET SERVEROUTPUT ON;
BEGIN
  DBMS_OUTPUT.PUT_LINE(Pkg_demo.Print_string);
END;
/
```

**Executing the Procedure:**
```plsql
-- Assuming table new_superheroes exists
SELECT * FROM new_superheroes;

BEGIN
  Pkg_demo.proc_superhero('Black', 'Panther');
END;
/
```

---

## 📌 Additional Topics
* **Overloading subprograms:** Creating multiple procedures/functions with the same name but different parameters within a package.
* **Persistent state of package:** How variables maintain their values across transactions in a single session.
* **Using Associative Arrays in package:** Leveraging collections for complex data manipulation.
