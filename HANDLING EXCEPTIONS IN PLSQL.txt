# PL/SQL Exception Handling

Exception handling in PL/SQL allows you to write robust code by gracefully managing runtime errors.

## 📋 Types of Exceptions
There are 2 main types of exceptions in PL/SQL:
1. **System-defined Exception (Implicitly raised)**
2. **User-defined Exception**

---

## 🛠️ User-Defined Exceptions

There are 3 ways to handle user-defined exceptions:

### 1. Using Variable of EXCEPTION Type
- Declare a user-defined exception by declaring a variable of `EXCEPTION` data type in your code.
- Raise it explicitly in your program using the `RAISE` statement.

**Steps:**
1. Declare the Exception
2. Raise the Exception
3. Handle the Exception

**Example 1: Divide by Zero**
```plsql
DECLARE
  var_dividend NUMBER := 24;
  var_divisor  NUMBER := 0;
  var_res      NUMBER;
  ex_DivZero   EXCEPTION;
BEGIN
  IF var_divisor = 0 THEN
    RAISE ex_DivZero;
  END IF;
  
  var_result := var_dividend / var_divisor;
  DBMS_OUTPUT.PUT_LINE('Result: || var_result);
  
EXCEPTION 
  WHEN ex_DivZero THEN
    DBMS_OUTPUT.PUT_LINE('Error Error! - your div is zero');
END;
/
```

### 2. Using RAISE_APPLICATION_ERROR Method
- You can declare a user-defined exception with your own customized error number and message.

**Example 2: Age Validation**
```plsql
ACCEPT var_age NUMBER PROMPT 'what''s ur Age?';
DECLARE
  age NUMBER := &var_age;
BEGIN
  IF age < 18 THEN
    RAISE_APPLICATION_ERROR(-20001, 'you should be 18 or above');
  END IF;
  
  DBMS_OUTPUT.PUT_LINE('sure, would you like to have?');
  
EXCEPTION 
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE(SQLERRM); -- trace error msg last occured
END;
/
```

### 3. Using PRAGMA EXCEPTION_INIT Function
- You can map a non-predefined error number with the variable of `EXCEPTION` data type.

**Example 3: Age Validation with PRAGMA**
```plsql
DECLARE
  ex_age EXCEPTION;
  age    NUMBER := 17;
  PRAGMA EXCEPTION_INIT(ex_age, -20008);
BEGIN
  IF age < 18 THEN
    RAISE_APPLICATION_ERROR(-20008, 'you should be 18 or above');
  END IF;
  
  DBMS_OUTPUT.PUT_LINE('Sure!');
  
EXCEPTION 
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE(SQLERRM);
END;
/
```
