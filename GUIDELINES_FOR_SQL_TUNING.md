# GUIDELINES FOR SQL TUNING

## Guidelines for SQL Tuning Indexes
* How to tune SQL queries & Index -> makes SQL run faster. [cite: 9]
* **Avoid Re-parsing:** Avoid unnecessary parsing becoz parsing takes time. [cite: 9]
* Use Bind Vars, Avoid unnecessary alter/analyze. [cite: 9]
* Use consistent table aliases. [cite: 9]
* Use `where` to reduce rows. [cite: 9]
* `Distinct` -> removes duplicates -> may req. sorting. [cite: 9]
* `Group by` -> Group rows -> may req sorting. [cite: 9]
* `Order by` -> Sorts data. [cite: 9]
* `Union all` -> combine results without removing duplicates, so generally avoids the extra duplicate removal sort. [cite: 9]
* Sorting -> extra work. [cite: 9]
* **Order By** -> Use it when you actually need a particular order. [cite: 9] An appropriate index can sometimes help. [cite: 9]
* **Group By** -> Grp only cols you need. [cite: 9] Appropriate indexes may help. [cite: 9]
* Avoid `Having` when `where` can do the job. [cite: 9]

## B-Tree Index
* Common/default type of index. [cite: 9]
* Think of it like a dictionary. [cite: 9]
* Instead of checking every row, Oracle follows the Index structure to find required data. [cite: 9]
* B-Tree - General purpose index. [cite: 9]
* **B-Tree structure:** Index has Root -> Branch -> leaf. [cite: 9]
* The leaf contains the indexed value & corresponding rowid. [cite: 9]
* Rowid tells Oracle where actual row is stored. [cite: 9]

## Compressed & Reverse Key Index
* **Compressed Index** - Removes repeated vals from index, saves storage. [cite: 10]
* **Reverse Key Index** - Reverses Index key val. [cite: 10] Helps reduce index block contention in some workloads. [cite: 10] (multiple users trying to use the same db resource at same time). [cite: 10]
* A range scan means oracle uses an index to find range of vals, not just 1 exact val. [cite: 10]
* Reverse-key indexes are not suitable for range scans. [cite: 10]

## Bitmap Index
* Uses bits (0/1) to represent vals. [cite: 10]
* It is especially useful when a column has a few distinct vals. [cite: 10] (Gender -> M/F). [cite: 10]
* Bitmap = Few possible values. [cite: 10]

## B-Tree vs Bitmap

| Feature | B-Tree | Bitmap |
| :--- | :--- | :--- |
| **Suitability** | More suitable for OLTP. [cite: 10] | More suitable for DSS / data WH. [cite: 10] |
| **Purpose** | General purpose. [cite: 10] | Low cardinality cols. [cite: 10] |
| **Storage** | More index-storage in some cases. [cite: 10] | Can use less storage for suitable data. [cite: 10] |

## Function-Based Index
* You create index on a fn/expression. [cite: 10]
* Ex: `Create Index emp_ename_idx ON EMP (UPPER(ENAME));` [cite: 10]
* Then Oracle searches where `upper(ename)='JOHN'`. [cite: 10]

## Descending Index
* An index can store vals in descending order. [cite: 10]
* `Create index emp_ename_idx on emp(ename Desc);` [cite: 10]

## Monitoring Indexes
* You can monitor Indexes & collect statistics to check their condition & usage. [cite: 11]
