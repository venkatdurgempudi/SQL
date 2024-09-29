

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

### 26. Show customers who have placed more than one order on the same day in the same month for the last three years and display their total orders.
```sql
WITH DailyOrderCounts AS (
    SELECT CUSTOMER_ID, 
           TO_CHAR(ORDER_DATE, 'DD-MM') AS ORDER_DAY_MONTH, 
           COUNT(ORDER_ID) AS ORDER_COUNT
    FROM Orders
    WHERE ORDER_DATE >= ADD_MONTHS(SYSDATE, -36)  -- Last 3 years
    GROUP BY CUSTOMER_ID, TO_CHAR(ORDER_DATE, 'DD-MM')
    HAVING COUNT(ORDER_ID) > 1
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN DailyOrderCounts D ON C.CUSTOMER_ID = D.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME;
```

---

### 27. Identify customers who have orders with an amount that is a power of two and list their total orders.
```sql
WITH PowerOfTwoOrders AS (
    SELECT CUSTOMER_ID, COUNT(ORDER_ID) AS TOTAL_ORDERS
    FROM Orders
    WHERE ORDER_AMOUNT > 0 AND (ORDER_AMOUNT & (ORDER_AMOUNT - 1)) = 0  -- Power of two condition
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, P.TOTAL_ORDERS
FROM Customers C
JOIN PowerOfTwoOrders P ON C.CUSTOMER_ID = P.CUSTOMER_ID;
```

---

### 28. Retrieve customers who have spent more than 2000 in one order but have an average order amount of less than 500 overall.
```sql
WITH CustomerStats AS (
    SELECT CUSTOMER_ID, 
           AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT,
           MAX(ORDER_AMOUNT) AS MAX_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME
FROM Customers C
JOIN CustomerStats S ON C.CUSTOMER_ID = S.CUSTOMER_ID
WHERE S.MAX_ORDER_AMOUNT > 2000 AND S.AVG_ORDER_AMOUNT < 500;
```

---

### 29. List customers who placed the highest order on the last day of the year and show their total order amount for that year.
```sql
WITH LastDayOrders AS (
    SELECT CUSTOMER_ID, 
           SUM(ORDER_AMOUNT) AS TOTAL_YEAR_AMOUNT
    FROM Orders
    WHERE TO_CHAR(ORDER_DATE, 'MM-DD') = '12-31'  -- Last day of the year
    GROUP BY CUSTOMER_ID
),
HighestOrder AS (
    SELECT CUSTOMER_ID, MAX(ORDER_AMOUNT) AS MAX_ORDER
    FROM Orders
    WHERE TO_CHAR(ORDER_DATE, 'MM-DD') = '12-31'
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, L.TOTAL_YEAR_AMOUNT
FROM Customers C
JOIN LastDayOrders L ON C.CUSTOMER_ID = L.CUSTOMER_ID
JOIN HighestOrder H ON C.CUSTOMER_ID = H.CUSTOMER_ID
WHERE H.MAX_ORDER = (SELECT MAX(MAX_ORDER) FROM HighestOrder);
```

---

### 30. Find customers whose total amount spent in the last month is greater than the total amount spent in the previous month and display their total orders.
```sql
WITH MonthlySpending AS (
    SELECT CUSTOMER_ID, 
           SUM(CASE WHEN ORDER_DATE >= TRUNC(SYSDATE, 'MM') THEN ORDER_AMOUNT ELSE 0 END) AS LAST_MONTH,
           SUM(CASE WHEN ORDER_DATE >= ADD_MONTHS(TRUNC(SYSDATE, 'MM'), -1) AND ORDER_DATE < TRUNC(SYSDATE, 'MM') THEN ORDER_AMOUNT ELSE 0 END) AS PREVIOUS_MONTH
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN MonthlySpending M ON C.CUSTOMER_ID = M.CUSTOMER_ID
WHERE M.LAST_MONTH > M.PREVIOUS_MONTH
GROUP BY C.CUSTOMER_NAME;
```

