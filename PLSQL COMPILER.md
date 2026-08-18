# PL/SQL COMPILER

## Initialization Parameters for PL/SQL Compilation
* Includes information about the use of PL/SQL compiler initialization parameters.
* Use the PL/SQL compile time warnings.
* Initialization parameters for PL/SQL compilation include `PLSQL_CODE_TYPE`, `PLSQL_OPTIMIZE_LEVEL`, and `PLSQL_WARNINGS`.

### PLSQL_CODE_TYPE
* Tells Oracle how the PL/SQL program should be compiled (decides the compilation method).
* **Two options:**
  * **Interpreted:** Oracle compiles PL/SQL into an intermediate form that is interpreted when it runs.
  * **Native:** Oracle compiles PL/SQL into native machine code for execution.
* Example syntax: `Alter session set PLSQL_CODE_TYPE = 'NATIVE';`

### PLSQL_OPTIMIZE_LEVEL
* Tells how much optimization it should perform while compiling PL/SQL.
* The levels are: `0`, `1`, `2`, `3`.
* A higher optimization level gives Oracle more opportunities to improve generated code.
* Level `2` is the normal/recommended setting.

### PLSQL_WARNINGS
* A compiler parameter.
* While compilation, Oracle may notice something that isn't necessarily an error but could be a problem such as poor performance.
* These can be communicated using compiler warnings.

## Compile Time Warnings
* Is a message Oracle gives you while compiling PL/SQL when it notices something that might be wrong or inefficient.
* This is different from normal compilation errors.
* **Error:** Oracle cannot successfully compile the program.
* **Warning:** Your program can compile, but Oracle noticed something you should check.
* Example: procedure created with compilation warnings.
* Compiler warnings help developers find problems before the program is actually being used in production.
* They can help identify performance problems, programming problems, and potential runtime problems.
* It helps find problems early.

### Warning Categories
* 1) **SEVERE:** You should pay attention (serious).
* 2) **PERFORMANCE:** Oracle suggests a change that could make a procedure execute more efficiently.
* 3) **INFORMATIONAL:** Provide useful info about the code but aren't necessarily serious problems.

### Controlling Warnings
* We use `PLSQL_WARNINGS` to control these warnings.
* **3 actions:**
  * **ENABLE:** Turn the warning on. Oracle will report it as a warning.
  * **DISABLE:** Turn the warning off. It won't report that warning.
  * **ERROR:** Treats the warning as a compilation error.
* Example: `Alter session set PLSQL_WARNINGS = 'ENABLE:SEVERE';` Enables severe warnings for the session.
* Enabling different categories:
  * `Alter session set PLSQL_WARNINGS =`
  * `'ENABLE:SEVERE'` -> show
  * `'ENABLE:PERFORMANCE'` -> show
  * `'DISABLE:INFORMATIONAL'` -> Don't show
* **Making a warning an error:** You can tell Oracle if a particular warning occurs, treat it as an error.
* A warning allows compilation; a warning configured as an error can prevent successful compilation.

## Dictionary Views for Compiler Settings
* `show errors;` shows compilation errors and warnings.
* Using dictionary views: `USER_ERRORS`, `ALL_ERRORS`, `DBA_ERRORS`.
* These contain info about compilation errors and warnings.
* Where can compilation settings be seen? `USER_PLSQL_OBJECT_SETTINGS` shows the compilation settings used for PL/SQL objects belonging to the current user.
* Other views include `ALL_PLSQL_OBJECT_SETTINGS` and `DBA_PLSQL_OBJECT_SETTINGS`.
* Helps in checking how PL/SQL objects are compiled.
* Helps find info about: Optimization level, Compilation type, Warnings, Debugging settings, and PL/SQL compiler settings.

## Changing and Recompiling with Settings
* Settings can be changed for a session or for the system.
* `Alter session set PLSQL_OPTIMIZE_LEVEL=2;`
* `Alter session` -> current session.
* `Alter system` -> system/db level.
* **Recompiling with settings:** Suppose a PL/SQL procedure was compiled using certain compiler settings, and later you change settings.
* If you simply recompile the program normally, Oracle can use current compiler settings.
* But there is an option: `Alter ... compile reuse settings;`
* `reuse settings` tells Oracle to reuse the settings that were previously stored for that PL/SQL object.

## PLSQL_CCFLAGS
* Used for conditional compilation.
* Means you can tell Oracle to compile certain parts of PL/SQL code only when particular conditions are satisfied.
* Controls flags during conditional compilation.

## Performance Related Warnings (NOCOPY)
* Compiler warnings can identify possible performance improvements such as using nocopy.
* `nocopy` is a performance related warning involving PL/SQL parameters.
* It can allow certain `OUT` or `IN OUT` parameters to be passed more efficiently, potentially moving (improving) performance for large values.
