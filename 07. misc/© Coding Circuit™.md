# **SQL PLSQL Developer Course Outline (Oracle)**

<br>  
<br>  


## **<span style="color:coral;"> 1. Basic SQL</span>**  &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;   &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp;&nbsp;  &nbsp;  &nbsp;  &nbsp; **<span style="color:coral;"> 16 classes</span>**

1. **Oracle 11g Database Configuration**  – **<span style="color:coral;"> 1 class</span>**
   - Installing Oracle 11g  
   - Configuring Oracle Users and Roles  
   - Configuring the Sample Database  

2. **RDBMS and SQL** –  **<span style="color:coral;"> 1 class</span>**
   - Database Management System (DBMS)  
   - Relational Database Management System (RDBMS)  
   - Structured Query Language (SQL)  

3. **Data Types and Constraints** – **<span style="color:coral;"> 2 classes</span>**
   - Oracle Data Types (CHAR, VARCHAR2, NUMBER, DATE, BLOB)  
   - Implicit and Explicit Data Type Conversion  
   - Types of Constraints (NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK)  
   - Defining and Modifying Constraints  

4. **DDL (Data Definition Language)**  – **<span style="color:coral;"> 1 class</span>**
   - CREATE  
   - ALTER  
   - DROP  
   - RENAME  
   - TRUNCATE  

5. **DML (Data Manipulation Language)**  – **<span style="color:coral;"> 1 class</span>**
   - INSERT
   - UPDATE
   - DELETE  

6. **TCL (Transaction Control Language)**  – **<span style="color:coral;"> 1 class</span>**
   - COMMIT
   - ROLLBAC
   - SAVEPOINT  

7. **DRL (Data Retrieval Language) - SELECT**  – **<span style="color:coral;"> 2 classes</span>**
   - Basic SELECT Query
   - Using WHERE Clause
   - Sorting Results with ORDER BY
   - Filtering Data with DISTINCT
   - Limiting Results with ROWNUM and FETCH

8. **Single Row Functions**  – **<span style="color:coral;"> 4 classes</span>**
   - String Functions (e.g., CONCAT, LENGTH, SUBSTR, INSTR)
   - Numeric Functions (e.g., ROUND, CEIL, FLOOR, MOD)
   - Date Functions (e.g., SYSDATE, ADD_MONTHS, TO_DATE)
   - Conversion Functions (e.g., TO_CHAR, TO_NUMBER, TO_DATE)
   - NULL-related Functions (e.g., NVL, COALESCE, NULLIF) 

9. **Aggregate Functions**  – **<span style="color:coral;"> 1 class</span>**
   - COUNT
   - SUM
   - AVG
   - MAX
   - MIN
   - Using GROUP BY and HAVING with Aggregate Functions  

10. **Joins**  – **<span style="color:coral;"> 2 classes</span>**
    - INNER JOIN  
    - OUTER JOIN (LEFT, RIGHT, FULL)  
    - CROSS JOIN  
    - SELF JOIN 

<br>   



## **<span style="color:coral;"> 2. Advanced SQL</span>** &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;    &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp;&nbsp;  &nbsp;  &nbsp;  &nbsp; **<span style="color:coral;"> 14 classes</span>**

1. **Pseudo Column** – **<span style="color:coral;"> 1 class</span>**
    - ROWNUM  
    - ROWID  
    - SYSDATE  
    - LEVEL  
    - CURRVAL and NEXTVAL (for Sequences)  
      
2. **Advanced DML**  – **<span style="color:coral;"> 2 classes</span>**
   - INSERT ALL
   - UPDATE ALL
   - MERGE (Upsert Operation)
   - UPDATE with JOIN
   - DELETE with JOIN
  

3. **Subqueries**  – **<span style="color:coral;"> 3 classes</span>**
    - Single-row Subquery  
    - Multiple-row Subquery  
    - Correlated Subquery  
    - Nested Subquery  
    - Subquery in SELECT, WHERE, and FROM clauses  
 
4. **SET Operators**  – **<span style="color:coral;"> 1 class</span>**
    - UNION  
    - UNION ALL  
    - INTERSECT  
    - MINUS  
    - Differences between UNION and UNION ALL  

5. **Analytical (Window) Functions**  – **<span style="color:coral;"> 2 classes</span>**
    - ROW_NUMBER()  
    - RANK()  
    - DENSE_RANK()  
    - NTILE()  
    - LEAD() and LAG()  
    - FIRST_VALUE() and LAST_VALUE()  
    - PERCENT_RANK()  

6. **Conditional Statements**  – **<span style="color:coral;"> 1 class</span>**
    - CASE Statement  
    - DECODE Function  
    - NVL (as a conditional function)  

7. **Views**  – **<span style="color:coral;"> 2 class</span>**
    - Creating Views  
    - Updating Views  
    - Dropping Views  
    - Materialized Views  
    - Advantages and Limitations of Views  

 
