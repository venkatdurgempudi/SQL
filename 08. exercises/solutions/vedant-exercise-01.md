### **Solutions**

---

Here are the SQL solutions for the provided queries:

---

### 1. Retrieve the total amount spent by each customer, their first name, and city.
```sql
SELECT C.FIRST_NAME, C.CITY, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.CITY;
```

---

### 2. Find customers who placed an order after January 1, 2021, and calculate the average amount spent per order.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, AVG(O.ORDER_AMOUNT) AS AVG_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE > TO_DATE('2021-01-01', 'YYYY-MM-DD')
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME;
```

---

### 3. Retrieve all customers from New York who have placed more than 2 orders.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.CITY = 'New York'
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(O.ORDER_ID) > 2;
```

---

### 4. Show the first and last names of customers who placed an order between 500 and 2000.
```sql
SELECT DISTINCT C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT BETWEEN 500 AND 2000;
```

---

### 5. Find the maximum order amount placed by customers in each city.
```sql
SELECT C.CITY, MAX(O.ORDER_AMOUNT) AS MAX_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CITY;
```

---

### 6. Retrieve customers who placed orders in February 2021 and order them by order amount in descending order.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE BETWEEN TO_DATE('2021-02-01', 'YYYY-MM-DD') AND TO_DATE('2021-02-28', 'YYYY-MM-DD')
ORDER BY O.ORDER_AMOUNT DESC;
```

---

### 7. List customers whose email contains 'example.com' and calculate the total number of such customers.
```sql
SELECT COUNT(*) AS TOTAL_CUSTOMERS
FROM Customers
WHERE EMAIL LIKE '%example.com%';
```

---

### 8. Retrieve all customers who have placed an order whose amount is more than the average order amount of all customers.
```sql
WITH AvgOrder AS (
    SELECT AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
)
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > (SELECT AVG_ORDER_AMOUNT FROM AvgOrder);
```

---

### 9. Find customers who have not placed any orders since January 1, 2021.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
WHERE C.CUSTOMER_ID NOT IN (
    SELECT O.CUSTOMER_ID
    FROM Orders O
    WHERE O.ORDER_DATE >= TO_DATE('2021-01-01', 'YYYY-MM-DD')
);
```

---

### 10. Retrieve the total number of customers from each city, and the sum of their order amounts.
```sql
SELECT C.CITY, COUNT(DISTINCT C.CUSTOMER_ID) AS TOTAL_CUSTOMERS, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
LEFT JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CITY;
```

---

### 11. Calculate the number of days since each customer’s account creation date and order the results by customer ID.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, TRUNC(SYSDATE - C.ACCOUNT_CREATION_DATE) AS DAYS_SINCE_CREATION
FROM Customers C
ORDER BY C.CUSTOMER_ID;
```

---

### 12. Retrieve the first order placed by each customer and display their name, order date, and amount.
```sql
WITH FirstOrders AS (
    SELECT CUSTOMER_ID, 
           MIN(ORDER_DATE) AS FIRST_ORDER_DATE
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_DATE, O.ORDER_AMOUNT
FROM Customers C
JOIN FirstOrders FO ON C.CUSTOMER_ID = FO.CUSTOMER_ID
JOIN Orders O ON FO.CUSTOMER_ID = O.CUSTOMER_ID AND FO.FIRST_ORDER_DATE = O.ORDER_DATE;
```

---

### 13. Show the total number of customers who placed orders in each month of 2021.
```sql
SELECT EXTRACT(MONTH FROM O.ORDER_DATE) AS MONTH, COUNT(DISTINCT O.CUSTOMER_ID) AS TOTAL_CUSTOMERS
FROM Orders O
WHERE O.ORDER_DATE >= TO_DATE('2021-01-01', 'YYYY-MM-DD') AND O.ORDER_DATE < TO_DATE('2022-01-01', 'YYYY-MM-DD')
GROUP BY EXTRACT(MONTH FROM O.ORDER_DATE)
ORDER BY MONTH;
```

---

### 14. Find customers who placed an order with an amount greater than the highest order placed in New York.
```sql
WITH MaxOrderNY AS (
    SELECT MAX(O.ORDER_AMOUNT) AS MAX_ORDER_AMOUNT
    FROM Customers C
    JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    WHERE C.CITY = 'New York'
)
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > (SELECT MAX_ORDER_AMOUNT FROM MaxOrderNY);
```

---

### 15. List all customers who placed at least one order in March 2022.
```sql
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE BETWEEN TO_DATE('2022-03-01', 'YYYY-MM-DD') AND TO_DATE('2022-03-31', 'YYYY-MM-DD');
```

---

### 16. Retrieve the total order amount, rounded to 2 decimal places, for each customer whose last name starts with 'S'.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, ROUND(SUM(O.ORDER_AMOUNT), 2) AS TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.LAST_NAME LIKE 'S%'
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME;
```

---

### 17. Find the difference between the highest and lowest order amounts for customers in Los Angeles.
```sql
SELECT MAX(O.ORDER_AMOUNT) - MIN(O.ORDER_AMOUNT) AS ORDER_AMOUNT_DIFFERENCE
FROM Orders O
JOIN Customers C ON O.CUSTOMER_ID = C.CUSTOMER_ID
WHERE C.CITY = 'Los Angeles';
```

---

### 18. Retrieve the last order date for each customer and the total amount they spent.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, MAX(O.ORDER_DATE) AS LAST_ORDER_DATE, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME;
```

---

### 19. Retrieve all customers whose first name starts with 'A' and who placed an order above 1000 in the last 6 months.
```sql
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.FIRST_NAME LIKE 'A%' AND O.ORDER_AMOUNT > 1000 AND O.ORDER_DATE >= ADD_MONTHS(SYSDATE, -6);
```

---

### 20. Show the total amount of orders placed by customers from each city, grouped by city and sorted by total amount.
```sql
SELECT C.CITY, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CITY
ORDER BY TOTAL_ORDER_AMOUNT DESC;
```

---


