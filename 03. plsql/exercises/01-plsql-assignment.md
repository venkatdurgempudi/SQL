## PL/SQL Assignments on HR Schema


### **1. Employee Bonus Calculation** *(Procedures, Exceptions, Cursors)*  
- Create a procedure that calculates a **bonus** for each employee based on salary and job title.  
- Use an **explicit cursor** to iterate over employees.  
- If salary > 10,000, bonus = 10%; otherwise, bonus = 15%.  
- Handle exceptions where **salary is NULL** and log errors into an error table.  

---

### **2. Department Salary Budget** *(Functions, Records, Loops, Exception Handling)*  
- Develop a function that **accepts a department ID** and returns the **total salary budget**.  
- Use `%ROWTYPE` to fetch employee details.  
- Loop through employee records using a **FOR LOOP** and sum their salaries.  
- If department ID does not exist, raise a **custom exception**.  

---

### **3. Employee Promotion** *(Triggers, Cursors, Dynamic SQL, Bulk Collect)*  
- Implement a **trigger** that updates an employee’s **job title and salary** when their performance rating is updated in a `performance_reviews` table.  
- Use **explicit cursors** to fetch employee details.  
- Apply **bulk collect** to handle multiple employee records efficiently.  
- Ensure salary updates follow the company's **dynamic business rules using Dynamic SQL**.  

---

### **4. Employee Hiring System** *(Procedures, Dynamic SQL, Autonomous Transactions)*  
- Create a **procedure** to insert new employee records.  
- Use **Dynamic SQL** to insert into different tables based on **department type** (e.g., IT, HR, Sales).  
- Implement an **autonomous transaction** to commit data independently, even if called inside another transaction.  

---

### **5. Employee Hierarchy Report** *(Recursion using PL/SQL, Cursors, Table Functions)*  
- Develop a **table function** that **returns the hierarchy** of an employee up to the CEO level.  
- Use **recursive logic** to traverse the **manager-subordinate** hierarchy.  
- Utilize **cursors** to fetch employee-manager relationships.  

---

### **6. Department Reorganization** *(Collections, Bulk Collect, Update Statements, Exception Handling)*  
- Move all employees from **one department to another**.  
- Store their **previous department** details in a **history table**.  
- Use **collections** and **bulk collect** for faster processing.  
- Implement **exception handling** for scenarios where a department does not exist.  

---

### **7. Employee Performance History** *(Triggers, Collections, Procedures, Exception Handling)*  
- Implement a **trigger** that logs **salary updates** into a `salary_history` table.  
- Store old and new values in a **collection** before inserting into the log.  
- Handle exceptions when an employee has no salary record.  

---

### **8. Dynamic Employee Reports** *(Dynamic SQL, Records, Cursors, CASE Statement)*  
- Write a **procedure** that generates a **report** based on user inputs:  
  - **Department**  
  - **Job title**  
  - **Hire date range**  
- Use **Dynamic SQL** to build queries dynamically.  
- Store fetched data into **records** and display output.  

---

### **9. Top Performers by Department** *(Bulk Collect, Table Functions, Cursors, Loops)*  
- Develop a **table function** that returns the **top 3 highest-paid employees** in each department.  
- Use **bulk collect** and **cursor loops** to process multiple records efficiently.  

---

### **10. Employee Transfer Log** *(Autonomous Transactions, Exception Handling, Triggers)*  
- Implement a **trigger** that logs department **transfers** into a `transfer_log` table.  
- Use **autonomous transactions** to ensure logs are committed **independently** of the main transaction.  
- Handle **exceptions** where an employee is transferred to a non-existent department.  

---

### **11. Salary Adjustment by Job Type** *(Procedures, Dynamic SQL, Cursors, Records, Exception Handling)*  
- Create a **procedure** to adjust salaries based on **job title**:  
  - **Technical jobs:** Increase by 5%  
  - **Managerial jobs:** Increase by 10%  
