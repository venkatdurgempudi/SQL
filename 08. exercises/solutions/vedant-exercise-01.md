### **Solutions**

---

**1. Retrieve the total amount spent by each customer, their first name, and city.**

```sql
SELECT C.FIRST_NAME, C.CITY, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.CITY;
```

---

**2. Find customers who placed an order after January 1, 2021, and calculate the average amount spent per order.**

```sql
SELECT C.FIRST_NAME, AVG(O.ORDER_AMOUNT) AS AVG_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE > TO_DATE('01-JAN-2021', 'DD-MON-YYYY')
GROUP BY C.FIRST_NAME;
```

---

**3. Retrieve all customers from New York who have placed more than 2 orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.CITY = 'New York'
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(O.ORDER_ID) > 2;
```

---

**4. Show the first and last names of customers who placed an order between 500 and 2000.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT BETWEEN 500 AND 2000;
```

---

**5. Find the maximum order amount placed by customers in each city.**

```sql
SELECT C.CITY, MAX(O.ORDER_AMOUNT) AS MAX_ORDER
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CITY;
```

---

**6. Retrieve customers who placed orders in February 2021 and order them by order amount in descending order.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE BETWEEN TO_DATE('01-FEB-2021', 'DD-MON-YYYY') 
AND TO_DATE('28-FEB-2021', 'DD-MON-YYYY')
ORDER BY O.ORDER_AMOUNT DESC;
```

---

**7. List customers whose email contains 'example.com' and calculate the total number of such customers.**

```sql
SELECT COUNT(*) AS TOTAL_CUSTOMERS
FROM CUSTOMERS
WHERE INSTR(EMAIL, 'example.com') > 0;
```

---

**8. Retrieve all customers who have placed an order whose amount is more than the average order amount of all customers.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > (SELECT AVG(ORDER_AMOUNT) FROM ORDERS);
```

---

**9. Find customers who have not placed any orders since January 1, 2021.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
WHERE C.CUSTOMER_ID NOT IN (
    SELECT O.CUSTOMER_ID 
    FROM ORDERS O
    WHERE O.ORDER_DATE > TO_DATE('01-JAN-2021', 'DD-MON-YYYY')
);
```

---

**10. Retrieve the total number of customers from each city, and the sum of their order amounts.**

```sql
SELECT C.CITY, COUNT(C.CUSTOMER_ID) AS TOTAL_CUSTOMERS, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CITY;
```

---

**11. Calculate the number of days since each customer’s account creation date and order the results by customer ID.**

```sql
SELECT CUSTOMER_ID, FIRST_NAME, SYSDATE - ACCOUNT_CREATION_DATE AS DAYS_SINCE_CREATION
FROM CUSTOMERS
ORDER BY CUSTOMER_ID;
```

---

**12. Retrieve the first order placed by each customer and display their name, order date, and amount.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, MIN(O.ORDER_DATE) AS FIRST_ORDER_DATE, O.ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT;
```

---

**13. Show the total number of customers who placed orders in each month of 2021.**

```sql
SELECT TO_CHAR(O.ORDER_DATE, 'MONTH') AS ORDER_MONTH, COUNT(DISTINCT C.CUSTOMER_ID) AS TOTAL_CUSTOMERS
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE TO_CHAR(O.ORDER_DATE, 'YYYY') = '2021'
GROUP BY TO_CHAR(O.ORDER_DATE, 'MONTH');
```

---

**14. Find customers who placed an order with an amount greater than the highest order placed in New York.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > (
    SELECT MAX(O.ORDER_AMOUNT)
    FROM CUSTOMERS C
    JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    WHERE C.CITY = 'New York'
);
```

---

**15. List all customers who placed at least one order in March 2022.**

```sql
SELECT DISTINCT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE BETWEEN TO_DATE('01-MAR-2022', 'DD-MON-YYYY') 
AND TO_DATE('31-MAR-2022', 'DD-MON-YYYY');
```

---

**16. Retrieve the total order amount, rounded to 2 decimal places, for each customer whose last name starts with 'S'.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, ROUND(SUM(O.ORDER_AMOUNT), 2) AS TOTAL_ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.LAST_NAME LIKE 'S%'
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**17. Find the difference between the highest and lowest order amounts for customers in Los Angeles.**

```sql
SELECT MAX(O.ORDER_AMOUNT) - MIN(O.ORDER_AMOUNT) AS AMOUNT_DIFFERENCE
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.CITY = 'Los Angeles';
```

---

**18. Retrieve the last order date for each customer and the total amount they spent.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, MAX(O.ORDER_DATE) AS LAST_ORDER_DATE, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT_SPENT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**19. Retrieve all customers whose first name starts with 'A' and who placed an order above 1000 in the last 6 months.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.FIRST_NAME LIKE 'A%' 
AND O.ORDER_AMOUNT > 1000 
AND O.ORDER_DATE >= ADD_MONTHS(SYSDATE, -6);
```

