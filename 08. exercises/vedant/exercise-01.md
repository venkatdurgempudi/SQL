# **50 SQL Questions**  
The questions combine various subtopics like single-row functions, aggregate functions, joins, and other operations.


<br>  

<br>  

  
## Below is DDL & DML for the `CUSTOMERS` and `ORDERS` tables.  



### **DDL for `CUSTOMERS` Table**

```sql
CREATE TABLE CUSTOMERS (
    CUSTOMER_ID NUMBER PRIMARY KEY,
    FIRST_NAME VARCHAR2(50),
    LAST_NAME VARCHAR2(50),
    EMAIL VARCHAR2(100),
    CITY VARCHAR2(50),
    ACCOUNT_CREATION_DATE DATE
);
```

### **DDL for `ORDERS` Table**

```sql
CREATE TABLE ORDERS (
    ORDER_ID NUMBER PRIMARY KEY,
    CUSTOMER_ID NUMBER,
    ORDER_DATE DATE,
    ORDER_AMOUNT NUMBER(10, 2),
    FOREIGN KEY (CUSTOMER_ID) REFERENCES CUSTOMERS(CUSTOMER_ID)
);
```


### **Complete Dataset (DML for `CUSTOMERS` Table - 200 rows)**

```sql
INSERT INTO CUSTOMERS (CUSTOMER_ID, FIRST_NAME, LAST_NAME, EMAIL, CITY, ACCOUNT_CREATION_DATE)
VALUES (1, 'Aaron', 'Johnson', 'aaron.johnson@example.com', 'New York', TO_DATE('12-JAN-2021', 'DD-MON-YYYY'));

INSERT INTO CUSTOMERS (CUSTOMER_ID, FIRST_NAME, LAST_NAME, EMAIL, CITY, ACCOUNT_CREATION_DATE)
VALUES (2, 'Alex', 'Davidson', 'alex.davidson@example.com', 'Los Angeles', TO_DATE('08-MAR-2020', 'DD-MON-YYYY'));

INSERT INTO CUSTOMERS (CUSTOMER_ID, FIRST_NAME, LAST_NAME, EMAIL, CITY, ACCOUNT_CREATION_DATE)
VALUES (3, 'Ben', 'Miller', 'ben.miller@example.com', 'Chicago', TO_DATE('01-FEB-2021', 'DD-MON-YYYY'));

INSERT INTO CUSTOMERS (CUSTOMER_ID, FIRST_NAME, LAST_NAME, EMAIL, CITY, ACCOUNT_CREATION_DATE)
VALUES (4, 'Catherine', 'Stewart', 'catherine.stewart@example.com', 'New York', TO_DATE('15-SEP-2021', 'DD-MON-YYYY'));

INSERT INTO CUSTOMERS (CUSTOMER_ID, FIRST_NAME, LAST_NAME, EMAIL, CITY, ACCOUNT_CREATION_DATE)
VALUES (5, 'Diana', 'Green', 'diana.green@example.com', 'San Francisco', TO_DATE('22-MAY-2022', 'DD-MON-YYYY'));

-- Repeat similar inserts to create a total of 200 customers

-- Auto-generate customer data using a loop in your preferred method
-- Example for generating a few rows at a time:
DECLARE
    v_id NUMBER := 6;
BEGIN
    FOR i IN 1..195 LOOP
        INSERT INTO CUSTOMERS (CUSTOMER_ID, FIRST_NAME, LAST_NAME, EMAIL, CITY, ACCOUNT_CREATION_DATE)
        VALUES (v_id, 'Customer'||i, 'Last'||i, 'customer'||i||'@example.com', 'City'||MOD(i, 5), SYSDATE - i*30);
        v_id := v_id + 1;
    END LOOP;
END;
/
```


### **Complete Dataset (DML for `ORDERS` Table - 200 rows)**

```sql
INSERT INTO ORDERS (ORDER_ID, CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT)
VALUES (101, 1, TO_DATE('15-JAN-2021', 'DD-MON-YYYY'), 1500.75);

INSERT INTO ORDERS (ORDER_ID, CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT)
VALUES (102, 2, TO_DATE('20-MAR-2020', 'DD-MON-YYYY'), 250.00);

INSERT INTO ORDERS (ORDER_ID, CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT)
VALUES (103, 3, TO_DATE('25-FEB-2021', 'DD-MON-YYYY'), 980.60);

INSERT INTO ORDERS (ORDER_ID, CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT)
VALUES (104, 1, TO_DATE('10-AUG-2021', 'DD-MON-YYYY'), 520.45);

INSERT INTO ORDERS (ORDER_ID, CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT)
VALUES (105, 4, TO_DATE('12-NOV-2021', 'DD-MON-YYYY'), 2200.10);

-- Repeat similar inserts to create a total of 200 orders
DECLARE
    v_id NUMBER := 106;
BEGIN
    FOR i IN 1..195 LOOP
        INSERT INTO ORDERS (ORDER_ID, CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT)
        VALUES (v_id, MOD(i, 200) + 1, SYSDATE - i*15, ROUND(DBMS_RANDOM.VALUE(100, 5000), 2));
        v_id := v_id + 1;
    END LOOP;
END;
/
```