- Use **explicit cursors** to fetch employee details.  
- Apply **Dynamic SQL** for flexible query execution.  

---

### **12. Employee Leave Management** *(Collections, Procedures, Exception Handling, Bulk Collect)*  
- Implement a **procedure** to process employee **leave requests**.  
- Store leave requests in a **collection**.  
- Validate **leave balance** before approving.  
- Use **bulk collect** for performance optimization.  
- Handle **exceptions** where employees exceed leave limits.  

---

### **13. Departmental Headcount Analysis** *(Functions, Collections, Bulk Collect, Loops)*  
- Write a **function** that returns **department-wise headcount**.  
- Use **collections** and **bulk collect** to store employee data efficiently.  
- Display only departments where employee count **> 10**.  

---

### **14. Employee Resignation Process** *(Procedures, Cursors, Dynamic SQL, Exception Handling)*  
- Write a **procedure** to process **employee resignations**.  
- The procedure should:  
  - Update employee status  
  - Remove login credentials  
  - Transfer responsibilities  
- Use **Dynamic SQL** and **cursors** to update records.  

---

### **15. Dynamic Table Creation for Employee Logs** *(Dynamic SQL, Procedures, Triggers, Autonomous Transactions)*  
- Create a **procedure** that dynamically creates **log tables** based on **current year** (e.g., `employee_log_2025`).  
- Implement a **trigger** to insert employee updates into the correct table.  

---

### **16. Employee Appraisal System** *(Functions, Cursors, CASE Statements, Bulk Collect)*  
- Develop a **function** to evaluate **employee appraisals** based on:  
  - **Performance scores**  
  - **Current salary**  
- Use **CASE statements** to determine the salary increase.  
- Fetch records using **bulk collect**.  

---

### **17. Payroll System with Error Handling** *(Procedures, Cursors, Exception Handling, Bulk Collect)*  
- Implement a **payroll system** using a **procedure** that calculates **net salary** after deductions.  
- Use **bulk collect** to process multiple employees.  
- If an employee has **missing salary details**, log errors separately and continue processing.  

---

### **18. Department Budget Forecasting** *(Table Functions, Bulk Collect, Aggregations, Cursors)*  
- Create a **table function** that forecasts **department budgets** for the next **5 years**.  
- Use **bulk collect** and **cursors** to fetch and process salary trends.  

---

### **19. Employee Role Change Process** *(Procedures, Triggers, Dynamic SQL, Exception Handling, Autonomous Transactions)*  
- Write a **procedure** to process employee **role changes**:  
  - Update **job title**  
  - Modify **salary** based on role  
  - Trigger a **notification system**  
- Use **Dynamic SQL** and **autonomous transactions** for data integrity.  

---

### **20. Cross-Database Employee Audit** *(Database Links, Cursors, Procedures, Exception Handling)*  
- Implement a **procedure** that fetches **employee details** from a **remote HR database** via a **database link**.  
- Insert fetched records into a **local audit table**.  
- Handle connection **errors** and **missing data** scenarios.  

---



<!--

-- 1. Employee Bonus Calculation (Procedures, Exceptions, Cursors)
CREATE OR REPLACE PROCEDURE calculate_bonus IS
    CURSOR emp_cursor IS SELECT employee_id, salary FROM employees;
    emp_id employees.employee_id%TYPE;
    emp_salary employees.salary%TYPE;
    emp_bonus NUMBER;
BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO emp_id, emp_salary;
        EXIT WHEN emp_cursor%NOTFOUND;
        
        IF emp_salary IS NULL THEN
            INSERT INTO error_log VALUES (emp_id, 'Salary is NULL');
        ELSE
            emp_bonus := CASE WHEN emp_salary > 10000 THEN emp_salary * 0.10 ELSE emp_salary * 0.15 END;
            INSERT INTO bonuses(employee_id, bonus_amount) VALUES (emp_id, emp_bonus);
        END IF;
    END LOOP;
    CLOSE emp_cursor;
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/