---

### 31. Retrieve customers who placed at least one order during each month of a specific quarter and show their total order amount.
```sql
WITH QuarterOrders AS (
    SELECT CUSTOMER_ID, 
           EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH,
           SUM(ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
    FROM Orders
    WHERE ORDER_DATE >= TO_DATE('01-JAN-2024', 'DD-MON-YYYY') AND ORDER_DATE < TO_DATE('01-APR-2024', 'DD-MON-YYYY')  -- Adjust quarter as needed
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
)
SELECT C.CUSTOMER_NAME, SUM(Q.TOTAL_ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN QuarterOrders Q ON C.CUSTOMER_ID = Q.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME
HAVING COUNT(DISTINCT Q.ORDER_MONTH) = 3;  -- Ensures orders in all three months of the quarter
```

---

### 32. Identify customers whose orders show a cyclic pattern of spending (e.g., high-low-high-low) over a period of time and display their total orders.
```sql
WITH OrderPatterns AS (
    SELECT CUSTOMER_ID, 
           ORDER_DATE, 
           ORDER_AMOUNT,
           LAG(ORDER_AMOUNT, 1) OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_DATE) AS PREV_ORDER_AMOUNT,
           LAG(ORDER_AMOUNT, 2) OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_DATE) AS PREV2_ORDER_AMOUNT
    FROM Orders
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN OrderPatterns P ON C.CUSTOMER_ID = P.CUSTOMER_ID
WHERE (P.ORDER_AMOUNT > P.PREV_ORDER_AMOUNT AND P.PREV_ORDER_AMOUNT < P.PREV2_ORDER_AMOUNT)
   OR (P.ORDER_AMOUNT < P.PREV_ORDER_AMOUNT AND P.PREV_ORDER_AMOUNT > P.PREV2_ORDER_AMOUNT)
GROUP BY C.CUSTOMER_NAME;
```

---

### 33. Show customers who have a higher order count but lower order amounts compared to their peers and display their average order amount.
```sql
WITH CustomerStats AS (
    SELECT CUSTOMER_ID, 
           COUNT(ORDER_ID) AS ORDER_COUNT, 
           SUM(ORDER_AMOUNT) AS TOTAL_AMOUNT,
           AVG(ORDER_AMOUNT) AS AVG_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID
),
PeerStats AS (
    SELECT AVG(AVG_AMOUNT) AS AVG_PEER_AMOUNT
    FROM CustomerStats
)
SELECT C.CUSTOMER_NAME, S.ORDER_COUNT, S.AVG_AMOUNT
FROM Customers C
JOIN CustomerStats S ON C.CUSTOMER_ID = S.CUSTOMER_ID
JOIN PeerStats P ON S.AVG_AMOUNT < P.AVG_PEER_AMOUNT
WHERE S.ORDER_COUNT > (SELECT AVG(ORDER_COUNT) FROM CustomerStats);
```

---

### 34. Find customers who placed orders on the same date every year for three consecutive years and display their total orders.
```sql
WITH YearlyOrders AS (
    SELECT CUSTOMER_ID, 
           TO_CHAR(ORDER_DATE, 'MM-DD') AS ORDER_DATE,
           COUNT(DISTINCT EXTRACT(YEAR FROM ORDER_DATE)) AS DISTINCT_YEARS
    FROM Orders
    WHERE ORDER_DATE >= ADD_MONTHS(SYSDATE, -36)  -- Last 3 years
    GROUP BY CUSTOMER_ID, TO_CHAR(ORDER_DATE, 'MM-DD')
)
SELECT C.CUSTOMER_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN YearlyOrders Y ON C.CUSTOMER_ID = Y.CUSTOMER_ID
WHERE Y.DISTINCT_YEARS = 3
GROUP BY C.CUSTOMER_NAME;
```

---

