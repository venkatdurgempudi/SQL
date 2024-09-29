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

1. Retrieve the names of customers who have placed the most orders on the first day of each quarter for the past three years.  
2. Find the customers whose total order amount, over all their orders, is a perfect square and display their total order count.  
3. For each customer, find the month in which they placed the highest number of orders and show their total order amount for that month.  
4. Identify customers whose total amount spent exceeds the total amount spent by 75% of all other customers and display their total orders.  
5. Retrieve customers who have placed an order every single day in at least one month of the current year and list their total orders.  
6. List customers who have never placed an order on the 15th day of any month and display their total orders and the first order date.  
7. Show customers who have a decreasing trend in their average order amount, i.e., their monthly average order amount has consistently decreased for the last three months.  
8. Find customers whose orders contain at least one instance of an order amount that is exactly the mean of all their other orders and list their total orders.  
9. Retrieve customers who have placed the largest and the smallest orders in the same month and show their total amount spent that month.  
10. List customers whose total amount spent in the last 30 days is at least 150% of their average order amount over their entire order history.  
11. Identify customers who placed orders in all 12 months of the previous year and show their total order amount for that year.  
12. Find customers whose first order amount is greater than the average order amount of all customers but whose last order amount is less than the average.  
13. Retrieve customers who have a standard deviation of their order amounts greater than the average of all customers and display their total orders.  
14. List customers who have not placed any orders in the last year but have a birthday within the next 30 days.  
15. Show the total number of orders placed by customers who only place orders on weekdays and their average order value.  
16. Retrieve customers whose orders contain an order amount that is the median of all their orders and display their total orders.  
17. Find customers who have a pattern of placing orders on the last day of each month for the last year and show their total order amounts.  
18. List customers who have spent more on average in the first half of the year compared to the second half and display their total orders.  
19. Identify customers who placed an order in every week of the year and show the total order amount for that year.  
20. Retrieve customers who have a positive correlation between the number of orders placed and the order amounts over the past year.  
21. Find customers whose orders are always placed at a specific time of day (e.g., between 6 PM and 8 PM) and show their total order amounts.  
22. List customers who have placed orders on holidays and display their total orders and the average order amount on those days.  
23. Identify customers who have placed orders with an amount that is the Fibonacci sequence for the last six months and list their total orders.  
24. Retrieve customers who have a consistent increase in order amount every month for six consecutive months and display their total orders.  
25. Find customers whose total order amount is a palindrome and show their total order count.  
26. Show customers who have placed more than one order on the same day in the same month for the last three years and display their total orders.  
27. Identify customers who have orders with an amount that is a power of two and list their total orders.  
28. Retrieve customers who have spent more than 2000 in one order but have an average order amount of less than 500 overall.  
29. List customers who placed the highest order on the last day of the year and show their total order amount for that year.  
30. Find customers whose total amount spent in the last month is greater than the total amount spent in the previous month and display their total orders.  
31. Retrieve customers who placed at least one order during each month of a specific quarter and show their total order amount.  
32. Identify customers whose orders show a cyclic pattern of spending (e.g., high-low-high-low) over a period of time and display their total orders.  
33. Show customers who have a higher order count but lower order amounts compared to their peers and display their average order amount.  
34. Find customers who placed orders on the same date every year for three consecutive years and display their total orders.  
35. List customers whose orders have an amount that is a significant outlier compared to the average order amounts of their peers.  
36. Retrieve customers whose order history reflects a seasonal trend (e.g., higher in December) and show their total order amounts for those seasons.  
37. Identify customers who have made repeat purchases of the same product multiple times and display their total spending on those products.  
38. Find customers whose average order value increases significantly (by 25% or more) after a specific marketing campaign and show their total orders.  
39. List customers who have placed orders that match the mode of their previous order amounts and display their total orders.  
40. Retrieve customers who have maintained an average order amount within a specific range (e.g., 1000 to 2000) over a long period and show their total orders.  
41. Identify customers who place the highest number of orders during a promotional period and display their total order amounts for that period.  
42. Find customers who have placed orders with a total amount exceeding the combined amounts of their last three orders and show their total orders.  
43. Show customers whose order history shows a pattern of decreasing frequency but increasing average order value over time.  
44. Retrieve customers who placed orders that exactly match the average order amount of all customers and display their total orders.  
45. List customers who have an average order amount that is consistently higher than the average of their segment and display their total orders.  
46. Identify customers whose orders reflect an increase in order value but a decrease in frequency and display their total order amounts.  
47. Find customers who have been inactive for the longest time and display their total order count before inactivity began.  
48. Retrieve customers who have made purchases from multiple categories and show their total spending across those categories.  
49. List customers whose orders include unique order amounts that differ from their average order amount and display their total orders.  
50. Find customers who have an increasing order amount pattern during the last six months and display their total orders and average order amount.

---