-- 2. Department Salary Budget (Functions, Records, Loops, Exception Handling)
CREATE OR REPLACE FUNCTION department_salary_budget(p_dept_id NUMBER) RETURN NUMBER IS
    v_total_salary NUMBER := 0;
    v_employee employees%ROWTYPE;
    CURSOR dept_cursor IS SELECT * FROM employees WHERE department_id = p_dept_id;
BEGIN
    OPEN dept_cursor;
    LOOP
        FETCH dept_cursor INTO v_employee;
        EXIT WHEN dept_cursor%NOTFOUND;
        v_total_salary := v_total_salary + v_employee.salary;
    END LOOP;
    CLOSE dept_cursor;
    RETURN v_total_salary;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN -1;
    WHEN OTHERS THEN
        RETURN -2;
END;
/

-- 3. Employee Promotion (Triggers, Cursors, Dynamic SQL, Bulk Collect)
CREATE OR REPLACE TRIGGER employee_promotion_trigger
AFTER UPDATE OF performance_rating ON performance_reviews
FOR EACH ROW
DECLARE
    CURSOR emp_cur IS SELECT employee_id, job_id, salary FROM employees WHERE employee_id = :NEW.employee_id;
    emp_rec employees%ROWTYPE;
    new_salary NUMBER;
BEGIN
    OPEN emp_cur;
    FETCH emp_cur INTO emp_rec;
    CLOSE emp_cur;
    
    IF :NEW.performance_rating >= 5 THEN
        new_salary := emp_rec.salary * 1.10;
        UPDATE employees SET salary = new_salary WHERE employee_id = :NEW.employee_id;
    END IF;
END;
/

-- 4. Employee Hiring System (Procedures, Dynamic SQL, Autonomous Transactions)
CREATE OR REPLACE PROCEDURE hire_employee(
    p_emp_name VARCHAR2, p_job_id VARCHAR2, p_salary NUMBER, p_dept_id NUMBER
) IS
    PRAGMA AUTONOMOUS_TRANSACTION;
    v_sql VARCHAR2(500);
BEGIN
    v_sql := 'INSERT INTO employees(employee_name, job_id, salary, department_id) VALUES (' ||
             '''' || p_emp_name || ''', ''' || p_job_id || ''', ' || p_salary || ', ' || p_dept_id || ')';
    EXECUTE IMMEDIATE v_sql;
    COMMIT;
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/

-- 5. Employee Hierarchy Report (Recursion using PL/SQL, Cursors, Table Functions)
CREATE OR REPLACE FUNCTION get_employee_hierarchy(p_emp_id NUMBER) RETURN SYS_REFCURSOR IS
    v_cursor SYS_REFCURSOR;
BEGIN
    OPEN v_cursor FOR
        WITH hierarchy AS (
            SELECT employee_id, manager_id FROM employees WHERE employee_id = p_emp_id
            UNION ALL
            SELECT e.employee_id, e.manager_id FROM employees e JOIN hierarchy h ON e.manager_id = h.employee_id
        )
        SELECT * FROM hierarchy;
    RETURN v_cursor;
END;
/

-- 6. Department Reorganization (Packages, Procedures, Dynamic SQL)
CREATE OR REPLACE PACKAGE department_reorg_pkg AS
    PROCEDURE transfer_employee(p_emp_id NUMBER, p_new_dept_id NUMBER);
END department_reorg_pkg;
/

CREATE OR REPLACE PACKAGE BODY department_reorg_pkg AS
    PROCEDURE transfer_employee(p_emp_id NUMBER, p_new_dept_id NUMBER) IS
    BEGIN
        UPDATE employees SET department_id = p_new_dept_id WHERE employee_id = p_emp_id;
        COMMIT;
    EXCEPTION
        WHEN OTHERS THEN
            ROLLBACK;
            DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
    END;
END department_reorg_pkg;
/