8. **Sequences**  – **<span style="color:coral;"> 1 class</span>**
    - Creating Sequences  
    - Using Sequences with NEXTVAL and CURRVAL  
    - Alter and Drop Sequences
 
9. **Synonyms**  – **<span style="color:coral;"> 1 class</span>**
    - Creating Synonyms  
    - Using Public and Private Synonyms  
    - Dropping Synonyms  
    - Advantages of Synonyms in SQL Queries  


<br>  

## **<span style="color:coral;"> 3. PLSQL</span>** &nbsp;  &nbsp;  &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp;&nbsp;  &nbsp;  &nbsp;  &nbsp; **<span style="color:coral;"> 34 classes</span>**

1. **Getting Started with PL/SQL**  – **<span style="color:coral;"> 1 class</span>**
   - Introduction to PL/SQL  
   - PL/SQL Block Structure (DECLARE, BEGIN, EXCEPTION, END)  
   - PL/SQL vs SQL  
   - Writing and Executing Simple PL/SQL Programs  

2. **Flow Control (Conditional Statements)**  – **<span style="color:coral;"> 1 class</span>**
   - IF-THEN-ELSE  
   - CASE  
   - GOTO
   - NULL  

3. **Flow Control (Iterative Statements)**  – **<span style="color:coral;"> 1 class</span>**
   - LOOP  
   - WHILE LOOP  
   - FOR LOOP  
   - EXIT and CONTINUE Statements  

4. **SELECT INTO**  – **<span style="color:coral;"> 1 class</span>**
     - Using SELECT INTO for Variable Assignment  
     - Selecting into Record Variables  
     - Handling Multiple Rows with SELECT INTO  
     - Common Errors with SELECT INTO  


5. **Exceptions**  – **<span style="color:coral;"> 2 classes</span>**
   - Predefined Exceptions  
   - User-defined Exceptions  
   - EXCEPTION_INIT  
   - Handling Exceptions with RAISE and RAISE_APPLICATION_ERROR  
   - Exception Propagation  

6. **Cursors**  – **<span style="color:coral;"> 2 classes</span>**
   - Implicit Cursors  
   - Explicit Cursors  
   - Cursor FOR Loops  
   - Cursor Variables  
   - Handling Cursor Exceptions  

7. **Records**  – **<span style="color:coral;"> 2 classes</span>**
   - Declaring a Record Type  
   - Using Records with SELECT INTO  
   - %ROWTYPE vs %TYPE  
   - Manipulating Record Values  

8. **Procedures**  – **<span style="color:coral;"> 3 classes</span>**
   - Creating and Calling Procedures  
   - IN, OUT, and INOUT Parameters  
   - Procedure Overloading  
   - Handling Exceptions in Procedures  
   - Dropping and Altering Procedures  

9. **Functions**  – **<span style="color:coral;"> 2 classes</span>**
   - Creating and Calling Functions  
   - IN and OUT Parameters in Functions  
   - Function Overloading  
   - Returning Values from Functions  
   - Dropping and Altering Functions
   - Procuedure vs Functions

10. **Packages**  – **<span style="color:coral;"> 3 classes</span>**
    - Creating and Using Packages  
    - Package Specification vs Package Body  
    - Public and Private Procedures/Functions  
    - Package Initialization  
    - Package State (Global Variables)  
    - Advantages of Packages over Procedures and Functions  

11. **Triggers**  – **<span style="color:coral;"> 2 classes</span>**
    - Creating Triggers  
    - Types of Triggers (BEFORE, AFTER, INSTEAD OF)  
    - INSTEAD OF Triggers  
    - Row-Level vs Statement-Level Triggers  
    - Compound Triggers  
    - Trigger Timing and Order  
    - Disabling and Dropping Triggers  

12. **Collections**  – **<span style="color:coral;"> 3 classes</span>**
    - Types of Collections (Associative Arrays, Nested Tables, VARRAYs)  
    - Creating and Using Collections  
    - Manipulating Collection Data  
    - Collection Methods (COUNT, EXISTS, FIRST, LAST)  

13. **Bulk Collect**  – **<span style="color:coral;"> 2 classes</span>**
    - Using BULK COLLECT to Fetch Data into Collections  
    - LIMIT Clause in BULK COLLECT  
    - Performance Benefits of BULK COLLECT  
    - Handling Exceptions with BULK COLLECT  
    - Fetching Multiple Collections in One Query  
    - Using FORALL with BULK COLLECT for Efficient DML Operations  

14. **Dynamic SQL**  – **<span style="color:coral;"> 2 classes</span>**
    - Introduction to Dynamic SQL  
    - EXECUTE IMMEDIATE  
    - Using Bind Variables with Dynamic SQL  
    - Building Dynamic SQL for DML and DDL  
    - Executing Dynamic SQL with RETURNING INTO  

15. **Table Functions**  – **<span style="color:coral;"> 2 classes</span>**
    - Creating and Using Table Functions  
    - Returning a Table from a Function  
    - Using Table Functions in SQL Queries
   