### 35. List customers whose orders have an amount that is a significant outlier compared to the average order amounts of their peers.
```sql
WITH CustomerStats AS (
    SELECT CUSTOMER_ID, 
           AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT,
           STDDEV(ORDER_AMOUNT) AS STDDEV_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID
),
OutlierCustomers AS (
    SELECT CUSTOMER_ID, AVG_ORDER_AMOUNT
    FROM CustomerStats
    WHERE AVG_ORDER_AMOUNT > (SELECT AVG(AVG_ORDER_AMOUNT) + 2 * AVG(STDDEV_ORDER_AMOUNT) FROM CustomerStats)  -- Assuming outlier is > mean + 2*stddev
)
SELECT C.CUSTOMER_NAME
FROM Customers C
JOIN OutlierCustomers O ON C.CUSTOMER_ID = O.CUSTOMER_ID;
```

---

### 36. Retrieve customers whose order history reflects a seasonal trend (e.g., higher in December) and show their total order amounts for those seasons.
```sql
WITH SeasonalOrders AS (
    SELECT CUSTOMER_ID, 
           EXTRACT(MONTH FROM ORDER_DATE) AS ORDER_MONTH, 
           SUM(ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID, EXTRACT(MONTH FROM ORDER_DATE)
)
SELECT C.CUSTOMER_NAME, SUM(S.TOTAL_ORDER_AMOUNT) AS TOTAL_SEASONAL_AMOUNT
FROM Customers C
JOIN SeasonalOrders S ON C.CUSTOMER_ID = S.CUSTOMER_ID
WHERE S.ORDER_MONTH IN (12)  -- December as an example of a season
GROUP BY C.CUSTOMER_NAME
HAVING SUM(S.TOTAL_ORDER_AMOUNT) > (SELECT AVG(TOTAL_ORDER

_AMOUNT) FROM SeasonalOrders WHERE ORDER_MONTH IN (12));
```

---

### 37. Identify customers who have made repeat purchases of the same product multiple times and display their total spending on those products.
```sql
WITH RepeatPurchases AS (
    SELECT CUSTOMER_ID, PRODUCT_ID, 
           COUNT(ORDER_ID) AS PURCHASE_COUNT, 
           SUM(ORDER_AMOUNT) AS TOTAL_SPENDING
    FROM Orders
    GROUP BY CUSTOMER_ID, PRODUCT_ID
    HAVING COUNT(ORDER_ID) > 1
)
SELECT C.CUSTOMER_NAME, RP.PRODUCT_ID, RP.TOTAL_SPENDING
FROM Customers C
JOIN RepeatPurchases RP ON C.CUSTOMER_ID = RP.CUSTOMER_ID;
```

---

### 38. Find customers whose average order value increases significantly (by 25% or more) after a specific marketing campaign and show their total orders.
```sql
WITH PreCampaign AS (
    SELECT CUSTOMER_ID, AVG(ORDER_AMOUNT) AS AVG_BEFORE
    FROM Orders
    WHERE ORDER_DATE < TO_DATE('01-JAN-2024', 'DD-MON-YYYY')  -- Adjust to campaign start date
    GROUP BY CUSTOMER_ID
),
PostCampaign AS (
    SELECT CUSTOMER_ID, AVG(ORDER_AMOUNT) AS AVG_AFTER
    FROM Orders
    WHERE ORDER_DATE >= TO_DATE('01-JAN-2024', 'DD-MON-YYYY')  -- Adjust to campaign start date
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, P.AVG_BEFORE, P.AVG_AFTER, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN PreCampaign P ON C.CUSTOMER_ID = P.CUSTOMER_ID
JOIN PostCampaign A ON C.CUSTOMER_ID = A.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE (A.AVG_AFTER - P.AVG_BEFORE) / P.AVG_BEFORE >= 0.25;  -- 25% increase
GROUP BY C.CUSTOMER_NAME, P.AVG_BEFORE, A.AVG_AFTER;
```

---