-- 7. Employee Performance History (Collections, Bulk Collect, Cursors)
CREATE OR REPLACE PROCEDURE get_performance_history(p_emp_id NUMBER) IS
    TYPE perf_table IS TABLE OF performance_reviews%ROWTYPE;
    v_perf_data perf_table;
BEGIN
    SELECT * BULK COLLECT INTO v_perf_data FROM performance_reviews WHERE employee_id = p_emp_id;
    FOR i IN v_perf_data.FIRST..v_perf_data.LAST LOOP
        DBMS_OUTPUT.PUT_LINE('Review Date: ' || v_perf_data(i).review_date || ' Rating: ' || v_perf_data(i).performance_rating);
    END LOOP;
END;
/

-- 8. Dynamic Employee Reports (Dynamic SQL, Table Functions)
CREATE OR REPLACE FUNCTION dynamic_employee_report(p_column_name VARCHAR2) RETURN SYS_REFCURSOR IS
    v_cursor SYS_REFCURSOR;
    v_query VARCHAR2(200);
BEGIN
    v_query := 'SELECT ' || p_column_name || ' FROM employees';
    OPEN v_cursor FOR v_query;
    RETURN v_cursor;
END;
/

-- 9. Employee Salary Audit (Triggers, Exception Handling)
CREATE OR REPLACE TRIGGER salary_audit_trigger
BEFORE UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
    INSERT INTO salary_audit(employee_id, old_salary, new_salary, change_date)
    VALUES (:OLD.employee_id, :OLD.salary, :NEW.salary, SYSDATE);
END;
/

-- 10. Employee Leave Management (Procedures, Cursors, Exception Handling)
CREATE OR REPLACE PROCEDURE apply_leave(
    p_emp_id NUMBER, p_leave_days NUMBER
) IS
    v_leave_balance NUMBER;
BEGIN
    SELECT leave_balance INTO v_leave_balance FROM employees WHERE employee_id = p_emp_id;
    IF v_leave_balance < p_leave_days THEN
        RAISE_APPLICATION_ERROR(-20001, 'Insufficient leave balance');
    ELSE
        UPDATE employees SET leave_balance = leave_balance - p_leave_days WHERE employee_id = p_emp_id;
    END IF;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RAISE_APPLICATION_ERROR(-20002, 'Employee not found');
END;
/

-- 11. Salary Adjustment by Job Type (Procedures, Dynamic SQL, Cursors, Records, Exception Handling)
CREATE OR REPLACE PROCEDURE adjust_salary_by_job IS
    CURSOR emp_cursor IS SELECT employee_id, job_id, salary FROM employees;
    emp_record employees%ROWTYPE;
    v_sql VARCHAR2(500);
    v_new_salary NUMBER;
BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO emp_record;
        EXIT WHEN emp_cursor%NOTFOUND;
        
        IF emp_record.job_id LIKE 'TECH%' THEN
            v_new_salary := emp_record.salary * 1.05;
        ELSIF emp_record.job_id LIKE 'MAN%' THEN
            v_new_salary := emp_record.salary * 1.10;
        ELSE
            CONTINUE;
        END IF;
        
        v_sql := 'UPDATE employees SET salary = ' || v_new_salary || ' WHERE employee_id = ' || emp_record.employee_id;
        EXECUTE IMMEDIATE v_sql;
    END LOOP;
    CLOSE emp_cursor;
    COMMIT;
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/


-- 12. Employee Leave Management (Collections, Procedures, Exception Handling, Bulk Collect)
CREATE OR REPLACE PROCEDURE process_leave_requests IS
    TYPE leave_table IS TABLE OF leaves%ROWTYPE;
    v_leave_requests leave_table;
    v_leave_balance NUMBER;
