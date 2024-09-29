

**1. Retrieve the names of customers who have placed the most orders on the first day of each quarter for the past three years.**
```sql
WITH QuarterlyOrders AS (
    SELECT CUSTOMER_ID, COUNT(ORDER_ID) AS ORDER_COUNT
    FROM ORDERS
    WHERE ORDER_DATE IN (TO_DATE('01-JAN-' || EXTRACT(YEAR FROM SYSDATE), 'DD-MON-YYYY'),
                         TO_DATE('01-APR-' || EXTRACT(YEAR FROM SYSDATE), 'DD-MON-YYYY'),
                         TO_DATE('01-JUL-' || EXTRACT(YEAR FROM SYSDATE), 'DD-MON-YYYY'),
                         TO_DATE('01-OCT-' || EXTRACT(YEAR FROM SYSDATE), 'DD-MON-YYYY'))
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME
FROM Customers C
JOIN QuarterlyOrders Q ON C.CUSTOMER_ID = Q.CUSTOMER_ID
WHERE Q.ORDER_COUNT = (SELECT MAX(ORDER_COUNT) FROM QuarterlyOrders);
```

---

**2. Find the customers whose total order amount, over all their orders, is a perfect square and display their total order count.**
```sql
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME
HAVING MOD(SUM(O.ORDER_AMOUNT), 1) = 0 AND 
       MOD(SQRT(SUM(O.ORDER_AMOUNT)), 1) = 0;
```

---

**3. For each customer, find the month in which they placed the highest number of orders and show their total order amount for that month.**
```sql
WITH MonthlyOrders AS (
    SELECT CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH, COUNT(ORDER_ID) AS MONTHLY_COUNT, SUM(ORDER_AMOUNT) AS TOTAL_AMOUNT
    FROM ORDERS
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
)
SELECT CUSTOMER_ID, ORDER_MONTH, TOTAL_AMOUNT
FROM MonthlyOrders
WHERE (CUSTOMER_ID, MONTHLY_COUNT) IN (
    SELECT CUSTOMER_ID, MAX(MONTHLY_COUNT)
    FROM MonthlyOrders
    GROUP BY CUSTOMER_ID
);
```

---

**4. Identify customers whose total amount spent exceeds the total amount spent by 75% of all other customers and display their total orders.**
```sql
WITH CustomerTotals AS (
    SELECT CUSTOMER_ID, SUM(ORDER_AMOUNT) AS TOTAL_SPENT
    FROM Orders
    GROUP BY CUSTOMER_ID
), Threshold AS (
    SELECT PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY TOTAL_SPENT) AS SPENDING_THRESHOLD
    FROM CustomerTotals
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN CustomerTotals CT ON C.CUSTOMER_ID = CT.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE CT.TOTAL_SPENT > (SELECT SPENDING_THRESHOLD FROM Threshold)
GROUP BY C.CUSTOMER_NAME;
```

---

**5. Retrieve customers who have placed an order every single day in at least one month of the current year and list their total orders.**
```sql
WITH DailyOrders AS (
    SELECT CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH, COUNT(DISTINCT ORDER_DATE) AS DAYS_COUNT
    FROM Orders
    WHERE ORDER_DATE >= TRUNC(SYSDATE, 'YYYY')
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN DailyOrders D ON C.CUSTOMER_ID = D.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE D.DAYS_COUNT = 31 -- Assuming the month has 31 days
GROUP BY C.CUSTOMER_NAME;
```

---

**6. List customers who have never placed an order on the 15th day of any month and display their total orders and the first order date.**
```sql
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS, MIN(O.ORDER_DATE) AS FIRST_ORDER_DATE
FROM Customers C
LEFT JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME
HAVING SUM(CASE WHEN EXTRACT(DAY FROM O.ORDER_DATE) = 15 THEN 1 ELSE 0 END) = 0;
```

---

**7. Show customers who have a decreasing trend in their average order amount, i.e., their monthly average order amount has consistently decreased for the last three months.**
```sql
WITH MonthlyAverages AS (
    SELECT CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH, AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
    WHERE ORDER_DATE >= ADD_MONTHS(SYSDATE, -3)
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
)
SELECT CUSTOMER_ID
FROM MonthlyAverages
GROUP BY CUSTOMER_ID
HAVING COUNT(CASE WHEN AVG_ORDER_AMOUNT < LAG(AVG_ORDER_AMOUNT, 1) OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_MONTH) THEN 1 END) = 2;
```