---

**20. Show the total amount of orders placed by customers from each city, grouped by city and sorted by total amount.**

```sql
SELECT C.CITY, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CITY
ORDER BY TOTAL_AMOUNT DESC;
```

---

**21. Retrieve all customers whose account creation date is earlier than the current year and who have placed orders this year.**

```sql
SELECT DISTINCT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(YEAR FROM C.ACCOUNT_CREATION_DATE) < EXTRACT(YEAR FROM SYSDATE)
AND EXTRACT(YEAR FROM O.ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE);
```

---

**22. Find customers who placed orders totaling more than 3000 but have not placed an order in the last 6 months.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING SUM(O.ORDER_AMOUNT) > 3000
AND MAX(O.ORDER_DATE) < ADD_MONTHS(SYSDATE, -6);
```

---

**23. Retrieve the total amount of orders placed on each day of January 2021.**

```sql
SELECT O.ORDER

_DATE, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM ORDERS O
WHERE O.ORDER_DATE BETWEEN TO_DATE('01-JAN-2021', 'DD-MON-YYYY') AND TO_DATE('31-JAN-2021', 'DD-MON-YYYY')
GROUP BY O.ORDER_DATE
ORDER BY O.ORDER_DATE;
```

---

**24. Find the highest order amount placed by customers from Chicago and retrieve the name of the customer who placed it.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT = (
    SELECT MAX(O.ORDER_AMOUNT)
    FROM CUSTOMERS C
    JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    WHERE C.CITY = 'Chicago'
);
```

---

**25. Retrieve the average order amount for all customers and show customers who placed orders above that average.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > (SELECT AVG(O.ORDER_AMOUNT) FROM ORDERS);
```

---

**26. Find customers whose first and last order amounts are the same.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN (SELECT CUSTOMER_ID, 
             MIN(ORDER_AMOUNT) AS FIRST_ORDER_AMOUNT, 
             MAX(ORDER_AMOUNT) AS LAST_ORDER_AMOUNT
      FROM ORDERS
      GROUP BY CUSTOMER_ID) ORDER_STATS
ON C.CUSTOMER_ID = ORDER_STATS.CUSTOMER_ID
WHERE ORDER_STATS.FIRST_ORDER_AMOUNT = ORDER_STATS.LAST_ORDER_AMOUNT;
```

---

**27. Retrieve customers who placed more than 3 orders in 2021 and calculate the total amount spent by them.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(YEAR FROM O.ORDER_DATE) = 2021
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(O.ORDER_ID) > 3;
```

---

**28. Find customers who placed orders in the last 6 months and their average order value is higher than the overall average.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, AVG(O.ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE >= ADD_MONTHS(SYSDATE, -6)
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING AVG(O.ORDER_AMOUNT) > (SELECT AVG(ORDER_AMOUNT) FROM ORDERS);
```

---

**29. Retrieve the difference in days between the first and last orders of customers who placed more than 5 orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, 
       MAX(O.ORDER_DATE) - MIN(O.ORDER_DATE) AS DAYS_DIFFERENCE
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(O.ORDER_ID) > 5;
```

---

**30. Show customers who placed orders only on weekends and display their total order amount.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE TO_CHAR(O.ORDER_DATE, 'DY') IN ('SAT', 'SUN')
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**31. Find customers who placed orders where the order amount is a prime number and display the count of such orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS PRIME_ORDER_COUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE MOD(O.ORDER_AMOUNT, 2) <> 0 AND MOD(O.ORDER_AMOUNT, 3) <> 0
AND MOD(O.ORDER_AMOUNT, 5) <> 0 AND MOD(O.ORDER_AMOUNT, 7) <> 0
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**32. Retrieve customers who have not placed an order on their birthday month and list their total orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM CUSTOMERS C
LEFT JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
AND TO_CHAR(C.BIRTH_DATE, 'MM') <> TO_CHAR(O.ORDER_DATE, 'MM')
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**33. List customers who have placed orders in every quarter of the current year.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(YEAR FROM O.ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE)
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(DISTINCT TO_CHAR(O.ORDER_DATE, 'Q')) = 4;
```