BEGIN
    -- Fetch all pending leave requests
    SELECT * BULK COLLECT INTO v_leave_requests FROM leaves WHERE status = 'PENDING';
    
    FOR i IN v_leave_requests.FIRST..v_leave_requests.LAST LOOP
        -- Check leave balance
        SELECT leave_balance INTO v_leave_balance FROM employees WHERE employee_id = v_leave_requests(i).employee_id;
        
        IF v_leave_requests(i).leave_days <= v_leave_balance THEN
            -- Approve leave and update balance
            UPDATE leaves SET status = 'APPROVED' WHERE leave_id = v_leave_requests(i).leave_id;
            UPDATE employees SET leave_balance = leave_balance - v_leave_requests(i).leave_days WHERE employee_id = v_leave_requests(i).employee_id;
        ELSE
            -- Reject leave request
            UPDATE leaves SET status = 'REJECTED' WHERE leave_id = v_leave_requests(i).leave_id;
        END IF;
    END LOOP;
    
    COMMIT;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No leave requests found.');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/

--  13. Departmental Headcount Analysis using Functions, Collections, Bulk Collect, and Loops
CREATE OR REPLACE FUNCTION get_department_headcount 
RETURN SYS_REFCURSOR IS
    TYPE dept_emp_count IS TABLE OF NUMBER INDEX BY PLS_INTEGER;
    dept_count dept_emp_count;
    v_dept_id NUMBER;
    v_emp_count NUMBER;
    result_cursor SYS_REFCURSOR;
BEGIN
    -- Bulk collect department-wise employee count
    SELECT department_id, COUNT(*)
    BULK COLLECT INTO dept_count
    FROM employees
    WHERE department_id IS NOT NULL
    GROUP BY department_id;
    
    -- Open a cursor to return only departments with employee count > 10
    OPEN result_cursor FOR 
        SELECT department_id, COUNT(*) AS employee_count
        FROM employees
        WHERE department_id IS NOT NULL
        GROUP BY department_id
        HAVING COUNT(*) > 10;
    
    RETURN result_cursor;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No departments found with employees.');
        RETURN NULL;
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
        RETURN NULL;
END get_department_headcount;
/



VARIABLE rc REFCURSOR;
EXEC :rc := get_department_headcount;
PRINT rc;


-- 14. Employee Resignation Process using Procedures, Cursors, Dynamic SQL, and Exception Handling
CREATE OR REPLACE PROCEDURE process_employee_resignation (p_employee_id IN employees.employee_id%TYPE) IS
    CURSOR emp_cursor IS 
        SELECT job_id, manager_id, department_id 
        FROM employees 
        WHERE employee_id = p_employee_id;

    v_job_id employees.job_id%TYPE;
    v_manager_id employees.manager_id%TYPE;
    v_department_id employees.department_id%TYPE;
    v_sql VARCHAR2(500);
BEGIN
    -- Fetch employee details
    OPEN emp_cursor;
    FETCH emp_cursor INTO v_job_id, v_manager_id, v_department_id;
    CLOSE emp_cursor;

    IF v_job_id IS NULL THEN
        RAISE_APPLICATION_ERROR(-20001, 'Employee not found.');
    END IF;

    -- Update employee status to 'Resigned'
    v_sql := 'UPDATE employees SET status = ''RESIGNED'' WHERE employee_id = ' || p_employee_id;
    EXECUTE IMMEDIATE v_sql;

    -- Remove login credentials (assuming a separate credentials table exists)
    v_sql := 'DELETE FROM user_credentials WHERE employee_id = ' || p_employee_id;
    EXECUTE IMMEDIATE v_sql;

    -- Transfer responsibilities to the manager
    v_sql := 'UPDATE tasks SET assigned_to = ' || v_manager_id || ' WHERE assigned_to = ' || p_employee_id;
    EXECUTE IMMEDIATE v_sql;

    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Resignation processed successfully for Employee ID: ' || p_employee_id);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: Employee does not exist.');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END process_employee_resignation;
/

-- 15. Dynamic Table Creation for Employee Logs (Dynamic SQL, Procedures, Triggers, Autonomous Transactions)

-- Procedure to dynamically create log tables based on the current year
CREATE OR REPLACE PROCEDURE create_employee_log_table IS
    v_table_name VARCHAR2(50);
    v_sql VARCHAR2(500);