---

**8. Find customers whose orders contain at least one instance of an order amount that is exactly the mean of all their other orders and list their total orders.**
```sql
WITH CustomerOrders AS (
    SELECT CUSTOMER_ID, ORDER_AMOUNT, AVG(ORDER_AMOUNT) OVER (PARTITION BY CUSTOMER_ID) AS AVG_AMOUNT
    FROM Orders
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN CustomerOrders CO ON C.CUSTOMER_ID = CO.CUSTOMER_ID
WHERE CO.ORDER_AMOUNT = CO.AVG_AMOUNT
GROUP BY C.CUSTOMER_NAME;
```

---

**9. Retrieve customers who have placed the largest and the smallest orders in the same month and show their total amount spent that month.**
```sql
WITH MonthlyOrders AS (
    SELECT CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH, MAX(ORDER_AMOUNT) AS MAX_AMOUNT, MIN(ORDER_AMOUNT) AS MIN_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
    HAVING MAX(ORDER_AMOUNT) <> MIN(ORDER_AMOUNT)
)
SELECT C.CUSTOMER_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN MonthlyOrders MO ON C.CUSTOMER_ID = MO.CUSTOMER_ID
WHERE EXTRACT(MONTH FROM O.ORDER_DATE) = MO.ORDER_MONTH
GROUP BY C.CUSTOMER_NAME;
```

---

**10. List customers whose total amount spent in the last 30 days is at least 150% of their average order amount over their entire order history.**
```sql
WITH Last30Days AS (
    SELECT CUSTOMER_ID, SUM(ORDER_AMOUNT) AS LAST_30_DAYS_TOTAL
    FROM Orders
    WHERE ORDER_DATE >= SYSDATE - 30
    GROUP BY CUSTOMER_ID
), AverageOrders AS (
    SELECT CUSTOMER_ID, AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME
FROM Customers C
JOIN Last30Days L ON C.CUSTOMER_ID = L.CUSTOMER_ID
JOIN AverageOrders A ON C.CUSTOMER_ID = A.CUSTOMER_ID
WHERE L.LAST_30_DAYS_TOTAL >= A.AVG_ORDER_AMOUNT * 1.5;
```

---

**11. Identify customers who placed orders in all 12 months of the previous year and show their total order amount for that year.**
```sql
WITH MonthlyOrders AS (
    SELECT CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH
    FROM Orders
    WHERE ORDER_DATE >= TRUNC(ADD_MONTHS(SYSDATE, -12), 'YYYY')
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
)
SELECT C.CUSTOMER_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.CUSTOMER_ID IN (SELECT CUSTOMER_ID FROM MonthlyOrders GROUP BY CUSTOMER_ID HAVING COUNT(DISTINCT ORDER_MONTH) = 12)
GROUP BY C.CUSTOMER_NAME;
```

---

**12. Find customers whose first order amount is greater than the average order amount of all customers but whose last order amount is less than the average.**
```sql
WITH OrderStats AS (
    SELECT CUSTOMER_ID, 
           MIN(ORDER_AMOUNT) AS FIRST_ORDER_AMOUNT, 
           MAX(ORDER_AMOUNT) AS LAST_ORDER_AMOUNT,
           AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME
FROM Customers C
JOIN OrderStats O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.FIRST_ORDER_AMOUNT > (SELECT AVG(ORDER_AMOUNT) FROM Orders) 
  AND O.LAST_ORDER_AMOUNT < (SELECT AVG(ORDER_AMOUNT) FROM Orders);
```

---

**13. Retrieve customers who have a standard deviation of their order amounts greater than the average of all customers and display their total orders.**
```sql
WITH CustomerStats AS (
    SELECT

 CUSTOMER_ID, 
           STDDEV(ORDER_AMOUNT) AS ORDER_STDDEV
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN CustomerStats CS ON C.CUSTOMER_ID = CS.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE CS.ORDER_STDDEV > (SELECT AVG(STDDEV(ORDER_AMOUNT)) FROM (SELECT CUSTOMER_ID, STDDEV(ORDER_AMOUNT) AS ORDER_STDDEV FROM Orders GROUP BY CUSTOMER_ID));
GROUP BY C.CUSTOMER_NAME;
```

---

