### **Solutions**


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


### 21. Retrieve all customers whose account creation date is earlier than the current year and who have placed orders this year.
```sql
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.ACCOUNT_CREATION_DATE < TRUNC(SYSDATE, 'YYYY')
  AND O.ORDER_DATE >= TRUNC(SYSDATE, 'YYYY');
```

---

### 22. Find customers who placed orders totaling more than 3000 but have not placed an order in the last 6 months.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
HAVING SUM(O.ORDER_AMOUNT) > 3000
   AND MAX(O.ORDER_DATE) < ADD_MONTHS(SYSDATE, -6);
```

---

### 23. Retrieve the total amount of orders placed on each day of January 2021.
```sql
SELECT O.ORDER_DATE, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Orders O
WHERE O.ORDER_DATE >= TO_DATE('2021-01-01', 'YYYY-MM-DD')
  AND O.ORDER_DATE < TO_DATE('2021-02-01', 'YYYY-MM-DD')
GROUP BY O.ORDER_DATE
ORDER BY O.ORDER_DATE;
```

---

### 24. Calculate the average number of orders per customer for customers from California.
```sql
SELECT AVG(OrderCounts.ORDER_COUNT) AS AVG_ORDERS_PER_CUSTOMER
FROM (
    SELECT C.CUSTOMER_ID, COUNT(O.ORDER_ID) AS ORDER_COUNT
    FROM Customers C
    LEFT JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    WHERE C.STATE = 'California'
    GROUP BY C.CUSTOMER_ID
) OrderCounts;
```

---

### 25. Find customers who have placed orders every month since their account creation date.
```sql
WITH MonthlyOrders AS (
    SELECT C.CUSTOMER_ID, COUNT(DISTINCT EXTRACT(YEAR FROM O.ORDER_DATE) || '-' || EXTRACT(MONTH FROM O.ORDER_DATE)) AS MONTHS_ORDERED
    FROM Customers C
    JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    WHERE O.ORDER_DATE >= C.ACCOUNT_CREATION_DATE
    GROUP BY C.CUSTOMER_ID
)
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN MonthlyOrders MO ON C.CUSTOMER_ID = MO.CUSTOMER_ID
WHERE MO.MONTHS_ORDERED = MONTHS_BETWEEN(TRUNC(SYSDATE), C.ACCOUNT_CREATION_DATE);
```

---

### 26. Retrieve the maximum, minimum, and average order amounts for customers whose account creation date is in 2020.
```sql
SELECT MAX(O.ORDER_AMOUNT) AS MAX_ORDER_AMOUNT,
       MIN(O.ORDER_AMOUNT) AS MIN_ORDER_AMOUNT,
       AVG(O.ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.ACCOUNT_CREATION_DATE >= TO_DATE('2020-01-01', 'YYYY-MM-DD')
  AND C.ACCOUNT_CREATION_DATE < TO_DATE('2021-01-01', 'YYYY-MM-DD');
```

---

### 27. List the total number of orders and total amount spent for customers with more than 3 orders.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(O.ORDER_ID) > 3;
```

---

### 28. Retrieve customers who have placed orders in more than one city and display their total order amount.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN Customers C2 ON O.CUSTOMER_ID = C2.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(DISTINCT C2.CITY) > 1;
```

---

### 29. Find customers who placed an order with an amount above 1000 but did not place any orders in the previous year.
```sql
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > 1000
  AND C.CUSTOMER_ID NOT IN (
    SELECT O.CUSTOMER_ID
    FROM Orders O
    WHERE O.ORDER_DATE < TO_DATE('2021-01-01', 'YYYY-MM-DD')
      AND O.ORDER_DATE >= TO_DATE('2020-01-01', 'YYYY-MM-DD')
);
```

---

### 30. Show the total number of orders and the total amount spent by customers whose first names are at least 6 characters long.
```sql
SELECT COUNT(O.ORDER_ID) AS TOTAL_ORDERS, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE LENGTH(C.FIRST_NAME) >= 6;
```

---

### 31. Retrieve all customers who placed an order in 2021 and have not placed another order since then.
```sql
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE BETWEEN TO_DATE('2021-01-01', 'YYYY-MM-DD') AND TO_DATE('2021-12-31', 'YYYY-MM-DD')
  AND C.CUSTOMER_ID NOT IN (
    SELECT O.CUSTOMER_ID
    FROM Orders O
    WHERE O.ORDER_DATE > TO_DATE('2021-12-31', 'YYYY-MM-DD')
);
```

---

### 32. List the total number of customers and the average order amount for customers who placed orders in at least 3 different months.
```sql
SELECT COUNT(DISTINCT C.CUSTOMER_ID) AS TOTAL_CUSTOMERS, AVG(O.ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID
HAVING COUNT(DISTINCT EXTRACT(YEAR FROM O.ORDER_DATE) || '-' || EXTRACT(MONTH FROM O.ORDER_DATE)) >= 3;
```

---

### 33. Find the percentage of customers from New York who placed orders above 2000.
```sql
WITH TotalCustomers AS (
    SELECT COUNT(*) AS TOTAL FROM Customers WHERE CITY = 'New York'
),
HighValueCustomers AS (
    SELECT COUNT(DISTINCT C.CUSTOMER_ID) AS HIGH_VALUE_TOTAL
    FROM Customers C
    JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    WHERE C.CITY = 'New York' AND O.ORDER_AMOUNT > 2000
)
SELECT (HIGH_VALUE_TOTAL * 100.0 / TOTAL) AS PERCENTAGE_HIGH_VALUE_CUSTOMERS
FROM TotalCustomers, HighValueCustomers;
```

---

### 34. Show the difference between the total order amount of customers in California and those in Texas.
```sql
SELECT 
    (SELECT SUM(O.ORDER_AMOUNT) FROM Customers C JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID WHERE C.STATE = 'California') AS TOTAL_CA,
    (SELECT SUM(O.ORDER_AMOUNT) FROM Customers C JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID WHERE C.STATE = 'Texas') AS TOTAL_TX,
    (SELECT SUM(O.ORDER_AMOUNT) FROM Customers C JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID WHERE C.STATE = 'California') -
    (SELECT SUM(O.ORDER_AMOUNT) FROM Customers C JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID WHERE C.STATE = 'Texas') AS DIFFERENCE;
```

---

### 35. Retrieve all customers who placed at least one order in the last 30 days and show their total order amount.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE >= SYSDATE - 30
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME;
```

---


### 36. Find the customer who placed the highest number of orders and display the total amount they spent.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
ORDER BY TOTAL_ORDERS DESC
FETCH FIRST 1 ROW ONLY;
```

---

### 37. Retrieve the first and last name of customers who placed an order exactly 6 months after their account creation date.
```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE = ADD_MONTHS(C.ACCOUNT_CREATION_DATE, 6);
```

---

### 38. List customers whose first order amount was more than twice the average order amount of all customers.
```sql
WITH AvgOrder AS (
    SELECT AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
),
FirstOrders AS (
    SELECT CUSTOMER_ID, MIN(ORDER_AMOUNT) AS FIRST_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.FIRST_NAME, C.LAST_NAME, FO.FIRST_ORDER_AMOUNT
FROM Customers C
JOIN FirstOrders FO ON C.CUSTOMER_ID = FO.CUSTOMER_ID
WHERE FO.FIRST_ORDER_AMOUNT > 2 * (SELECT AVG_ORDER_AMOUNT FROM AvgOrder);
```

---

### 39. Retrieve the last 3 orders placed by each customer in New York and their total amount.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, O.ORDER_DATE, O.ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.CITY = 'New York'
ORDER BY O.ORDER_DATE DESC
FETCH FIRST 3 ROWS ONLY;
```

---

### 40. Find the number of customers who placed an order of more than 1000 in 2021 but did not place any orders in 2022.
```sql
SELECT COUNT(DISTINCT C.CUSTOMER_ID) AS TOTAL_CUSTOMERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > 1000 AND EXTRACT(YEAR FROM O.ORDER_DATE) = 2021
AND C.CUSTOMER_ID NOT IN (
    SELECT DISTINCT O2.CUSTOMER_ID
    FROM Orders O2
    WHERE EXTRACT(YEAR FROM O2.ORDER_DATE) = 2022
);
```

---

### 41. Show customers who placed at least 2 orders in the last 12 months and display their total amount spent.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE >= ADD_MONTHS(SYSDATE, -12)
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(O.ORDER_ID) >= 2;
```

---

### 42. Retrieve customers who placed orders with a total value higher than the median order amount of all customers.
```sql
WITH MedianOrder AS (
    SELECT AVG(ORDER_AMOUNT) AS MEDIAN_ORDER_AMOUNT
    FROM (
        SELECT ORDER_AMOUNT
        FROM Orders
        ORDER BY ORDER_AMOUNT
        OFFSET (SELECT COUNT(*) FROM Orders) / 2 ROWS
        FETCH NEXT 1 ROW ONLY
    )
),
CustomerTotals AS (
    SELECT C.CUSTOMER_ID, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
    FROM Customers C
    JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    GROUP BY C.CUSTOMER_ID
)
SELECT C.CUSTOMER_ID
FROM CustomerTotals C
WHERE C.TOTAL_ORDER_AMOUNT > (SELECT MEDIAN_ORDER_AMOUNT FROM MedianOrder);
```

---

### 43. List customers whose order amount is within 20% of the highest order placed this year.
```sql
WITH MaxOrder AS (
    SELECT MAX(ORDER_AMOUNT) AS MAX_ORDER_AMOUNT
    FROM Orders
    WHERE EXTRACT(YEAR FROM ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE)
)
SELECT DISTINCT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT BETWEEN 0.8 * (SELECT MAX_ORDER_AMOUNT FROM MaxOrder) 
                         AND (SELECT MAX_ORDER_AMOUNT FROM MaxOrder);
```

---

### 44. Find customers who placed orders in exactly 3 different months of the current year.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(YEAR FROM O.ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE)
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(DISTINCT EXTRACT(MONTH FROM O.ORDER_DATE)) = 3;
```

---

### 45. Retrieve the last name and total order amount of customers who placed their first order before turning 25.
```sql
SELECT C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.ACCOUNT_CREATION_DATE <= ADD_MONTHS(TO_DATE(O.ORDER_DATE), -25 * 12)
GROUP BY C.LAST_NAME;
```

---

### 46. List all customers whose account creation date is in the current month and display their total order amount.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
LEFT JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(YEAR FROM C.ACCOUNT_CREATION_DATE) = EXTRACT(YEAR FROM SYSDATE)
AND EXTRACT(MONTH FROM C.ACCOUNT_CREATION_DATE) = EXTRACT(MONTH FROM SYSDATE)
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME;
```

---

### 47. Find customers who placed orders with a total value that is a multiple of 500.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
HAVING MOD(SUM(O.ORDER_AMOUNT), 500) = 0;
```

---

### 48. Retrieve the first and last order dates for each customer and calculate the difference between them.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME,
       MIN(O.ORDER_DATE) AS FIRST_ORDER_DATE,
       MAX(O.ORDER_DATE) AS LAST_ORDER_DATE,
       MAX(O.ORDER_DATE) - MIN(O.ORDER_DATE) AS ORDER_DATE_DIFFERENCE
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME;
```

---

### 49. Show customers who placed an order more than 2 years after their account creation date.
```sql
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE > ADD_MONTHS(C.ACCOUNT_CREATION_DATE, 24);
```

---

### 50. Find the customers who placed orders with an amount greater than the average of their previous 3 orders.
```sql
WITH PreviousOrders AS (
    SELECT CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT,
           ROW_NUMBER() OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_DATE DESC) AS RN
    FROM Orders
),
AvgPreviousOrders AS (
    SELECT CUSTOMER_ID, AVG(ORDER_AMOUNT) AS AVG_PREVIOUS_ORDER
    FROM PreviousOrders
    WHERE RN <= 3
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN AvgPreviousOrders A ON C.CUSTOMER_ID = A.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > A.AVG_PREVIOUS_ORDER;
```

---