BEGIN
    -- Generate table name based on the current year
    v_table_name := 'employee_log_' || TO_CHAR(SYSDATE, 'YYYY');
    
    -- Construct dynamic SQL to create the table if it doesn't exist
    v_sql := 'CREATE TABLE ' || v_table_name || ' (
                 log_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
                 employee_id NUMBER,
                 action_type VARCHAR2(50),
                 old_salary NUMBER,
                 new_salary NUMBER,
                 updated_by VARCHAR2(100),
                 update_time TIMESTAMP DEFAULT SYSTIMESTAMP
              )';

    -- Execute the dynamic SQL
    EXECUTE IMMEDIATE v_sql;
    
    DBMS_OUTPUT.PUT_LINE('Table ' || v_table_name || ' created successfully.');
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END create_employee_log_table;
/

-- Trigger to log employee salary updates into the correct table
CREATE OR REPLACE TRIGGER trg_employee_salary_log
AFTER UPDATE OF salary ON employees
FOR EACH ROW
DECLARE
    PRAGMA AUTONOMOUS_TRANSACTION; -- Ensures logging is independent of main transaction
    v_table_name VARCHAR2(50);
    v_sql VARCHAR2(500);
BEGIN
    -- Determine the log table name based on the current year
    v_table_name := 'employee_log_' || TO_CHAR(SYSDATE, 'YYYY');
    
    -- Construct dynamic SQL to insert log data
    v_sql := 'INSERT INTO ' || v_table_name || ' 
              (employee_id, action_type, old_salary, new_salary, updated_by, update_time) 
              VALUES (:1, ''SALARY UPDATE'', :2, :3, USER, SYSTIMESTAMP)';

    -- Execute dynamic insert statement
    EXECUTE IMMEDIATE v_sql USING :OLD.employee_id, :OLD.salary, :NEW.salary;
    
    COMMIT; -- Commit in autonomous transaction
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error in trigger: ' || SQLERRM);
END trg_employee_salary_log;
/

-- 16. Employee Appraisal System (Functions, Cursors, CASE Statements, Bulk Collect)

-- Function to evaluate and return the new salary based on appraisal
CREATE OR REPLACE FUNCTION evaluate_appraisal (p_employee_id IN employees.employee_id%TYPE) 
RETURN NUMBER IS
    v_current_salary employees.salary%TYPE;
    v_performance_score NUMBER;
    v_new_salary NUMBER;
BEGIN
    -- Fetch employee's current salary and performance score
    SELECT salary, performance_score 
    INTO v_current_salary, v_performance_score
    FROM employees
    WHERE employee_id = p_employee_id;

    -- Determine salary increase based on performance score
    v_new_salary := v_current_salary * CASE
        WHEN v_performance_score >= 90 THEN 1.20 -- 20% increase
        WHEN v_performance_score >= 80 THEN 1.15 -- 15% increase
        WHEN v_performance_score >= 70 THEN 1.10 -- 10% increase
        ELSE 1.05 -- 5% increase
    END;

    RETURN v_new_salary;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: Employee not found.');
        RETURN NULL;
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
        RETURN NULL;
END evaluate_appraisal;
/

-- Procedure to apply appraisal to all employees using Bulk Collect
CREATE OR REPLACE PROCEDURE apply_appraisal IS
    TYPE emp_table IS TABLE OF employees%ROWTYPE;
    v_employees emp_table;
    v_new_salary NUMBER;
BEGIN
    -- Fetch all employee records using Bulk Collect
    SELECT * BULK COLLECT INTO v_employees FROM employees;

    -- Loop through each employee and update salary
    FOR i IN v_employees.FIRST..v_employees.LAST LOOP
        v_new_salary := evaluate_appraisal(v_employees(i).employee_id);
        
        -- Update salary only if the function returns a valid value
        IF v_new_salary IS NOT NULL THEN
            UPDATE employees 
            SET salary = v_new_salary 
            WHERE employee_id = v_employees(i).employee_id;
        END IF;
    END LOOP;

    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Employee appraisals applied successfully.');
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END apply_appraisal;
/