**14. List customers who have not placed any orders in the last year but have a birthday within the next 30 days.**
```sql
SELECT C.CUSTOMER_NAME
FROM Customers C
WHERE NOT EXISTS (
    SELECT 1
    FROM Orders O
    WHERE O.CUSTOMER_ID = C.CUSTOMER_ID 
    AND O.ORDER_DATE >= ADD_MONTHS(SYSDATE, -12)
)
AND C.BIRTH_DATE BETWEEN SYSDATE AND ADD_MONTHS(SYSDATE, 1);
```

---

**15. Show the total number of orders placed by customers who only place orders on weekdays and their average order value.**
```sql
WITH WeekdayOrders AS (
    SELECT CUSTOMER_ID, COUNT(ORDER_ID) AS TOTAL_ORDERS, AVG(ORDER_AMOUNT) AS AVG_ORDER_VALUE
    FROM Orders
    WHERE TO_CHAR(ORDER_DATE, 'DY', 'NLS_DATE_LANGUAGE=ENGLISH') NOT IN ('SAT', 'SUN')
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, SUM(W.TOTAL_ORDERS) AS TOTAL_ORDERS, AVG(W.AVG_ORDER_VALUE) AS AVG_ORDER_VALUE
FROM Customers C
JOIN WeekdayOrders W ON C.CUSTOMER_ID = W.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME;
```

---

**16. Retrieve customers whose orders contain an order amount that is the median of all their orders and display their total orders.**
```sql
WITH MedianOrders AS (
    SELECT CUSTOMER_ID, 
           ORDER_AMOUNT,
           ROW_NUMBER() OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_AMOUNT) AS RN,
           COUNT(*) OVER (PARTITION BY CUSTOMER_ID) AS CNT
    FROM Orders
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN MedianOrders M ON C.CUSTOMER_ID = M.CUSTOMER_ID
WHERE M.ORDER_AMOUNT = (
    SELECT AVG(ORDER_AMOUNT)
    FROM (
        SELECT ORDER_AMOUNT
        FROM MedianOrders
        WHERE CUSTOMER_ID = M.CUSTOMER_ID
        ORDER BY ORDER_AMOUNT
        OFFSET (CNT - 1) / 2 ROWS FETCH NEXT 2 ROWS ONLY
    )
)
GROUP BY C.CUSTOMER_NAME;
```

---

**17. Find customers who have a pattern of placing orders on the last day of each month for the last year and show their total order amounts.**
```sql
WITH LastDayOrders AS (
    SELECT CUSTOMER_ID, COUNT(*) AS ORDER_COUNT
    FROM Orders
    WHERE ORDER_DATE = LAST_DAY(ORDER_DATE)
    AND ORDER_DATE >= ADD_MONTHS(SYSDATE, -12)
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_AMOUNT
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN LastDayOrders L ON C.CUSTOMER_ID = L.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME
HAVING L.ORDER_COUNT >= 12;
```

---

**18. List customers who have spent more on average in the first half of the year compared to the second half and display their total orders.**
```sql
WITH HalfYearTotals AS (
    SELECT CUSTOMER_ID, 
           SUM(CASE WHEN EXTRACT(MONTH FROM ORDER_DATE) <= 6 THEN ORDER_AMOUNT ELSE 0 END) AS FIRST_HALF,
           SUM(CASE WHEN EXTRACT(MONTH FROM ORDER_DATE) > 6 THEN ORDER_AMOUNT ELSE 0 END) AS SECOND_HALF
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN HalfYearTotals H ON C.CUSTOMER_ID = H.CUSTOMER_ID
WHERE H.FIRST_HALF / 6 > H.SECOND_HALF / 6
GROUP BY C.CUSTOMER_NAME;
```

---

**19. Identify customers who placed an order in every week of the year and show the total order amount for that year.**
```sql
WITH WeeklyOrders AS (
    SELECT CUSTOMER_ID, 
           COUNT(DISTINCT TRUNC(ORDER_DATE, 'IW')) AS WEEKS_COUNT,
           SUM(ORDER_AMOUNT) AS TOTAL_AMOUNT
    FROM Orders
    WHERE ORDER_DATE >= TRUNC(ADD_MONTHS(SYSDATE, -12), 'YYYY')
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, W.TOTAL_AMOUNT
FROM Customers C
JOIN WeeklyOrders W ON C.CUSTOMER_ID = W.CUSTOMER_ID
WHERE W.WEEKS_COUNT = 52; -- Assuming a non-leap year
```

