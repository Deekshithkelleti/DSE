# STORED PROCEDURES AND FUNCTIONS

## Procedures and Functions Overview
*   They are named PL/SQL blocks, also known as PL/SQL subprograms.
*   They share a similar block structure with anonymous blocks (optional declarative section, mandatory executable section, optional exception handling).
*   Unlike anonymous blocks, they are compiled, stored in the database, and can be invoked (called) repeatedly.

## Subprograms
A subprogram is a program unit/module that performs a particular task. They are the basis for modular design in PL/SQL.
A subprogram can be created:
*   At the schema level (standalone)
*   Inside a package
*   Inside a PL/SQL block

PL/SQL provides two kinds of subprograms:
1.  **Functions:** Return a single value; used mainly to compute and return a value.
2.  **Procedures:** Do not return a value directly; used mainly to perform an action.

## Differences Between Anonymous Blocks and Subprograms

| Feature | Anonymous Blocks | Subprograms |
| :--- | :--- | :--- |
| **Name** | Unnamed PL/SQL blocks | Named PL/SQL blocks |
| **Compilation** | Compiled every time they are executed | Compiled only once |
| **Storage** | Not stored in the database | Stored in the database |
| **Invocation** | Cannot be invoked by other applications | Named and, therefore, can be invoked by other applications |
| **Return Value** | Do not return values | Functions must return values |
| **Parameters** | Cannot take parameters | Can take parameters |

## Creating a Procedure
The syntax for creating a procedure:
```sql
CREATE [OR REPLACE] PROCEDURE procedure_name
    [(argument1 [mode1] datatype1,
      argument2 [mode2] datatype2,
      ...)]
IS | AS
    procedure_body;
```
*   `procedure_name`: Specifies the name of the procedure.
*   `OR REPLACE`: Allows the modification of an existing procedure.
*   `mode`: Can be `IN`, `OUT`, or `IN OUT`.
*   `procedure_body`: Contains the executable part.

## Parameter Modes in PL/SQL Subprograms

| Mode | Description |
| :--- | :--- |
| **IN** | Passes a value to the subprogram. It acts as a constant (read-only) inside the subprogram and cannot be changed. This is the **default mode**. |
| **OUT** | Returns a value to the calling program. It acts as a variable inside the subprogram. Its initial value is ignored, and it must be assigned a value. |
| **IN OUT** | Passes an initial value to the subprogram and returns an updated value to the caller. |

## Creating a Function
A standalone function is created using the `CREATE FUNCTION` statement. A function is similar to a procedure except that it **must** return a value.
```sql
CREATE [OR REPLACE] FUNCTION function_name
    [(parameter_name [IN | OUT | IN OUT] type [, ...])]
RETURN return_datatype
{IS | AS}
BEGIN
    < function_body >
END [function_name];
```
*   The `RETURN` clause specifies the data type of the value returned by the function.
*   The function body must contain at least one `RETURN` statement.

## Calling a Function
To execute a function, you call it within another block or application. If it requires parameters, you pass them. Because a function returns a value, the function call is usually assigned to a variable or used as part of an expression.
