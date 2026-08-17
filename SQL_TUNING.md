# SQL Tuning

## SQL Tuning Process
* Identify poorly tuned SQL stats. [cite: 1]
* Tune individual stats. [cite: 1]
* Tune appln as whole. [cite: 1]

We use SQL Tuning advisors to identify SQL stats using most resources & tune SQL stats using most resources. [cite: 1]
We use SQL Access advisor to tune workload. [cite: 1]

### Advisors:
* 1) ADDM [cite: 1]
* 2) Memory advisors [cite: 1]
* 3) SQL Advisors [cite: 1]
* 4) Automatic undo management [cite: 1]
* 5) MTTR advisor [cite: 1]
* 6) SQL performance analyzer [cite: 1]
* 7) Data recovery advisor [cite: 1]
* 8) Segment advisor [cite: 1]
* 9) Streams performance advisor. [cite: 1]

---

## Automatic SQL Tuning
Suppose you have SQL queries running slowly. [cite: 1] Oracle checks those and automatically finds ways to improve slow SQL. [cite: 1] It runs during system maintenance windows. [cite: 1]

### SQL Profile
Info that helps oracle choose better execution plan for a SQL query. [cite: 2]
* It is like guidance for oracle. [cite: 2]

### Automatic SQL Tuning Checks:
* A - Automatic SQL Tuning [cite: 2]
* I - Whether an index could help [cite: 2]
* S - Whether db statistics need improvement [cite: 2]
* R - Restructure SQL (suggest changing SQL statement itself). [cite: 2]

### Implement Automatic Tuning Recommendations:
Oracle finds better way to run SQL query it recommends. [cite: 2] DBA can click on implement to apply it. [cite: 2]

---

## Comprehensive SQL Tuning
* 1) Detect stale or missing statistics [cite: 2]
* 2) Tune SQL plan (SQL profile) [cite: 2]
* 3) Add missing index [cite: 2]
* 4) Restructure SQL [cite: 2]

### SQL Tuning Advisor
Analyzes SQL & gives performance suggestions. [cite: 2]
It gets SQL stats from 3 sources: [cite: 2]
* 1. Top Activity - checks SQL that is currently active / running. [cite: 2]
* 2. SQL Tuning Sets (STS) - checks set of SQL stats that you provide. [cite: 2]
* 3. Historical SQL (AWR) - checks old/past SQL stats stored in AWR snapshots - Taking a photo of db's performance at particular time. [cite: 2]

**Options:**
We can schedule SQL tuning advisor to run. [cite: 3]
Two analysis modes: [cite: 3]
* 1) Limited -> Quick, fewer recommendations [cite: 3]
* 2) Comprehensive -> deeper analysis, may take longer. [cite: 3]

You can analyze Top activity, AWR or SQL Tuning Sets. [cite: 3]

**Recommendations:**
After analyzing SQL, Oracle shows recommendation. [cite: 3] It may recommend: SQL profile, Index, statistics, Restructure SQL. [cite: 3]
* Ex. Usage: This shows top activity. [cite: 3]
* Oracle displays SQL stats that are using system resources. [cite: 3]
* select SQL stat -> run SQL Tuning advisor -> Get recommendations. [cite: 3]

**Duplicate SQL:**
Sometimes applns send almost the same SQL many times. [cite: 3] Here Oracle suggests Bind variable. [cite: 3]

---

## SQL Access Advisor
Looks at how data is accessed & suggests better db structures. [cite: 3]
It can recommend: [cite: 3]
* 1) Indexes [cite: 3]
* 2) Materialized views [cite: 3]
* 3) Materialized view logs. [cite: 3]
* 4) partitioning [cite: 3]

### Initial Options
* checks a workload and suggests better access structures. [cite: 4]
* How to access data faster. [cite: 4]

### Workload Source
You need to tell the advisor which SQL stats to analyze. [cite: 4]
Sources: [cite: 4]
* Current / Recent SQL Activity -> SQL from cache. [cite: 4]
* SQL Tuning Set - existing set of SQL. [cite: 4]
* Schemas / Tables - creating a hypothetical workload. [cite: 4]

*(Note on Workload specification: SQL Stat -> Tuning set -> Cache content -> statistics -> schema name -> Analyze [cite: 3])*

### Recommendation Options
Choose what Oracle should recommend: Indexes, materialized views, partitioning [cite: 4]
Also choose: [cite: 4]
* Limited -> faster, focus on expensive SQL [cite: 4]
* Comprehensive SQL -> checks everything in detail. [cite: 4]

### Advanced Options
Extra settings to control analysis. [cite: 4]
* Ex: Consider unused access structures, Limit additional storage space, Categorize workload [cite: 4]
* These control how the advisor perform its analysis. [cite: 4]

### Default Storage Locations
Tells Oracle where to store recommended Objects. [cite: 4]
* Index - Index table space [cite: 4]
* Materialized view - materialized view tablespace [cite: 4]
* Materialized view log - log tablespace [cite: 4]

### Review Recommendations
After analysis, Oracle shows final recommendation. [cite: 4]
* Analyze -> Get recomm -> Review -> Implement. [cite: 4]

---

## SQL Performance Analyzer (SPA)
SPA checks what happens to my SQL performance if I change smthing in the db? [cite: 4]
* Predicts impact of system changes [cite: 5]
* Compares SQL execution plans [cite: 5]
* Compares execution statistics [cite: 5]
* Finds performance diff's. [cite: 5]
* Can work with SQL Tuning advisor. [cite: 5]

### Key Characteristics
* Targeted users: DBA's, QA's, appln developers. [cite: 5]
* Build diff. versions of SQL workload performance. [cite: 5]
* Executes SQL serially [cite: 5]
* Offers fine grained performance analysis on individual SQL. [cite: 5]
* Is integrated with SQL Tuning advisor to tune regressions. [cite: 5]

### SPA Beneficial in Use Cases:
* 1) Db upgrades [cite: 5]
* 2) Implementation of tuning recommendations. [cite: 5]
* 3) schema changes [cite: 5]
* 4) statistics Gathering [cite: 5]
* 5) Db parameter changes [cite: 5]
* 6) OS & H/W changes. [cite: 5]

### Usage of SPA:
* 1) Capture SQL workload on production. [cite: 5]
* 2) Transport SQL workload to a test sys. [cite: 5]
* 3) Build "before-change" performance data. [cite: 5]
* 4) Make changes. [cite: 5]
* 5) Build "after-change" performance data. [cite: 5]
* 6) Compare results from step 3 & 5 [cite: 5]
* 7) Tune regressed SQL. [cite: 5]