-- 17. Payroll System with Error Handling (Procedures, Cursors, Exception Handling, Bulk Collect)

-- Table to log payroll errors
CREATE TABLE payroll_errors (
    error_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    employee_id NUMBER,
    error_message VARCHAR2(255),
    error_time TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Procedure to process payroll
CREATE OR REPLACE PROCEDURE process_payroll IS
    TYPE emp_table IS TABLE OF employees%ROWTYPE;
    v_employees emp_table;
    
    v_net_salary NUMBER;
    v_deductions NUMBER := 0.15; -- 15% deduction for taxes, insurance, etc.
    
BEGIN
    -- Fetch all employee records using Bulk Collect
    SELECT * BULK COLLECT INTO v_employees FROM employees;

    -- Loop through each employee
    FOR i IN v_employees.FIRST..v_employees.LAST LOOP
        BEGIN
            -- Check if salary exists
            IF v_employees(i).salary IS NULL THEN
                RAISE_APPLICATION_ERROR(-20001, 'Missing salary details for Employee ID ' || v_employees(i).employee_id);
            END IF;

            -- Calculate net salary after deductions
            v_net_salary := v_employees(i).salary * (1 - v_deductions);

            -- Update net salary in employees table
            UPDATE employees 
            SET net_salary = v_net_salary 
            WHERE employee_id = v_employees(i).employee_id;

        EXCEPTION
            WHEN OTHERS THEN
                -- Log the error and continue processing other employees
                INSERT INTO payroll_errors (employee_id, error_message) 
                VALUES (v_employees(i).employee_id, SQLERRM);
        END;
    END LOOP;

    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Payroll processing completed with error handling.');
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END process_payroll;
/

-- 18. Department Budget Forecasting (Table Functions, Bulk Collect, Aggregations, Cursors)

-- Create a custom type to return forecasted budget data
CREATE OR REPLACE TYPE dept_budget_forecast_type AS OBJECT (
    department_id NUMBER,
    year NUMBER,
    projected_budget NUMBER
);
/

-- Create a table type to store multiple forecasted budgets
CREATE OR REPLACE TYPE dept_budget_forecast_table AS TABLE OF dept_budget_forecast_type;
/

-- Table function to calculate department budget forecast
CREATE OR REPLACE FUNCTION forecast_department_budget
RETURN dept_budget_forecast_table PIPELINED IS
    TYPE dept_salary_table IS TABLE OF employees%ROWTYPE;
    v_employees dept_salary_table;
    
    v_department_id NUMBER;
    v_current_budget NUMBER;
    v_growth_rate NUMBER := 1.05; -- Assume 5% annual salary growth
    v_projected_budget NUMBER;
    
    CURSOR dept_cursor IS 
        SELECT department_id, SUM(salary) AS current_budget
        FROM employees
        GROUP BY department_id;

BEGIN
    -- Open cursor and fetch department-wise salary data
    OPEN dept_cursor;
    LOOP
        FETCH dept_cursor INTO v_department_id, v_current_budget;
        EXIT WHEN dept_cursor%NOTFOUND;
        
        -- Calculate budget forecast for the next 5 years
        FOR year_offset IN 1..5 LOOP
            v_projected_budget := v_current_budget * POWER(v_growth_rate, year_offset);
            
            -- Return the projected data using PIPE ROW
            PIPE ROW (dept_budget_forecast_type(v_department_id, EXTRACT(YEAR FROM SYSDATE) + year_offset, v_projected_budget));
        END LOOP;
        
    END LOOP;
    CLOSE dept_cursor;
    
    RETURN;
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
        RETURN;
END forecast_department_budget;
/

-- 19. Employee Role Change Process (Procedures, Triggers, Dynamic SQL, Exception Handling, Autonomous Transactions)

-- Create a table to log role changes
CREATE TABLE employee_role_changes (
    change_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    employee_id NUMBER,
    old_job VARCHAR2(50),
    new_job VARCHAR2(50),
    change_date TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Procedure to update employee role
CREATE OR REPLACE PROCEDURE update_employee_role(
    p_employee_id NUMBER,
    p_new_job_id VARCHAR2
) IS
    v_old_job VARCHAR2(50);
    v_new_salary NUMBER;
    v_sql VARCHAR2(500);
    PRAGMA AUTONOMOUS_TRANSACTION;
BEGIN
    -- Get the old job title
    SELECT job_id INTO v_old_job FROM employees WHERE employee_id = p_employee_id;

    -- Determine the new salary based on role
    CASE 
        WHEN p_new_job_id LIKE 'TECH%' THEN v_new_salary := (SELECT salary FROM employees WHERE employee_id = p_employee_id) * 1.10;
        WHEN p_new_job_id LIKE 'MAN%' THEN v_new_salary := (SELECT salary FROM employees WHERE employee_id = p_employee_id) * 1.20;
        ELSE v_new_salary := (SELECT salary FROM employees WHERE employee_id = p_employee_id) * 1.05;
    END CASE;

    -- Dynamic SQL to update job title and salary
    v_sql := 'UPDATE employees SET job_id = ''' || p_new_job_id || ''', salary = ' || v_new_salary || ' WHERE employee_id = ' || p_employee_id;
    EXECUTE IMMEDIATE v_sql;

    -- Log the role change
    INSERT INTO employee_role_changes (employee_id, old_job, new_job)
    VALUES (p_employee_id, v_old_job, p_new_job_id);
    
    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Role updated successfully for Employee ID: ' || p_employee_id);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: Employee ID not found.');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END update_employee_role;
/

-- Trigger to notify role change
CREATE OR REPLACE TRIGGER notify_role_change
AFTER UPDATE OF job_id ON employees
FOR EACH ROW
BEGIN
    DBMS_OUTPUT.PUT_LINE('Notification: Employee ID ' || :OLD.employee_id || ' role changed from ' || :OLD.job_id || ' to ' || :NEW.job_id);
END;
/

-- 20. Cross-Database Employee Audit (Database Links, Cursors, Procedures, Exception Handling)

-- Step 1: Create a database link (Ensure you replace 'hr_remote' with actual remote DB credentials)
CREATE DATABASE LINK hr_remote_link 
CONNECT TO hr_user IDENTIFIED BY hr_password 
USING 'hr_remote';

-- Step 2: Create an audit table to store employee details from the remote database
CREATE TABLE employee_audit (
    audit_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    employee_id NUMBER,
    first_name VARCHAR2(50),
    last_name VARCHAR2(50),
    job_id VARCHAR2(50),
    salary NUMBER,
    audit_date TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Step 3: Create a procedure to fetch data from the remote HR database and store it in the local audit table
CREATE OR REPLACE PROCEDURE fetch_remote_employees IS
    CURSOR emp_cursor IS 
        SELECT employee_id, first_name, last_name, job_id, salary 
        FROM employees@hr_remote_link;
    
    emp_record employees%ROWTYPE;
BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO emp_record;
        EXIT WHEN emp_cursor%NOTFOUND;

        -- Insert into the local audit table
        INSERT INTO employee_audit (employee_id, first_name, last_name, job_id, salary)
        VALUES (emp_record.employee_id, emp_record.first_name, emp_record.last_name, emp_record.job_id, emp_record.salary);
    END LOOP;
    CLOSE emp_cursor;
    
    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Employee records fetched successfully from remote database.');
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employee records found in remote database.');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END fetch_remote_employees;
/

-- Step 4: Execute the procedure to fetch and audit employee details
EXEC fetch_remote_employees;

-- Step 5: Verify the audited employee records
SELECT * FROM employee_audit;

-->

