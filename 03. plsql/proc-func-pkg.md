### 1. **Procedures**

#### a. **Procedure to Update Employee Salary**
   **Scenario**: Increase an employee's salary by a certain percentage and log the change.
   ```sql
   CREATE OR REPLACE PROCEDURE Update_Employee_Salary(
       p_employee_id IN NUMBER,
       p_percentage IN NUMBER
   ) AS
   BEGIN
       UPDATE employees
       SET salary = salary + (salary * p_percentage / 100)
       WHERE employee_id = p_employee_id;

       INSERT INTO salary_log (employee_id, change_amount, change_date)
       VALUES (p_employee_id, salary * p_percentage / 100, SYSDATE);
   END;
   ```

---

#### b. **Procedure to Add a New Department**
   **Scenario**: Create a new department with a specified name, manager, and location.
   ```sql
   CREATE OR REPLACE PROCEDURE Add_Department(
       p_department_name IN VARCHAR2,
       p_manager_id IN NUMBER,
       p_location_id IN NUMBER
   ) AS
   BEGIN
       INSERT INTO departments (department_id, department_name, manager_id, location_id)
       VALUES (departments_seq.NEXTVAL, p_department_name, p_manager_id, p_location_id);
   END;
   ```

---

### 2. **Functions**

#### a. **Function to Calculate Total Salary for a Department**
   **Scenario**: Return the total salary of all employees in a given department.
   ```sql
   CREATE OR REPLACE FUNCTION Get_Total_Salary(
       p_department_id IN NUMBER
   ) RETURN NUMBER AS
       v_total_salary NUMBER;
   BEGIN
       SELECT SUM(salary) INTO v_total_salary
       FROM employees
       WHERE department_id = p_department_id;

       RETURN v_total_salary;
   END;
   ```

---

#### b. **Function to Get Manager Name**
   **Scenario**: Retrieve the manager's name for a given employee.
   ```sql
   CREATE OR REPLACE FUNCTION Get_Manager_Name(
       p_employee_id IN NUMBER
   ) RETURN VARCHAR2 AS
       v_manager_name VARCHAR2(100);
   BEGIN
       SELECT e.first_name || ' ' || e.last_name
       INTO v_manager_name
       FROM employees e
       WHERE e.employee_id = (
           SELECT manager_id FROM employees WHERE employee_id = p_employee_id
       );

       RETURN v_manager_name;
   END;
   ```

---

### 3. **Packages**

#### a. **Employee Management Package**
   **Scenario**: Create a package to manage employees, including hiring and firing.

   **Specification:**
   ```sql
   CREATE OR REPLACE PACKAGE emp_mgmt_pkg AS
       PROCEDURE Hire_Employee(
           p_first_name IN VARCHAR2,
           p_last_name IN VARCHAR2,
           p_email IN VARCHAR2,
           p_job_id IN VARCHAR2,
           p_salary IN NUMBER,
           p_department_id IN NUMBER
       );

       PROCEDURE Fire_Employee(p_employee_id IN NUMBER);
   END emp_mgmt_pkg;
   ```

   **Body:**
   ```sql
   CREATE OR REPLACE PACKAGE BODY emp_mgmt_pkg AS
       PROCEDURE Hire_Employee(
           p_first_name IN VARCHAR2,
           p_last_name IN VARCHAR2,
           p_email IN VARCHAR2,
           p_job_id IN VARCHAR2,
           p_salary IN NUMBER,
           p_department_id IN NUMBER
       ) AS
       BEGIN
           INSERT INTO employees (employee_id, first_name, last_name, email, job_id, salary, department_id, hire_date)
           VALUES (employees_seq.NEXTVAL, p_first_name, p_last_name, p_email, p_job_id, p_salary, p_department_id, SYSDATE);
       END;

       PROCEDURE Fire_Employee(p_employee_id IN NUMBER) AS
       BEGIN
           DELETE FROM employees WHERE employee_id = p_employee_id;
       END;
   END emp_mgmt_pkg;
   ```

---

#### b. **HR Utilities Package**
   **Scenario**: Provide utilities for common HR tasks.

   **Specification:**
   ```sql
   CREATE OR REPLACE PACKAGE hr_utils_pkg AS
       FUNCTION Get_Department_Count RETURN NUMBER;
       FUNCTION Get_Employee_Count RETURN NUMBER;
   END hr_utils_pkg;
   ```

   **Body:**
   ```sql
   CREATE OR REPLACE PACKAGE BODY hr_utils_pkg AS
       FUNCTION Get_Department_Count RETURN NUMBER AS
           v_count NUMBER;
       BEGIN
           SELECT COUNT(*) INTO v_count FROM departments;
           RETURN v_count;
       END;

       FUNCTION Get_Employee_Count RETURN NUMBER AS
           v_count NUMBER;
       BEGIN
           SELECT COUNT(*) INTO v_count FROM employees;
           RETURN v_count;
       END;
   END hr_utils_pkg;
   ```

---

### 4. **Advanced Scenarios with Packages**

#### **Payroll Package**
   **Scenario**: Manage payroll operations.
   - **Specification**:
     ```sql
     CREATE OR REPLACE PACKAGE payroll_pkg AS
         FUNCTION Calculate_Annual_Salary(p_employee_id IN NUMBER) RETURN NUMBER;
         PROCEDURE Process_Payroll(p_month IN VARCHAR2);
     END payroll_pkg;
     ```

   - **Body**:
     ```sql
     CREATE OR REPLACE PACKAGE BODY payroll_pkg AS
         FUNCTION Calculate_Annual_Salary(p_employee_id IN NUMBER) RETURN NUMBER AS
             v_annual_salary NUMBER;
         BEGIN
             SELECT salary * 12 INTO v_annual_salary FROM employees WHERE employee_id = p_employee_id;
             RETURN v_annual_salary;
         END;

         PROCEDURE Process_Payroll(p_month IN VARCHAR2) AS
         BEGIN
             INSERT INTO payroll_log (employee_id, salary_paid, payroll_month, processed_date)
             SELECT employee_id, salary, p_month, SYSDATE FROM employees;
         END;
     END payroll_pkg;
     ```