### 39. List customers who have placed orders that match the mode of their previous order amounts and display their total orders.
```sql
WITH PreviousOrders AS (
    SELECT CUSTOMER_ID, 
           ORDER_AMOUNT,
           LAG(ORDER_AMOUNT) OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_DATE) AS PREV_ORDER_AMOUNT
    FROM Orders
),
ModeOrders AS (
    SELECT CUSTOMER_ID, ORDER_AMOUNT AS MODE_AMOUNT
    FROM PreviousOrders
    GROUP BY CUSTOMER_ID, ORDER_AMOUNT
    HAVING COUNT(*) = (SELECT MAX(COUNT(*)) 
                       FROM PreviousOrders 
                       WHERE CUSTOMER_ID = PreviousOrders.CUSTOMER_ID
                       GROUP BY ORDER_AMOUNT)
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN ModeOrders M ON C.CUSTOMER_ID = M.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE O.ORDER_AMOUNT = M.MODE_AMOUNT
GROUP BY C.CUSTOMER_NAME;
```

---

### 40. Retrieve customers who have maintained an average order amount within a specific range (e.g., 1000 to 2000) over a long period and show their total orders.
```sql
WITH CustomerStats AS (
    SELECT CUSTOMER_ID, AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN CustomerStats S ON C.CUSTOMER_ID = S.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE S.AVG_ORDER_AMOUNT BETWEEN 1000 AND 2000
GROUP BY C.CUSTOMER_NAME;
```

---

### 41. Identify customers who place the highest number of orders during a promotional period and display their total order amounts for that period.
```sql
WITH PromotionalOrders AS (
    SELECT CUSTOMER_ID, 
           COUNT(ORDER_ID) AS ORDER_COUNT, 
           SUM(ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
    FROM Orders
    WHERE ORDER_DATE >= TO_DATE('01-JUN-2024', 'DD-MON-YYYY') AND ORDER_DATE <= TO_DATE('30-JUN-2024', 'DD-MON-YYYY')  -- Promotional period
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, P.ORDER_COUNT, P.TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN PromotionalOrders P ON C.CUSTOMER_ID = P.CUSTOMER_ID
ORDER BY P.ORDER_COUNT DESC
FETCH FIRST 1 ROW ONLY;  -- Top customer during promotional period
```

---

### 42. Find customers who have placed orders with a total amount exceeding the combined amounts of their last three orders and show their total orders.
```sql
WITH LastThreeOrders AS (
    SELECT CUSTOMER_ID, 
           ORDER_AMOUNT,
           ROW_NUMBER() OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_DATE DESC) AS ORDER_RANK
    FROM Orders
),
CombinedLastThree AS (
    SELECT CUSTOMER_ID, SUM(ORDER_AMOUNT) AS LAST_THREE_TOTAL
    FROM LastThreeOrders
    WHERE ORDER_RANK <= 3
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
JOIN CombinedLastThree L ON C.CUSTOMER_ID = L.CUSTOMER_ID
WHERE O.ORDER_AMOUNT > L.LAST_THREE_TOTAL
GROUP BY C.CUSTOMER_NAME;
```

---

### 43. Show customers whose order history shows a pattern of decreasing frequency but increasing average order value over time.
```sql
WITH OrderFrequency AS (
    SELECT CUSTOMER_ID, 
           EXTRACT(YEAR FROM ORDER_DATE) AS ORDER_YEAR,
           COUNT(ORDER_ID) AS ORDER_COUNT,
           AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
    GROUP BY CUSTOMER_ID, EXTRACT(YEAR FROM ORDER_DATE)
),
FrequencyTrend AS (
    SELECT CUSTOMER_ID,
           LAG(ORDER_COUNT) OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_YEAR) AS PREV_ORDER_COUNT,
           AVG_ORDER_AMOUNT
    FROM OrderFrequency
)
SELECT C.CUSTOMER_NAME
FROM Customers C
JOIN FrequencyTrend F ON C.CUSTOMER_ID = F.CUSTOMER_ID
WHERE F.ORDER_COUNT < F.PREV_ORDER_COUNT AND F.AVG_ORDER_AMOUNT > LAG(F.AVG_ORDER_AMOUNT) OVER (PARTITION BY F.CUSTOMER_ID ORDER BY F.ORDER_YEAR);
```