---

**20. Retrieve customers who have a positive correlation between the number of orders placed and the order amounts over the past year.**
```sql
WITH OrderCorrelation AS (
    SELECT CUSTOMER_ID, 
           COUNT(ORDER_ID) AS TOTAL_ORDERS, 
           SUM(ORDER_AMOUNT) AS TOTAL_AMOUNT,
           AVG(ORDER_AMOUNT) AS AVG_AMOUNT
    FROM Orders
    WHERE ORDER_DATE >= ADD_MONTHS(SYSDATE, -12)
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME
FROM Customers C
JOIN OrderCorrelation O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.TOTAL_ORDERS > 0 AND O.TOTAL_AMOUNT > 0
HAVING (O.AVG_AMOUNT / O.TOTAL_ORDERS) > 0;
```

---

**21. Find customers whose orders are always placed at a specific time of day (e.g., between 6 PM and 8 PM) and show their total order amounts.**
```sql
WITH EveningOrders AS (
    SELECT CUSTOMER_ID, SUM(ORDER_AMOUNT) AS TOTAL_AMOUNT
    FROM Orders
    WHERE TO_CHAR(ORDER_DATE, 'HH24') BETWEEN '18' AND '20'
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, E.TOTAL_AMOUNT
FROM Customers C
JOIN EveningOrders E ON C.CUSTOMER_ID = E.CUSTOMER_ID;
```

---

**22. List customers who have placed orders on holidays and display their total orders and the average order amount on those days.**
```sql
WITH HolidayOrders AS (
    SELECT CUSTOMER_ID, COUNT(ORDER_ID) AS TOTAL_ORDERS, AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
    WHERE ORDER_DATE IN (SELECT HOLIDAY_DATE FROM Holidays)
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, H.TOTAL_ORDERS, H.AVG_ORDER_AMOUNT
FROM Customers C
JOIN HolidayOrders H ON C.CUSTOMER_ID = H.CUSTOMER_ID;
```

---

**23. Identify customers who have placed orders with an amount that is the Fibonacci sequence for the last six months and list their total orders.**
```sql
WITH FibonacciAmounts AS (
    SELECT CUSTOMER_ID, COUNT(ORDER_ID) AS TOTAL_ORDERS
    FROM Orders
    WHERE ORDER_AMOUNT IN (0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597) -- Example Fibonacci sequence
    AND ORDER_DATE >= ADD_MONTHS(SYSDATE, -6)
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, F.TOTAL_ORDERS
FROM Customers C
JOIN FibonacciAmounts F ON C.CUSTOMER_ID = F.CUSTOMER_ID;
```

---

**24. Retrieve customers who have a consistent increase in order amount every month for six consecutive months and display their total orders.**
```sql
WITH MonthlyIncreases AS (
    SELECT CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH, SUM(ORDER_AMOUNT) AS MONTHLY_TOTAL
    FROM Orders
    WHERE ORDER_DATE >= ADD_MONTHS(SYSDATE, -6)
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
),
TrendAnalysis AS (
    SELECT CUSTOMER_ID,
           COUNT(*) AS INCREASING_MONTHS
    FROM (
        SELECT CUSTOMER_ID, ORDER_MONTH, 
               MONTHLY_TOTAL,
               LAG(MONTHLY_TOTAL) OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_MONTH) AS PREV_MONTH_TOTAL
        FROM MonthlyIncreases
    )
    WHERE MONTHLY_TOTAL > PREV_MONTH_TOTAL
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN TrendAnalysis T ON C.CUSTOMER_ID = T.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE T.INCREASING_MONTHS = 6
GROUP BY C.CUSTOMER_NAME;
```

---

**25. Find customers whose total order amount is a palindrome and show their total order count.**
```sql
WITH PalindromeOrders AS (
    SELECT CUSTOMER_ID, SUM(ORDER_AMOUNT) AS TOTAL_AMOUNT
    FROM Orders
    GROUP BY

 CUSTOMER_ID
    HAVING TOTAL_AMOUNT = TO_NUMBER(REVERSE(TO_CHAR(TOTAL_AMOUNT)))
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN PalindromeOrders P ON C.CUSTOMER_ID = P.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME;
```

---


Feel free to adjust any queries as needed for your specific data structures or requirements!