16. **Objects** – **<span style="color:coral;"> 2 classes</span>**
      - Creating and Using Object Types  
      - Methods in Object Types  
      - Using Objects in Tables (Object-Relational Features)  
      - Managing Object Type Dependencies
   
17. **Database Links** – **<span style="color:coral;"> 1 class</span>**
      - Creating Database Links  
      - Using Database Links in Queries  
      - Managing Database Links   
      - Performance Considerations  

   
18. **Autonomous Transactions** – **<span style="color:coral;"> 2 classes</span>**
      - Using PRAGMA AUTONOMOUS_TRANSACTION  
      - When to Use Autonomous Transactions  
      - COMMIT and ROLLBACK Inside Autonomous Transactions  
      - Performance Considerations 

<br>  


## **<span style="color:coral;"> 4. Performance Tuning</span>** &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;    &nbsp; &nbsp;   &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  &nbsp;&nbsp;  &nbsp;   **<span style="color:coral;"> 16 classes</span>**

1. **Read an Execution Plan** – **<span style="color:coral;"> 2 class</span>**
   - Understanding the Execution Plan  
   - How to Generate an Execution Plan  
   - Interpreting Execution Plan Outputs  
   - Using EXPLAIN PLAN 
 
2. **Database Statistics** – **<span style="color:coral;"> 2 class</span>**
   - Collecting Database Statistics  
   - Viewing and Interpreting Statistics  
   - Using DBMS_STATS  
   - Automatic vs Manual Statistics Collection  
   - Analyzing and Tuning Based on Statistics 

3. **Query Performance** – **<span style="color:coral;"> 2 class</span>**
   - Identifying Slow Queries  
   - Optimizing Query Execution  
   - Using Hints for Query Optimization  
   - Common Query Performance Pitfalls  
   - Analyzing Query Performance with EXPLAIN PLAN
 
4. **Create Indexes** – **<span style="color:coral;"> 1 class</span>**
   - Types of Indexes (B-tree, Bitmap, Clustered, etc.)  
   - Creating Indexes in Oracle  
   - Using Indexes to Improve Query Performance  
   - Index Maintenance (Rebuilding, Dropping)  
   - When Not to Use Indexes 

5. **Why My Query Not Using an Index** – **<span style="color:coral;"> 1 class</span>**
   - Reasons Queries Don’t Use Indexes  
   - Optimizer Hints to Force Index Usage  
   - Index Selectivity and Cardinality  
   - Using EXPLAIN PLAN to Diagnose Index Usage  
   - Resolving Common Indexing Issues 

6. **Summarize Data Fast with Materialized Views** – **<span style="color:coral;"> 2 classes</span>**
   - Creating Materialized Views  
   - Refreshing Materialized Views (On-demand, Periodically)  
   - Materialized View Logs  
   - Performance Benefits of Materialized Views  
   - When to Use Materialized Views 

7. **Joins**  – **<span style="color:coral;"> 1 class</span>**
   - Hash Joins  
   - Nested Loops Joins  
   - Merge Joins  
   - Join Algorithms and Their Performance  
   - Choosing the Right Join for Performance
  

8. **Make Inserts, Updates, and Deletes Faster** – **<span style="color:coral;"> 2 classes</span>**
      - Optimizing INSERT Performance  
      - Batch Processing for Bulk Inserts  
      - Optimizing UPDATE and DELETE Queries  
      - Using Direct Path Inserts  
      - Avoiding Locking Issues during DML Operations 

9. **Find Slow SQL** – **<span style="color:coral;"> 2 classes</span>**
      - Identifying Slow SQL Queries  
      - Using AWR and ASH Reports for Performance Tuning  
      - SQL Trace and TKPROF  
      - Using Oracle Enterprise Manager (OEM) to Find Slow SQL  
      - Optimizing Slow Queries 
 
10. **Further Reading** – **<span style="color:coral;"> 1 class</span>**
      - Books and Resources for SQL and PL/SQL Tuning  
      - Oracle Documentation for Advanced Features  
      - Best Practices for SQL Performance  
      - Websites and Communities for Oracle Performance Tuning



<br>  
<br>  
<br>  



## **<span style="color:blue;">📌 Course Fee</span>**


<br>   



| Course Name | Topics Covered | Fee |
|-------------|----------------|------|
| SQL only | Basic SQL, Advanced SQL - **`30 classes`**  | **₹20,000** |
| SQL + PLSQL | Basic SQL, Advanced SQL, PL/SQL - **`64 classes`** | **₹35,000** |
| SQL + PLSQL + Perf Tuning | Basic SQL, Advanced SQL, PL/SQL, Performance Tuning  - **`80 classes`** | **₹40,000** |
| | | |



<br>  
<br>  
<br>  



## **<span style="color:blue;">✅ Included</span>**

- One-to-One Sessions 
- Live Session Recording 
- Project-Specific Assignments 
- GitHub Training Material Access 
