# Performance Tuning in Oracle

## Index

1. [Optimizing SQL Queries](#1-optimizing-sql-queries)
   - [Using Indexes](#using-indexes)
   - [Avoiding Full Table Scans](#avoiding-full-table-scans)
   - [Using Bind Variables](#using-bind-variables)
   - [Using Appropriate WHERE Clauses](#using-appropriate-where-clauses)
   - [Using EXISTS instead of IN](#using-exists-instead-of-in)
   - [Using Analytical Functions](#using-analytical-functions)
   - [Eliminating Unnecessary Columns in SELECT](#eliminating-unnecessary-columns-in-select)
   - [Using Optimizer Hints](#using-optimizer-hints)
   - [Optimizing Subqueries](#optimizing-subqueries)
   - [Using UNION ALL instead of UNION](#using-union-all-instead-of-union)
2. [Optimizing Joins](#2-optimizing-joins)
   - [Choosing the Right Join Type](#choosing-the-right-join-type)
   - [Using Nested Loops, Hash Joins, and Merge Joins](#using-nested-loops-hash-joins-and-merge-joins)
   - [Using Join Conditions Properly](#using-join-conditions-properly)
   - [Optimizing Join Order](#optimizing-join-order)
   - [Using Outer Joins Wisely](#using-outer-joins-wisely)
   - [Using Inline Views](#using-inline-views)
   - [Indexing Join Columns](#indexing-join-columns)
   - [Avoiding Cartesian Joins](#avoiding-cartesian-joins)
   - [Using Materialized Views](#using-materialized-views)
   - [Using Partitioned Tables in Joins](#using-partitioned-tables-in-joins)
3. [Partitioning Tables](#3-partitioning-tables)
   - [Range Partitioning](#range-partitioning)
   - [List Partitioning](#list-partitioning)
   - [Hash Partitioning](#hash-partitioning)
   - [Composite Partitioning](#composite-partitioning)
   - [Interval Partitioning](#interval-partitioning)
   - [Reference Partitioning](#reference-partitioning)
   - [Subpartitioning](#subpartitioning)
   - [Global Indexes on Partitioned Tables](#global-indexes-on-partitioned-tables)
   - [Local Indexes on Partitioned Tables](#local-indexes-on-partitioned-tables)
   - [Managing Partition Maintenance Operations](#managing-partition-maintenance-operations)
4. [Optimizing PL/SQL Code](#4-optimizing-plsql-code)
   - [Using BULK COLLECT](#using-bulk-collect)
   - [Using FORALL](#using-forall)
   - [Avoiding Slow-by-Slow Processing](#avoiding-slow-by-slow-processing)
   - [Using Native Compilation](#using-native-compilation)
   - [Using PL/SQL Function Result Cache](#using-plsql-function-result-cache)
   - [Using Collection Methods](#using-collection-methods)
   - [Using Pipelined Table Functions](#using-pipelined-table-functions)
   - [Using the RETURNING Clause](#using-the-returning-clause)
   - [Profiling PL/SQL Code](#profiling-plsql-code)
   - [Minimizing Context Switches](#minimizing-context-switches)
5. [Using SQL Plan Management](#5-using-sql-plan-management)
   - [SQL Plan Baselines](#sql-plan-baselines)
   - [SQL Profiles](#sql-profiles)
   - [SQL Patches](#sql-patches)
   - [SQL Plan Directives](#sql-plan-directives)
   - [Adaptive Execution Plans](#adaptive-execution-plans)
   - [Managing SQL Plan Evolution](#managing-sql-plan-evolution)
   - [Using SQL Tuning Sets](#using-sql-tuning-sets)
   - [Managing SQL Plan Baselines](#managing-sql-plan-baselines)
   - [Using SQL Performance Analyzer](#using-sql-performance-analyzer)
   - [Monitoring SQL Execution Plans](#monitoring-sql-execution-plans)
6. [Using Hints](#6-using-hints)
   - [Index Hint](#index-hint)
   - [Parallel Hint](#parallel-hint)
   - [No_Parallel Hint](#no_parallel-hint)
   - [Full Hint](#full-hint)
   - [Merge Hint](#merge-hint)
   - [Use_NL Hint](#use_nl-hint)
   - [Use_Hash Hint](#use_hash-hint)
   - [Use_Merge Hint](#use_merge-hint)
   - [Leading Hint](#leading-hint)
   - [Ordered Hint](#ordered-hint)
7. [Monitoring and Analyzing Performance](#7-monitoring-and-analyzing-performance)
   - [Using AWR Reports](#using-awr-reports)
   - [Using SQL Trace and TKPROF](#using-sql-trace-and-tkprof)
   - [Using V$ Views](#using-v-views)
   - [Using Enterprise Manager](#using-enterprise-manager)
   - [Using ASH Reports](#using-ash-reports)
   - [Using ADDM Reports](#using-addm-reports)
   - [Monitoring Wait Events](#monitoring-wait-events)
   - [Using Statspack](#using-statspack)
   - [Monitoring Session Activity](#monitoring-session-activity)
   - [Using Automatic Database Diagnostics Monitor](#using-automatic-database-diagnostics-monitor)
8. [Optimizing Indexes](#8-optimizing-indexes)
   - [Rebuilding Indexes](#rebuilding-indexes)
   - [Using Bitmap Indexes](#using-bitmap-indexes)
   - [Using Function-Based Indexes](#using-function-based-indexes)
   - [Dropping Unused Indexes](#dropping-unused-indexes)
   - [Indexing Foreign Keys](#indexing-foreign-keys)
   - [Using Reverse Key Indexes](#using-reverse-key-indexes)
   - [Using Compressed Indexes](#using-compressed-indexes)
   - [Monitoring Index Usage](#monitoring-index-usage)
   - [Creating Invisible Indexes](#creating-invisible-indexes)
   - [Using Global and Local Indexes](#using-global-and-local-indexes)
9. [Managing Memory](#9-managing-memory)
   - [Configuring the SGA and PGA](#configuring-the-sga-and-pga)
   - [Using Automatic Memory Management](#using-automatic-memory-management)
   - [Optimizing the Buffer Cache](#optimizing-the-buffer-cache)
   - [Optimizing the Shared Pool](#optimizing-the-shared-pool)
   - [Optimizing the Large Pool](#optimizing-the-large-pool)
   - [Optimizing the Java Pool](#optimizing-the-java-pool)
   - [Using the Memory Advisor](#using-the-memory-advisor)
   - [Managing PGA Memory](#managing-pga-memory)
   - [Monitoring Memory Usage](#monitoring-memory-usage)
   - [Avoiding Paging and Swapping](#avoiding-paging-and-swapping)
10. [Optimizing Storage](#10-optimizing-storage)
    - [Using Automatic Storage Management (ASM)](#using-automatic-storage-management-asm)
    - [Using Table Compression](#using-table-compression)
    - [Managing Tablespaces](#managing-tablespaces)
    - [Using Datafile Autoextend](#using-datafile-autoextend)
    - [Using Direct Path Load](#using-direct-path-load)
    - [Optimizing Redo and Undo](#optimizing-redo-and-undo)
    - [Using Data Deduplication](#using-data-deduplication)
    - [Using SecureFiles for LOBs](#using-securefiles-for-lobs)
    - [Monitoring Tablespace Usage](#monitoring-tablespace-usage)
    - [Using Storage Tiering](#using-storage-tiering)

## 1. Optimizing SQL Queries

### Using Indexes

Indexes can greatly improve query performance by allowing faster retrieval of records.

```sql
CREATE INDEX idx_employee_name ON employees (name);
```

### Avoiding Full Table Scans

Avoid full table scans by ensuring that queries use indexes.

```sql
SELECT name FROM employees WHERE employee_id = 100;
```

### Using Bind Variables

Bind variables help Oracle to reuse execution plans and reduce parsing time.

```sql
SELECT name FROM employees WHERE employee_id = :emp_id;
```

### Using Appropriate WHERE Clauses

Ensure WHERE clauses are selective to filter out unnecessary rows early.

```sql
SELECT name FROM employees WHERE department_id = 10 AND status = 'ACTIVE';
```

### Using EXISTS instead of IN

EXISTS can be more efficient than IN for subqueries.

```sql
SELECT name FROM employees WHERE EXISTS (SELECT 1 FROM departments WHERE departments.department_id = employees.department_id);
```

### Using Analytical Functions

Use analytical functions for complex calculations over partitions.

```sql
SELECT department_id, AVG(salary) OVER (PARTITION BY department_id) AS avg_salary FROM employees;
```

### Eliminating Unnecessary Columns in SELECT

Retrieve only necessary columns to reduce I/O.

```sql
SELECT name FROM employees WHERE department

_id = 10;
```

### Using Optimizer Hints

Use optimizer hints to influence the execution plan.

```sql
SELECT /*+ INDEX(emp emp_idx) */ name FROM employees emp WHERE employee_id = 100;
```

### Optimizing Subqueries

Rewrite subqueries to use joins or EXISTS for better performance.

```sql
SELECT name FROM employees e WHERE EXISTS (SELECT 1 FROM departments d WHERE d.department_id = e.department_id);
```

### Using UNION ALL instead of UNION

Use UNION ALL if you do not need to eliminate duplicates.

```sql
SELECT name FROM employees WHERE department_id = 10
UNION ALL
SELECT name FROM employees WHERE status = 'ACTIVE';
```

## 2. Optimizing Joins

### Choosing the Right Join Type

Select the appropriate join type based on the data and query requirements.

```sql
SELECT e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Using Nested Loops, Hash Joins, and Merge Joins

Use different join methods based on the data size and indexes.

```sql
SELECT /*+ USE_NL(e d) */ e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Using Join Conditions Properly

Ensure join conditions are properly specified.

```sql
SELECT e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Optimizing Join Order

Order tables in the FROM clause to start with the most selective conditions.

```sql
SELECT e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id WHERE e.status = 'ACTIVE';
```

### Using Outer Joins Wisely

Use outer joins only when necessary to avoid unnecessary processing.

```sql
SELECT e.name, d.department_name FROM employees e LEFT JOIN departments d ON e.department_id = d.department_id;
```

### Using Inline Views

Use inline views to simplify complex queries and improve readability.

```sql
SELECT name, avg_salary FROM (SELECT name, AVG(salary) OVER (PARTITION BY department_id) AS avg_salary FROM employees);
```

### Indexing Join Columns

Index columns used in join conditions for faster lookups.

```sql
CREATE INDEX idx_employee_department ON employees (department_id);
```

### Avoiding Cartesian Joins

Ensure proper join conditions to avoid Cartesian joins.

```sql
SELECT e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Using Materialized Views

Use materialized views to precompute and store complex join results.

```sql
CREATE MATERIALIZED VIEW mv_employee_department AS SELECT e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Using Partitioned Tables in Joins

Partition tables to improve join performance on large datasets.

```sql
CREATE TABLE employees (
  employee_id NUMBER,
  name VARCHAR2(100),
  department_id NUMBER,
  status VARCHAR2(10)
) PARTITION BY HASH (department_id) PARTITIONS 4;
```

## 3. Partitioning Tables

### Range Partitioning

Partition tables based on a range of values.

```sql
CREATE TABLE sales (
  sale_id NUMBER,
  sale_date DATE,
  amount NUMBER
) PARTITION BY RANGE (sale_date) (
  PARTITION p1 VALUES LESS THAN (TO_DATE('2021-01-01', 'YYYY-MM-DD')),
  PARTITION p2 VALUES LESS THAN (TO_DATE('2022-01-01', 'YYYY-MM-DD'))
);
```

### List Partitioning

Partition tables based on a list of values.

```sql
CREATE TABLE employees (
  employee_id NUMBER,
  name VARCHAR2(100),
  department_id NUMBER,
  status VARCHAR2(10)
) PARTITION BY LIST (status) (
  PARTITION p1 VALUES ('ACTIVE'),
  PARTITION p2 VALUES ('INACTIVE')
);
```

### Hash Partitioning

Partition tables using a hash function.

```sql
CREATE TABLE orders (
  order_id NUMBER,
  order_date DATE,
  customer_id NUMBER
) PARTITION BY HASH (customer_id) PARTITIONS 4;
```

### Composite Partitioning

Combine multiple partitioning methods.

```sql
CREATE TABLE sales (
  sale_id NUMBER,
  sale_date DATE,
  region_id NUMBER,
  amount NUMBER
) PARTITION BY RANGE (sale_date) SUBPARTITION BY HASH (region_id) (
  PARTITION p1 VALUES LESS THAN (TO_DATE('2021-01-01', 'YYYY-MM-DD')),
  PARTITION p2 VALUES LESS THAN (TO_DATE('2022-01-01', 'YYYY-MM-DD'))
);
```

### Interval Partitioning

Automatically create new partitions based on interval.

```sql
CREATE TABLE orders (
  order_id NUMBER,
  order_date DATE,
  customer_id NUMBER
) PARTITION BY RANGE (order_date) INTERVAL (NUMTOYMINTERVAL(1, 'MONTH')) (
  PARTITION p1 VALUES LESS THAN (TO_DATE('2021-01-01', 'YYYY-MM-DD'))
);
```

### Reference Partitioning

Partition tables based on the parent table's partitioning.

```sql
CREATE TABLE departments (
  department_id NUMBER,
  department_name VARCHAR2(100)
) PARTITION BY RANGE (department_id) (
  PARTITION p1 VALUES LESS THAN (100),
  PARTITION p2 VALUES LESS THAN (200)
);

CREATE TABLE employees (
  employee_id NUMBER,
  name VARCHAR2(100),
  department_id NUMBER
) PARTITION BY REFERENCE (department_partition);
```

### Subpartitioning

Subpartition partitions for more granularity.

```sql
CREATE TABLE sales (
  sale_id NUMBER,
  sale_date DATE,
  region_id NUMBER,
  amount NUMBER
) PARTITION BY RANGE (sale_date) SUBPARTITION BY LIST (region_id) (
  PARTITION p1 VALUES LESS THAN (TO_DATE('2021-01-01', 'YYYY-MM-DD')) (
    SUBPARTITION s1 VALUES ('NORTH'),
    SUBPARTITION s2 VALUES ('SOUTH')
  )
);
```

### Global Indexes on Partitioned Tables

Create global indexes for partitioned tables.

```sql
CREATE INDEX idx_global_employee_id ON employees (employee_id) GLOBAL;
```

### Local Indexes on Partitioned Tables

Create local indexes for each partition.

```sql
CREATE INDEX idx_local_employee_name ON employees (name) LOCAL;
```

### Managing Partition Maintenance Operations

Manage partition operations like adding, dropping, or merging partitions.

```sql
ALTER TABLE sales ADD PARTITION p3 VALUES LESS THAN (TO_DATE('2023-01-01', 'YYYY-MM-DD'));
```

## 4. Optimizing PL/SQL Code

### Using BULK COLLECT

Use BULK COLLECT to fetch multiple rows at once.

```sql
DECLARE
  TYPE t_emp IS TABLE OF employees%ROWTYPE;
  l_emps t_emp;
BEGIN
  SELECT * BULK COLLECT INTO l_emps FROM employees;
END;
```

### Using FORALL

Use FORALL for bulk DML operations.

```sql
DECLARE
  TYPE t_emp_ids IS TABLE OF employees.employee_id%TYPE;
  l_emp_ids t_emp_ids := t_emp_ids(1001, 1002, 1003);
BEGIN
  FORALL i IN l_emp_ids.FIRST..l_emp_ids.LAST
    DELETE FROM employees WHERE employee_id = l_emp_ids(i);
END;
```

### Avoiding Slow-by-Slow Processing

Avoid row-by-row processing by using set operations.

```sql
-- Inefficient
FOR emp IN (SELECT * FROM employees) LOOP
  DELETE FROM employees WHERE employee_id = emp.employee_id;
END LOOP;

-- Efficient
DELETE FROM employees WHERE status = 'INACTIVE';
```

### Using Native Compilation

Compile PL/SQL code to native code for better performance.

```sql
ALTER SESSION SET plsql_code_type = 'NATIVE';
```

### Using PL/SQL Function Result Cache

Cache function results to avoid redundant calculations.

```sql
CREATE OR REPLACE FUNCTION get_dept_name (dept_id NUMBER) RETURN VARCHAR2
RESULT_CACHE RELIES_ON (departments)
IS
  v_dept_name VARCHAR2(100);
BEGIN
  SELECT department_name INTO v_dept_name FROM departments WHERE department_id = dept_id;
  RETURN v_dept_name;
END;
```

### Using Collection Methods

Use collection methods to improve PL/SQL performance.

```sql
DECLARE
  TYPE t_numbers IS TABLE OF NUMBER;
  l_numbers t_numbers := t_numbers(1, 2, 3);
BEGIN
  DBMS_OUTPUT.PUT_LINE('Count: ' || l_numbers.COUNT);
END;
```

### Using Pipelined Table Functions

Return rows from a PL/SQL function as a table.

```sql
CREATE OR REPLACE FUNCTION get_numbers (max_num NUMBER) RETURN t_numbers PIPELINED IS
BEGIN
  FOR i IN 1..max_num LOOP
    PIPE ROW (i);
  END LOOP;
  RETURN;
END;
```

### Using the RETURNING Clause

Return values from DML statements to avoid additional queries.

```sql
DECLARE
  v_emp_id employees.employee_id%TYPE;
BEGIN
  INSERT INTO employees (name, department_id, status) VALUES ('John Doe', 10, 'ACTIVE')
  RETURNING employee_id INTO v_emp_id;
END;
```

### Profiling PL/SQL Code

Use DBMS_PROFILER to profile PL/SQL code.

```sql
BEGIN
  DBMS_PROFILER.START_PROFILER('Profile Test');
  -- Your PL/SQL code here
  DBMS_PROFILER.STOP_PROFILER;
END;
```

### Minimizing Context Switches

Reduce

 the number of context switches between SQL and PL/SQL.

```sql
-- Inefficient
FOR emp IN (SELECT * FROM employees) LOOP
  DBMS_OUTPUT.PUT_LINE(emp.name);
END LOOP;

-- Efficient
DECLARE
  TYPE t_names IS TABLE OF employees.name%TYPE;
  l_names t_names;
BEGIN
  SELECT name BULK COLLECT INTO l_names FROM employees;
  FOR i IN l_names.FIRST..l_names.LAST LOOP
    DBMS_OUTPUT.PUT_LINE(l_names(i));
  END LOOP;
END;
```

## 5. Using SQL Plan Management

### SQL Plan Baselines

Capture and use SQL execution plans as baselines.

```sql
DECLARE
  v_sql_handle VARCHAR2(30);
BEGIN
  SELECT sql_handle INTO v_sql_handle FROM dba_sql_plan_baselines WHERE sql_text = 'SELECT * FROM employees';
  DBMS_SPM.LOAD_PLANS_FROM_SQLSET(sqlset_name => 'MY_SQLSET', basic_filter => 'sql_id = ''' || v_sql_handle || '''');
END;
```

### SQL Profiles

Use SQL profiles to provide additional information to the optimizer.

```sql
BEGIN
  DBMS_SQLTUNE.CREATE_SQL_PROFILE(task_name => 'TUNE_SQL', profile_name => 'SQL_PROFILE_EMPLOYEES');
END;
```

### SQL Patches

Apply SQL patches to influence the optimizer.

```sql
BEGIN
  DBMS_SQLDIAG.CREATE_SQL_PATCH(sql_id => 'abcd1234', hint_text => 'INDEX(@SEL$1 EMPLOYEES IDX_EMP_NAME)');
END;
```

### SQL Plan Directives

Create directives to improve execution plans.

```sql
BEGIN
  DBMS_SPM.CREATE_PLAN_DIRECTIVE(plan_name => 'EMPLOYEE_DIRECTIVE', description => 'Directive for Employee queries');
END;
```

### Adaptive Execution Plans

Enable adaptive execution plans for better performance.

```sql
ALTER SYSTEM SET optimizer_adaptive_plans = TRUE;
```

### Managing SQL Plan Evolution

Manage the evolution of SQL execution plans.

```sql
BEGIN
  DBMS_SPM.EVOLVE_SQL_PLAN_BASELINE(sql_handle => 'abcd1234', plan_name => 'new_plan');
END;
```

### Using SQL Tuning Sets

Use SQL Tuning Sets for bulk tuning operations.

```sql
BEGIN
  DBMS_SQLTUNE.CREATE_SQLSET(sqlset_name => 'MY_SQLSET');
END;
```

### Managing SQL Plan Baselines

Manage existing SQL plan baselines.

```sql
BEGIN
  DBMS_SPM.DROP_SQL_PLAN_BASELINE(sql_handle => 'abcd1234');
END;
```

### Using SQL Performance Analyzer

Analyze SQL performance using the SQL Performance Analyzer.

```sql
BEGIN
  DBMS_SQLPA.CREATE_ANALYSIS_TASK(sqlset_name => 'MY_SQLSET', task_name => 'SQL_PA_TASK');
END;
```

### Monitoring SQL Execution Plans

Monitor and compare SQL execution plans.

```sql
SELECT * FROM dba_sql_plan_baselines WHERE sql_handle = 'abcd1234';
```

## 6. Using Hints

### Index Hint

Force the use of a specific index.

```sql
SELECT /*+ INDEX(employees emp_idx) */ name FROM employees WHERE employee_id = 100;
```

### Parallel Hint

Enable parallel execution for a query.

```sql
SELECT /*+ PARALLEL(employees, 4) */ name FROM employees;
```

### No_Parallel Hint

Disable parallel execution for a query.

```sql
SELECT /*+ NO_PARALLEL(employees) */ name FROM employees;
```

### Full Hint

Force a full table scan.

```sql
SELECT /*+ FULL(employees) */ name FROM employees;
```

### Merge Hint

Force a merge join.

```sql
SELECT /*+ MERGE(employees departments) */ e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Use_NL Hint

Force a nested loop join.

```sql
SELECT /*+ USE_NL(employees departments) */ e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Use_Hash Hint

Force a hash join.

```sql
SELECT /*+ USE_HASH(employees departments) */ e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Use_Merge Hint

Force a merge join.

```sql
SELECT /*+ USE_MERGE(employees departments) */ e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Leading Hint

Specify the leading table in a join.

```sql
SELECT /*+ LEADING(employees) */ e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

### Ordered Hint

Force the optimizer to join tables in the order listed.

```sql
SELECT /*+ ORDERED */ e.name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
```

## 7. Monitoring and Analyzing Performance

### Using AWR Reports

Generate AWR reports to analyze performance.

```sql
BEGIN
  DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT();
  DBMS_WORKLOAD_REPOSITORY.AWR_REPORT_TEXT(
    l_dbid         => <dbid>,
    l_inst_num     => <inst_num>,
    l_bid          => <begin_snapshot_id>,
    l_eid          => <end_snapshot_id>
  );
END;
```

### Using SQL Trace and TKPROF

Use SQL Trace and TKPROF for detailed execution analysis.

```sql
ALTER SESSION SET sql_trace = TRUE;
-- Run your SQL statements
ALTER SESSION SET sql_trace = FALSE;
-- Use TKPROF to format the trace file
TKPROF tracefile.trc outputfile.prf;
```

### Using V$ Views

Use V$ views to monitor real-time performance.

```sql
SELECT * FROM v$session WHERE username = 'HR';
```

### Using Enterprise Manager

Use Oracle Enterprise Manager for comprehensive performance monitoring.

### Using ASH Reports

Generate ASH reports for detailed session activity.

```sql
SELECT * FROM v$active_session_history WHERE sample_time > SYSDATE - 1/24;
```

### Using ADDM Reports

Generate ADDM reports for automatic database diagnostics.

```sql
BEGIN
  DBMS_ADVISOR.ADDM_ANALYSIS();
END;
```

### Monitoring Wait Events

Monitor wait events to identify performance bottlenecks.

```sql
SELECT event, wait_class, total_waits FROM v$system_event ORDER BY total_waits DESC;
```

### Using Statspack

Use Statspack for performance snapshots and analysis.

```sql
BEGIN
  STATSPACK.SNAP;
END;
```

### Monitoring Session Activity

Monitor session activity to identify problematic sessions.

```sql
SELECT sid, serial#, username, program, status FROM v$session WHERE status = 'ACTIVE';
```

### Using Automatic Database Diagnostics Monitor

Use ADDM to analyze and provide recommendations for performance issues.

```sql
EXEC DBMS_ADDM.ANALYZE_INST(DB_ID, INSTANCE_ID, START_SNAPSHOT, END_SNAPSHOT);
```

## 8. Optimizing Indexes

### Rebuilding Indexes

Rebuild indexes to improve performance.

```sql
ALTER INDEX idx_employee_name REBUILD;
```

### Using Bitmap Indexes

Use bitmap indexes for columns with low cardinality.

```sql
CREATE BITMAP INDEX idx_employee_status ON employees (status);
```

### Using Function-Based Indexes

Create indexes based on functions.

```sql
CREATE INDEX idx_upper_name ON employees (UPPER(name));
```

### Dropping Unused Indexes

Drop indexes that are no longer used.

```sql
DROP INDEX idx_old_index;
```

### Indexing Foreign Keys

Index foreign key columns for better join performance.

```sql
CREATE INDEX idx_department_id ON employees (department_id);
```

### Using Reverse Key Indexes

Use reverse key indexes to avoid contention on indexed columns.

```sql
CREATE INDEX idx_reverse_employee_id ON employees (employee_id) REVERSE;
```

### Using Compressed Indexes

Use compressed indexes to save space.

```sql
CREATE INDEX idx_compressed_name ON employees (name) COMPRESS 2;
```

### Monitoring Index Usage

Monitor index usage to identify unused indexes.

```sql
SELECT * FROM v$object_usage WHERE index_name = 'IDX_EMPLOYEE_NAME';
```

### Creating Invisible Indexes

Create invisible indexes for testing without affecting query performance.

```sql
CREATE INDEX idx_invisible_name ON employees (name) INVISIBLE;
```

### Using Global and Local Indexes

Choose between global and local indexes based on your partitioning strategy.

```sql
CREATE INDEX idx_global_name ON employees (name) GLOBAL;

CREATE INDEX idx_local_name ON employees (name) LOCAL;
```

## 9. Managing Memory

### Configuring the SGA and PGA

Configure the SGA and PGA for optimal performance.

```sql
ALTER SYSTEM SET sga_target = 2G SCOPE=BOTH;
ALTER SYSTEM SET pga_aggregate_target = 1G SCOPE=BOTH;
```

### Using Automatic Memory Management

Enable automatic memory management for simplified memory tuning.

```sql
ALTER SYSTEM SET memory_target = 4G SCOPE=BOTH;
```

### Optimizing the Buffer Cache

Configure the buffer cache for optimal performance.

```sql
ALTER SYSTEM SET db_cache_size = 1G SCOPE=BOTH;
```

### Optimizing the Shared Pool

Configure the shared pool for efficient memory usage.

```sql
ALTER SYSTEM SET shared_pool_size = 500M SCOPE=BOTH;
```

### Optimizing the Large Pool

Configure the large pool for better performance of large memory allocations.

```sql
ALTER SYSTEM SET large_pool_size

 = 200M SCOPE=BOTH;
```

### Optimizing the Java Pool

Configure the Java pool for optimal performance of Java applications.

```sql
ALTER SYSTEM SET java_pool_size = 100M SCOPE=BOTH;
```

### Optimizing the Streams Pool

Configure the Streams pool for efficient Streams replication.

```sql
ALTER SYSTEM SET streams_pool_size = 100M SCOPE=BOTH;
```

### Using the Result Cache

Enable and configure the result cache for query performance improvement.

```sql
ALTER SYSTEM SET result_cache_max_size = 100M SCOPE=BOTH;
```

### Monitoring Memory Usage

Monitor memory usage to identify and resolve memory-related performance issues.

```sql
SELECT * FROM v$memory_target_advice;
```

### Using Memory Advisors

Use memory advisors to get recommendations for memory settings.

```sql
SELECT * FROM v$sga_target_advice;
```

## 10. Optimizing Storage

### Using ASM for Storage Management

Use ASM for efficient storage management.

```sql
CREATE DISKGROUP data_disk_group EXTERNAL REDUNDANCY DISK 'ORCL:DISK1', 'ORCL:DISK2';
```

### Using Partition Pruning

Ensure partition pruning is used to limit the amount of data scanned.

```sql
SELECT * FROM sales WHERE sale_date >= TO_DATE('2023-01-01', 'YYYY-MM-DD');
```

### Using Index-Organized Tables

Use index-organized tables for improved read performance.

```sql
CREATE TABLE employees (
  employee_id NUMBER,
  name VARCHAR2(100),
  department_id NUMBER,
  status VARCHAR2(10),
  PRIMARY KEY (employee_id)
) ORGANIZATION INDEX;
```

### Using External Tables

Use external tables for accessing data stored outside the database.

```sql
CREATE TABLE external_employees (
  employee_id NUMBER,
  name VARCHAR2(100),
  department_id NUMBER,
  status VARCHAR2(10)
)
ORGANIZATION EXTERNAL (
  TYPE ORACLE_LOADER
  DEFAULT DIRECTORY ext_data_dir
  ACCESS PARAMETERS (
    RECORDS DELIMITED BY NEWLINE
    FIELDS TERMINATED BY ','
    MISSING FIELD VALUES ARE NULL
    (
      employee_id INTEGER EXTERNAL,
      name CHAR(100),
      department_id INTEGER EXTERNAL,
      status CHAR(10)
    )
  )
  LOCATION ('employees.csv')
);
```

### Using Compression

Use compression to save storage space and improve performance.

```sql
ALTER TABLE employees COMPRESS FOR OLTP;
```

### Using Direct-Path Inserts

Use direct-path inserts for faster data loading.

```sql
INSERT /*+ APPEND */ INTO employees SELECT * FROM temp_employees;
```

### Using In-Memory Column Store

Use in-memory column store for faster query performance.

```sql
ALTER TABLE employees INMEMORY;
```

### Using Read-Only Tablespaces

Use read-only tablespaces for static data to reduce backup and recovery times.

```sql
ALTER TABLESPACE archive READ ONLY;
```

### Monitoring Storage Usage

Monitor storage usage to ensure optimal performance.

```sql
SELECT tablespace_name, used_space, free_space FROM dba_tablespace_usage_metrics;
```

### Using Flashback Technology

Use flashback technology for data recovery and analysis.

```sql
-- Enable flashback
ALTER DATABASE FLASHBACK ON;

-- Flashback a table
FLASHBACK TABLE employees TO SCN 123456;
```

## 11. Using Exadata

### Using Smart Scan

Leverage Exadata's Smart Scan for faster data retrieval.

```sql
SELECT /*+ CELL_SMART_SCAN */ name FROM employees WHERE department_id = 10;
```

### Using Storage Indexes

Utilize storage indexes to reduce I/O.

```sql
SELECT * FROM sales WHERE sale_date >= TO_DATE('2023-01-01', 'YYYY-MM-DD');
```

### Using Hybrid Columnar Compression

Use hybrid columnar compression for better storage efficiency.

```sql
ALTER TABLE employees COMPRESS FOR QUERY HIGH;
```

### Offloading Processing

Offload processing to storage cells to reduce database CPU usage.

```sql
SELECT /*+ CELL_OFFLOAD_PROCESSING */ name FROM employees;
```

### Using IORM

Use I/O Resource Manager to prioritize I/O resources.

```sql
ALTER IORMPLAN PLAN_NAME = 'MyPlan';
```

### Monitoring Exadata Performance

Monitor Exadata performance using Exadata-specific views and tools.

```sql
SELECT * FROM v$cell;
```

### Using Smart Flash Cache

Utilize Exadata's Smart Flash Cache for faster data access.

```sql
ALTER SYSTEM SET db_flash_cache_size = 100G;
```

### Using Exadata AWR Reports

Generate Exadata-specific AWR reports for performance analysis.

```sql
@?/rdbms/admin/awrexrpt.sql
```

### Using Exadata Compression Advisor

Use the Exadata Compression Advisor to determine optimal compression settings.

```sql
DECLARE
  result OUT VARCHAR2(32767);
BEGIN
  DBMS_COMPRESSION.GET_COMPRESSION_RATIO('HR', 'EMPLOYEES', NULL, result);
  DBMS_OUTPUT.PUT_LINE(result);
END;
```

### Using In-Memory on Exadata

Combine Exadata with in-memory options for even greater performance.

```sql
ALTER TABLE employees INMEMORY PRIORITY CRITICAL;
```