---

### 44. Retrieve customers who placed orders that exactly match the average order amount of all customers and display their total orders.
```sql
WITH AverageOrder AS (
    SELECT AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM Orders
),
MatchingOrders AS (
    SELECT CUSTOMER_ID, COUNT(ORDER_ID) AS TOTAL_ORDERS
    FROM Orders
    WHERE ORDER_AMOUNT = (SELECT AVG_ORDER_AMOUNT FROM AverageOrder)
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, M.TOTAL_ORDERS
FROM Customers C
JOIN MatchingOrders M ON C.CUSTOMER_ID = M.CUSTOMER_ID;
```

---

### 45. List customers who have an average order amount that is consistently higher than the average of their segment and display their total orders.
```sql
WITH SegmentStats AS (
    SELECT SEGMENT_ID, AVG(ORDER_AMOUNT) AS SEGMENT_AVG
    FROM Customers C
    JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    GROUP BY SEGMENT_ID
),
CustomerStats AS (
    SELECT CUSTOMER_ID, AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT, SEGMENT_ID
    FROM Customers C
    JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
    GROUP BY CUSTOMER_ID, SEGMENT_ID
)
SELECT C.CUSTOMER_NAME, COUNT(O.ORDER_ID) AS TOTAL_ORDERS
FROM Customers C
JOIN CustomerStats S ON C.CUSTOMER_ID = S.CUSTOMER_ID
JOIN SegmentStats G ON S.SEGMENT_ID = G.SEGMENT_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE S.AVG_ORDER_AMOUNT > G.SEGMENT_AVG
GROUP BY C.CUSTOMER_NAME;
```

---

### 46. Identify customers whose orders reflect an increase in order value but a decrease in frequency and display their total order amounts.
```sql
WITH OrderTrends AS (
    SELECT CUSTOMER_ID,
           COUNT(ORDER_ID) AS ORDER_COUNT,
           AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT,
           ROW_NUMBER() OVER (PARTITION BY CUSTOMER_ID ORDER BY EXTRACT(YEAR FROM ORDER_DATE)) AS YEAR_RANK
    FROM Orders
    GROUP BY CUSTOMER_ID, EXTRACT(YEAR FROM ORDER_DATE)
),
TrendsComparison AS (
    SELECT CUSTOMER_ID,
           LAG(ORDER_COUNT) OVER (PARTITION BY CUSTOMER_ID ORDER BY YEAR_RANK) AS PREV_ORDER_COUNT,
           LAG(AVG_ORDER_AMOUNT) OVER (PARTITION BY CUSTOMER_ID ORDER BY YEAR_RANK) AS PREV_AVG_ORDER_AMOUNT,
           AVG_ORDER_AMOUNT
    FROM OrderTrends
)
SELECT C.CUSTOMER_NAME, SUM(O.ORDER_AMOUNT) AS TOTAL_ORDER_AMOUNT
FROM Customers C
JOIN TrendsComparison T ON C.CUSTOMER_ID = T.CUSTOMER_ID
JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE T.ORDER_COUNT < T.PREV_ORDER_COUNT AND T.AVG_ORDER_AMOUNT > T.PREV_AVG_ORDER_AMOUNT
GROUP BY C.CUSTOMER_NAME;
```

---

