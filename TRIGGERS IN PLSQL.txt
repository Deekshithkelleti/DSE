# PL/SQL Triggers

A comprehensive guide and quick-reference cheat sheet for Oracle PL/SQL Triggers, including System Event Triggers (Logon/Startup) and INSTEAD OF Triggers (Insert/Update/Delete) for views.

---

## 📋 Table of Contents
1. [System Event Triggers](#1-system-event-triggers)
   - [Logoff Trigger](#logoff-trigger)
   - [Startup Trigger](#startup-trigger)
2. [INSTEAD OF Triggers](#2-instead-of-triggers)
   - [Setup (Tables & View)](#setup-tables--view)
   - [INSTEAD OF INSERT](#instead-of-insert)
   - [INSTEAD OF UPDATE](#instead-of-update)
   - [INSTEAD OF DELETE](#instead-of-delete)

---

## 1. System Event Triggers

System event triggers fire when database-wide events occur, such as startup, shutdown, logon, or logoff.

### Logoff Trigger

**1. Create the Audit Table:**
```plsql
CREATE TABLE db_event_audit (
    user_name VARCHAR2(15),
    event_type VARCHAR2(30),
    logon_date DATE,
    logon_time VARCHAR2(15),
    logoff_date DATE,
    logoff_time VARCHAR2(15)
);
```

**2. Create the Trigger:**
```plsql
CREATE OR REPLACE TRIGGER db_logoff_audit
BEFORE LOGOFF ON DATABASE
BEGIN
    INSERT INTO db_event_audit VALUES (
        USER,
        ora_sysevent,
        NULL,
        NULL,
        SYSDATE,
        TO_CHAR(SYSDATE, 'hh24:mi:ss')
    );
    COMMIT;
END;
/
```
*Note: To view the audit trail, use `SELECT * FROM db_event_audit;`*

### Startup Trigger

**1. Create the Audit Table:**
```plsql
CREATE TABLE startup_audit (
    event_type VARCHAR2(30),
    event_date DATE,
    event_time VARCHAR2(15)
);
```

**2. Create the Trigger:**
```plsql
CREATE OR REPLACE TRIGGER tr_startup_audit
AFTER STARTUP ON DATABASE
BEGIN
    INSERT INTO startup_audit VALUES (
        ora_sysevent,
        SYSDATE,
        TO_CHAR(SYSDATE, 'hh24:mi:ss')
    );
END;
/
```

---

## 2. INSTEAD OF Triggers

`INSTEAD OF` triggers are used exclusively with views. They tell Oracle what underlying table DML actions to perform *instead of* trying to perform the DML directly on the view itself (which is often not possible for complex views).

### Setup (Tables & View)

**1. Create Base Tables:**
```plsql
CREATE TABLE trainer (
    full_name VARCHAR2(20)
);

CREATE TABLE subject (
    subject_name VARCHAR2(20)
);
```

**2. Insert Initial Data:**
```plsql
INSERT INTO trainer VALUES ('manish sharma');
INSERT INTO subject VALUES ('oracle');
```

**3. Create the View:**
```plsql
CREATE VIEW vw_Tulasi AS
SELECT full_name, subject_name FROM trainer, subject;
```
*Attempting `INSERT INTO vw_Tulasi VALUES ('Tulasi', 'Java');` directly will fail without an INSTEAD OF trigger.*

---

### INSTEAD OF INSERT
```plsql
CREATE OR REPLACE TRIGGER tr_io_insert
INSTEAD OF INSERT ON vw_Tulasi
FOR EACH ROW
BEGIN
    INSERT INTO trainer (full_name) VALUES (:new.full_name);
    INSERT INTO subject (subject_name) VALUES (:new.subject_name);
END;
/
```

---

### INSTEAD OF UPDATE
```plsql
CREATE OR REPLACE TRIGGER io_update
INSTEAD OF UPDATE ON vw_Tulasi
FOR EACH ROW
BEGIN
    UPDATE trainer 
    SET full_name = :new.full_name
    WHERE full_name = :old.full_name;
    
    UPDATE subject 
    SET subject_name = :new.subject_name
    WHERE subject_name = :old.subject_name;
END;
/
```

---

### INSTEAD OF DELETE
```plsql
CREATE OR REPLACE TRIGGER io_delete
INSTEAD OF DELETE ON vw_Tulasi
FOR EACH ROW
BEGIN
    DELETE FROM trainer 
    WHERE full_name = :old.full_name;
    
    DELETE FROM subject 
    WHERE subject_name = :old.subject_name;
END;
/
```
*Example usage: `DELETE FROM vw_Tulasi WHERE full_name = 'manish sharma';`*