<br>  
<br>  


## Assignment  


**1.** Retrieve the total amount spent by each customer, their first name, and city.  
**2.** Find customers who placed an order after January 1, 2021, and calculate the average amount spent per order.  
**3.** Retrieve all customers from New York who have placed more than 2 orders.  
**4.** Show the first and last names of customers who placed an order between 500 and 2000.  
**5.** Find the maximum order amount placed by customers in each city.  
**6.** Retrieve customers who placed orders in February 2021 and order them by order amount in descending order.  
**7.** List customers whose email contains 'example.com' and calculate the total number of such customers.  
**8.** Retrieve all customers who have placed an order whose amount is more than the average order amount of all customers.  
**9.** Find customers who have not placed any orders since January 1, 2021.  
**10.** Retrieve the total number of customers from each city, and the sum of their order amounts.  
**11.** Calculate the number of days since each customer’s account creation date and order the results by customer ID.  
**12.** Retrieve the first order placed by each customer and display their name, order date, and amount.  
**13.** Show the total number of customers who placed orders in each month of 2021.  
**14.** Find customers who placed an order with an amount greater than the highest order placed in New York.  
**15.** List all customers who placed at least one order in March 2022.  
**16.** Retrieve the total order amount, rounded to 2 decimal places, for each customer whose last name starts with 'S'.  
**17.** Find the difference between the highest and lowest order amounts for customers in Los Angeles.  
**18.** Retrieve the last order date for each customer and the total amount they spent.  
**19.** Retrieve all customers whose first name starts with 'A' and who placed an order above 1000 in the last 6 months.  
**20.** Show the total amount of orders placed by customers from each city, grouped by city and sorted by total amount.  
**21.** Retrieve all customers whose account creation date is earlier than the current year and who have placed orders this year.  
**22.** Find customers who placed orders totaling more than 3000 but have not placed an order in the last 6 months.  
**23.** Retrieve the total amount of orders placed on each day of January 2021.  
**24.** Calculate the average number of orders per customer for customers from California.  
**25.** Find customers who have placed orders every month since their account creation date.  
**26.** Retrieve the maximum, minimum, and average order amounts for customers whose account creation date is in 2020.  
**27.** List the total number of orders and total amount spent for customers with more than 3 orders.  
**28.** Retrieve customers who have placed orders in more than one city and display their total order amount.  
**29.** Find customers who placed an order with an amount above 1000 but did not place any orders in the previous year.  
**30.** Show the total number of orders and the total amount spent by customers whose first names are at least 6 characters long.  
**31.** Retrieve all customers who placed an order in 2021 and have not placed another order since then.  
**32.** List the total number of customers and the average order amount for customers who placed orders in at least 3 different months.  
**33.** Find the percentage of customers from New York who placed orders above 2000.  
**34.** Show the difference between the total order amount of customers in California and those in Texas.  
**35.** Retrieve all customers who placed at least one order in the last 30 days and show their total order amount.  
**36.** Find the customer who placed the highest number of orders and display the total amount they spent.  
**37.** Retrieve the first and last name of customers who placed an order exactly 6 months after their account creation date.  
**38.** List customers whose first order amount was more than twice the average order amount of all customers.  
**39.** Retrieve the last 3 orders placed by each customer in New York and their total amount.  
**40.** Find the number of customers who placed an order of more than 1000 in 2021 but did not place any orders in 2022.  
**41.** Show customers who placed at least 2 orders in the last 12 months and display their total amount spent.  
**42.** Retrieve customers who placed orders with a total value higher than the median order amount of all customers.  
**43.** List customers whose order amount is within 20% of the highest order placed this year.  
**44.** Find customers who placed orders in exactly 3 different months of the current year.  
**45.** Retrieve the last name and total order amount of customers who placed their first order before turning 25.  
**46.** List all customers whose account creation date is in the current month and display their total order amount.  
**47.** Find customers who placed orders with a total value that is a multiple of 500.  
**48.** Retrieve the first and last order dates for each customer and calculate the difference between them.  
**49.** Show customers who placed an order more than 2 years after their account creation date.  
**50.** Find the customers who placed orders with an amount greater than the average of their previous 3 orders.

---