### 47. Find customers who have been inactive for the longest time and display their total order count before inactivity began.
```sql
WITH InactiveCustomers AS (
    SELECT CUSTOMER_ID, 
           MAX(ORDER_DATE) AS LAST_ORDER_DATE
    FROM Orders
    GROUP BY CUSTOMER_ID
),
InactiveDuration AS (
    SELECT C.CUSTOMER_ID, 
           C.CUSTOMER_NAME, 
           COUNT(O.ORDER_ID) AS TOTAL_ORDERS_BEFORE_INACTIVE,
           TRUNC(SYSDATE - LAST_ORDER_DATE) AS DAYS_INACTIVE
    FROM InactiveCustomers IC
    JOIN Customers C ON IC.CUSTOMER_ID = C.CUSTOMER_ID
    LEFT JOIN Orders O ON C.CUSTOMER_ID = O.CUSTOMER_ID AND O.ORDER_DATE < IC.LAST_ORDER_DATE
    GROUP BY C.CUSTOMER_ID, C.CUSTOMER_NAME, IC.LAST_ORDER_DATE
)
SELECT CUSTOMER_NAME, TOTAL_ORDERS_BEFORE_INACTIVE, DAYS_INACTIVE
FROM InactiveDuration
ORDER BY DAYS_INACTIVE DESC
FETCH FIRST 1 ROW ONLY;  -- Customer inactive for the longest time
```

---

### 48. Retrieve customers who have made purchases from multiple categories and show their total spending across those categories.
```sql
WITH CategorySpending AS (
    SELECT C.CUSTOMER_ID,
           P.PRODUCT_CATEGORY,
           SUM(O.ORDER_AMOUNT) AS TOTAL_SPENT
    FROM Orders O
    JOIN Products P ON O.PRODUCT_ID = P.PRODUCT_ID
    JOIN Customers C ON O.CUSTOMER_ID = C.CUSTOMER_ID
    GROUP BY C.CUSTOMER_ID, P.PRODUCT_CATEGORY
),
MultipleCategories AS (
    SELECT CUSTOMER_ID, SUM(TOTAL_SPENT) AS TOTAL_SPENDING
    FROM CategorySpending
    GROUP BY CUSTOMER_ID
    HAVING COUNT(DISTINCT PRODUCT_CATEGORY) > 1
)
SELECT C.CUSTOMER_NAME, M.TOTAL_SPENDING
FROM Customers C
JOIN MultipleCategories M ON C.CUSTOMER_ID = M.CUSTOMER_ID;
```

---

### 49. List customers whose orders include unique order amounts that differ from their average order amount and display their total orders.
```sql
WITH CustomerOrders AS (
    SELECT CUSTOMER_ID, 
           ORDER_AMOUNT,
           AVG(ORDER_AMOUNT) OVER (PARTITION BY CUSTOMER_ID) AS AVG_ORDER_AMOUNT
    FROM Orders
),
UniqueOrders AS (
    SELECT CUSTOMER_ID, 
           COUNT(ORDER_ID) AS TOTAL_ORDERS
    FROM CustomerOrders
    WHERE ORDER_AMOUNT <> AVG_ORDER_AMOUNT
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, U.TOTAL_ORDERS
FROM Customers C
JOIN UniqueOrders U ON C.CUSTOMER_ID = U.CUSTOMER_ID;
```

---

### 50. Find customers who have an increasing order amount pattern during the last six months and display their total orders and average order amount.
```sql
WITH RecentOrders AS (
    SELECT CUSTOMER_ID,
           ORDER_DATE,
           ORDER_AMOUNT,
           ROW_NUMBER() OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_DATE) AS ORDER_RANK
    FROM Orders
    WHERE ORDER_DATE >= ADD_MONTHS(SYSDATE, -6)  -- Last six months
),
IncreasingOrders AS (
    SELECT CUSTOMER_ID,
           COUNT(ORDER_ID) AS TOTAL_ORDERS,
           AVG(ORDER_AMOUNT) AS AVG_ORDER_AMOUNT
    FROM RecentOrders
    WHERE ORDER_AMOUNT > LAG(ORDER_AMOUNT) OVER (PARTITION BY CUSTOMER_ID ORDER BY ORDER_RANK)
    GROUP BY CUSTOMER_ID
)
SELECT C.CUSTOMER_NAME, I.TOTAL_ORDERS, I.AVG_ORDER_AMOUNT
FROM Customers C
JOIN IncreasingOrders I ON C.CUSTOMER_ID = I.CUSTOMER_ID;
```

---