---

**34. Find the top 3 customers with the highest average order value and show their total amount spent.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, AVG(O.ORDER_AMOUNT) AS AVG_ORDER_VALUE, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
ORDER BY AVG_ORDER_VALUE DESC
FETCH FIRST 3 ROWS ONLY;
```

---

**35. Retrieve customers who have placed orders whose standard deviation in amount is greater than 1000.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING STDDEV(O.ORDER_AMOUNT) > 1000;
```

---

**36. Find customers who placed their first order after 2 years of account creation date.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, MIN(O.ORDER_DATE) AS FIRST_ORDER_DATE
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME, C.ACCOUNT_CREATION_DATE
HAVING MIN(O.ORDER_DATE) > ADD_MONTHS(C.ACCOUNT_CREATION_DATE, 24);
```

---

**37. Show customers who placed orders with a cumulative total that matches exactly 3000.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS CUMULATIVE_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING SUM(O.ORDER_AMOUNT) = 3000;
```

---

**38. Retrieve customers who have placed an order every month in the current year and display their total orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(YEAR FROM O.ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE)
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(DISTINCT EXTRACT(MONTH FROM O.ORDER_DATE)) = 12;
```

---

**39. Find the first and last order dates for each customer and calculate the number of orders placed between these dates.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, MIN(O.ORDER_DATE) AS FIRST_ORDER, 
       MAX(O.ORDER_DATE) AS LAST_ORDER, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**40. List customers who placed at least one order in January, March, and July of the same year.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(MONTH FROM O.ORDER_DATE) IN (1, 3, 7)
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(DISTINCT EXTRACT(MONTH FROM O.ORDER_DATE)) = 3;
```

---

**41. Show customers who placed at least 2 orders in the last 12 months and display their total amount spent.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE >= ADD_MONTHS(SYSDATE, -12)
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(O.ORDER_ID) >= 2;
```

---


**42. Find customers whose orders have an average order amount greater than 2000 and have placed at least one order in December.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, AVG(O.ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING AVG(O.ORDER_AMOUNT) > 2000
AND COUNT(CASE WHEN EXTRACT(MONTH FROM O.ORDER_DATE) = 12 THEN 1 END) > 0;
```

---

**43. List customers whose highest and lowest order amounts differ by more than 5000.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, MAX(O.ORDER_AMOUNT) - MIN(O.ORDER_AMOUNT) AS ORDER_DIFF
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING MAX(O.ORDER_AMOUNT) - MIN(O.ORDER_AMOUNT) > 5000;
```

---

**44. Retrieve customers whose total order amount in the current year is at least double their total order amount from the previous year.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING SUM(CASE WHEN EXTRACT(YEAR FROM O.ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE) THEN O.ORDER_AMOUNT END) >=
       2 * SUM(CASE WHEN EXTRACT(YEAR FROM O.ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE) - 1 THEN O.ORDER_AMOUNT END);
```

---

**45. Find customers who placed their first order within 30 days of account creation and list their total orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME, C.ACCOUNT_CREATION_DATE
HAVING MIN(O.ORDER_DATE) <= C.ACCOUNT_CREATION_DATE + 30;
```

---

**46. List customers who placed orders only in the second half of the year and show their total amount spent.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(MONTH FROM O.ORDER_DATE) >= 7
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**47. Show customers who placed their orders on the last day of the month and the total number of such orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_DATE = LAST_DAY(O.ORDER_DATE)
GROUP BY C.FIRST_NAME, C.LAST_NAME;
```

---

**48. Retrieve customers whose orders have a total amount greater than 5000, but none of their individual orders exceed 2000.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING SUM(O.ORDER_AMOUNT) > 5000
AND MAX(O.ORDER_AMOUNT) <= 2000;
```

---

**49. List customers who placed orders in exactly 5 different months in the current year and display their total amount spent.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE EXTRACT(YEAR FROM O.ORDER_DATE) = EXTRACT(YEAR FROM SYSDATE)
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING COUNT(DISTINCT EXTRACT(MONTH FROM O.ORDER_DATE)) = 5;
```

---

**50. Find customers who placed orders whose order amounts are always within the range of 1000 to 3000 and display their total orders.**

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM CUSTOMERS C
JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING MIN(O.ORDER_AMOUNT) >= 1000 AND MAX(O.ORDER_AMOUNT) <= 3000;
```

---
