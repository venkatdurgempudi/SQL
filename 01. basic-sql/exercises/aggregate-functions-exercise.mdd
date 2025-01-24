```sql
-- Create the SALES table
CREATE TABLE SALES (
    SALE_ID NUMBER NOT NULL,        -- A single transaction ID (can repeat for grouped items)
    LINE_ITEM_ID NUMBER NOT NULL,  -- Unique identifier for each item in the transaction
    PRODUCT_NAME VARCHAR2(100) NOT NULL, -- Product name involved in the transaction
    CUSTOMER_NAME VARCHAR2(100) NOT NULL, -- Customer name who made the purchase
    SALE_DATE DATE NOT NULL,       -- Date of the transaction
    QUANTITY NUMBER NOT NULL,      -- Quantity of the product sold
    PRICE_PER_UNIT NUMBER NOT NULL,-- Price per unit of the product
    CITY VARCHAR2(50),             -- City where the sale happened
    COUNTRY VARCHAR2(50),          -- Country of the sale
    REGION VARCHAR2(50),           -- Region of the sale
    PRIMARY KEY (SALE_ID, LINE_ITEM_ID) -- Composite key to uniquely identify each item in a sale
);

-- Insert the data into Sales Table
INSERT INTO SALES VALUES (1, 1, 'Laptop', 'John Doe', TO_DATE('01-JAN-2023', 'DD-MON-YYYY'), 1, 1200, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (2, 1, 'Smartphone', 'Jane Smith', TO_DATE('02-JAN-2023', 'DD-MON-YYYY'), 1, 800, 'Toronto', 'Canada', 'North America');
INSERT INTO SALES VALUES (3, 1, 'Tablet', 'Alice Johnson', TO_DATE('03-JAN-2023', 'DD-MON-YYYY'), 1, 400, 'London', 'UK', 'Europe');
INSERT INTO SALES VALUES (4, 1, 'Refrigerator', 'Rajesh Kumar', TO_DATE('04-JAN-2023', 'DD-MON-YYYY'), 1, 1500, 'Mumbai', 'India', 'Asia');
INSERT INTO SALES VALUES (5, 1, 'Camera', 'Satoshi Tanaka', TO_DATE('05-JAN-2023', 'DD-MON-YYYY'), 1, 1000, 'Tokyo', 'Japan', 'Asia');
INSERT INTO SALES VALUES (6, 1, 'Washing Machine', 'Emily Davis', TO_DATE('06-JAN-2023', 'DD-MON-YYYY'), 1, 900, 'Sydney', 'Australia', 'Oceania');
INSERT INTO SALES VALUES (7, 1, 'Smartwatch', 'Liam Wilson', TO_DATE('07-JAN-2023', 'DD-MON-YYYY'), 1, 250, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (8, 1, 'Electric Kettle', 'Francois Dupont', TO_DATE('08-JAN-2023', 'DD-MON-YYYY'), 2, 40, 'Paris', 'France', 'Europe');
INSERT INTO SALES VALUES (9, 1, 'Air Conditioner', 'Sophia Brown', TO_DATE('09-JAN-2023', 'DD-MON-YYYY'), 1, 1200, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (10, 1, 'Fitness Tracker', 'David Green', TO_DATE('10-JAN-2023', 'DD-MON-YYYY'), 1, 150, 'Toronto', 'Canada', 'North America');
INSERT INTO SALES VALUES (11, 1, 'Heater', 'John Snow', TO_DATE('11-JAN-2023', 'DD-MON-YYYY'), 1, 300, 'Sydney', 'Australia', 'Oceania');
INSERT INTO SALES VALUES (12, 1, 'Microwave', 'Emily Watson', TO_DATE('12-JAN-2023', 'DD-MON-YYYY'), 1, 200, 'Mumbai', 'India', 'Asia');
INSERT INTO SALES VALUES (13, 1, 'Power Bank', 'Liam Neeson', TO_DATE('13-JAN-2023', 'DD-MON-YYYY'), 1, 75, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (14, 1, 'Wireless Earbuds', 'Sophia Loren', TO_DATE('14-JAN-2023', 'DD-MON-YYYY'), 2, 200, 'Paris', 'France', 'Europe');
INSERT INTO SALES VALUES (15, 1, 'Headphones', 'Chris Evans', TO_DATE('15-JAN-2023', 'DD-MON-YYYY'), 1, 100, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (16, 1, 'Tripod', 'Scarlett Johansson', TO_DATE('16-JAN-2023', 'DD-MON-YYYY'), 1, 50, 'Toronto', 'Canada', 'North America');
INSERT INTO SALES VALUES (17, 1, 'Vacuum Cleaner', 'Tony Stark', TO_DATE('17-JAN-2023', 'DD-MON-YYYY'), 1, 250, 'Mumbai', 'India', 'Asia');
INSERT INTO SALES VALUES (18, 1, 'Blender', 'Natasha Romanoff', TO_DATE('18-JAN-2023', 'DD-MON-YYYY'), 1, 100, 'Tokyo', 'Japan', 'Asia');
INSERT INTO SALES VALUES (19, 1, 'Stylus', 'Steve Rogers', TO_DATE('19-JAN-2023', 'DD-MON-YYYY'), 1, 30, 'London', 'UK', 'Europe');
INSERT INTO SALES VALUES (20, 1, 'Memory Card', 'Bruce Banner', TO_DATE('20-JAN-2023', 'DD-MON-YYYY'), 2, 20, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (21, 1, 'Laptop', 'John Doe', TO_DATE('21-JAN-2023', 'DD-MON-YYYY'), 1, 1200, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (21, 2, 'Mouse', 'John Doe', TO_DATE('21-JAN-2023', 'DD-MON-YYYY'), 2, 25, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (22, 1, 'Smartphone', 'Jane Smith', TO_DATE('22-JAN-2023', 'DD-MON-YYYY'), 1, 800, 'Toronto', 'Canada', 'North America');
INSERT INTO SALES VALUES (22, 2, 'Charger', 'Jane Smith', TO_DATE('22-JAN-2023', 'DD-MON-YYYY'), 1, 50, 'Toronto', 'Canada', 'North America');
INSERT INTO SALES VALUES (51, 1, 'Tablet', 'Alice Johnson', TO_DATE('23-JAN-2023', 'DD-MON-YYYY'), 1, 400, 'London', 'UK', 'Europe');
INSERT INTO SALES VALUES (51, 2, 'Stylus', 'Alice Johnson', TO_DATE('23-JAN-2023', 'DD-MON-YYYY'), 1, 30, 'London', 'UK', 'Europe');
INSERT INTO SALES VALUES (51, 3, 'Keyboard', 'Alice Johnson', TO_DATE('23-JAN-2023', 'DD-MON-YYYY'), 1, 45, 'London', 'UK', 'Europe');
INSERT INTO SALES VALUES (101, 1, 'Blender', 'Derek Miles', TO_DATE('01-FEB-2023', 'DD-MON-YYYY'), 1, 100, 'Chicago', 'USA', 'North America');
INSERT INTO SALES VALUES (102, 1, 'Microwave', 'Helen Carter', TO_DATE('02-FEB-2023', 'DD-MON-YYYY'), 1, 200, 'Los Angeles', 'USA', 'North America');
INSERT INTO SALES VALUES (103, 1, 'Refrigerator', 'Adam Black', TO_DATE('03-FEB-2023', 'DD-MON-YYYY'), 1, 1500, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (104, 1, 'Smartwatch', 'Emma Brown', TO_DATE('04-FEB-2023', 'DD-MON-YYYY'), 1, 250, 'Paris', 'France', 'Europe');
INSERT INTO SALES VALUES (105, 1, 'Fitness Tracker', 'Liam White', TO_DATE('05-FEB-2023', 'DD-MON-YYYY'), 2, 150, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (106, 1, 'Washing Machine', 'Sophia Johnson', TO_DATE('06-FEB-2023', 'DD-MON-YYYY'), 1, 900, 'Tokyo', 'Japan', 'Asia');
INSERT INTO SALES VALUES (106, 2, 'Dryer', 'Sophia Johnson', TO_DATE('06-FEB-2023', 'DD-MON-YYYY'), 1, 850, 'Tokyo', 'Japan', 'Asia');
INSERT INTO SALES VALUES (107, 1, 'Laptop', 'Oliver Williams', TO_DATE('07-FEB-2023', 'DD-MON-YYYY'), 1, 1200, 'Sydney', 'Australia', 'Oceania');
INSERT INTO SALES VALUES (107, 2, 'Mouse', 'Oliver Williams', TO_DATE('07-FEB-2023', 'DD-MON-YYYY'), 2, 25, 'Sydney', 'Australia', 'Oceania');
INSERT INTO SALES VALUES (108, 1, 'Tablet', 'Emily Smith', TO_DATE('08-FEB-2023', 'DD-MON-YYYY'), 1, 400, 'Mumbai', 'India', 'Asia');
INSERT INTO SALES VALUES (108, 2, 'Stylus', 'Emily Smith', TO_DATE('08-FEB-2023', 'DD-MON-YYYY'), 1, 30, 'Mumbai', 'India', 'Asia');
INSERT INTO SALES VALUES (108, 3, 'Keyboard', 'Emily Smith', TO_DATE('08-FEB-2023', 'DD-MON-YYYY'), 1, 45, 'Mumbai', 'India', 'Asia');
INSERT INTO SALES VALUES (108, 4, 'Smartphone', 'Emily Smith', TO_DATE('08-FEB-2023', 'DD-MON-YYYY'), 1, 800, 'Mumbai', 'India', 'Asia');
INSERT INTO SALES VALUES (109, 1, 'Air Conditioner', 'Mason Lee', TO_DATE('09-FEB-2023', 'DD-MON-YYYY'), 1, 1200, 'Toronto', 'Canada', 'North America');
INSERT INTO SALES VALUES (110, 1, 'Electric Kettle', 'Olivia Brown', TO_DATE('10-FEB-2023', 'DD-MON-YYYY'), 1, 40, 'London', 'UK', 'Europe');
INSERT INTO SALES VALUES (111, 1, 'Vacuum Cleaner', 'Isabella Clark', TO_DATE('11-FEB-2023', 'DD-MON-YYYY'), 1, 250, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (112, 1, 'Blender', 'Ethan Martinez', TO_DATE('12-FEB-2023', 'DD-MON-YYYY'), 1, 100, 'Chicago', 'USA', 'North America');
INSERT INTO SALES VALUES (113, 1, 'Memory Card', 'Lucas Anderson', TO_DATE('13-FEB-2023', 'DD-MON-YYYY'), 2, 20, 'Tokyo', 'Japan', 'Asia');
INSERT INTO SALES VALUES (114, 1, 'Fitness Tracker', 'Benjamin Young', TO_DATE('14-FEB-2023', 'DD-MON-YYYY'), 1, 150, 'Paris', 'France', 'Europe');
INSERT INTO SALES VALUES (114, 2, 'Wireless Earbuds', 'Benjamin Young', TO_DATE('14-FEB-2023', 'DD-MON-YYYY'), 1, 200, 'Paris', 'France', 'Europe');
INSERT INTO SALES VALUES (115, 1, 'Tripod', 'Sophia Lopez', TO_DATE('15-FEB-2023', 'DD-MON-YYYY'), 1, 50, 'Sydney', 'Australia', 'Oceania');
INSERT INTO SALES VALUES (115, 2, 'Camera', 'Sophia Lopez', TO_DATE('15-FEB-2023', 'DD-MON-YYYY'), 1, 1000, 'Sydney', 'Australia', 'Oceania');
INSERT INTO SALES VALUES (116, 1, 'Refrigerator', 'Emma King', TO_DATE('16-FEB-2023', 'DD-MON-YYYY'), 1, 1500, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (116, 2, 'Microwave', 'Emma King', TO_DATE('16-FEB-2023', 'DD-MON-YYYY'), 1, 200, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (116, 3, 'Blender', 'Emma King', TO_DATE('16-FEB-2023', 'DD-MON-YYYY'), 1, 100, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (116, 4, 'Electric Kettle', 'Emma King', TO_DATE('16-FEB-2023', 'DD-MON-YYYY'), 2, 40, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (117, 1, 'Heater', 'Ethan Walker', TO_DATE('17-FEB-2023', 'DD-MON-YYYY'), 1, 300, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (118, 1, 'Mouse', 'Sophia Scott', TO_DATE('18-FEB-2023', 'DD-MON-YYYY'), 2, 25, 'Toronto', 'Canada', 'North America');
INSERT INTO SALES VALUES (119, 1, 'Keyboard', 'Isabella Davis', TO_DATE('19-FEB-2023', 'DD-MON-YYYY'), 1, 45, 'London', 'UK', 'Europe');
INSERT INTO SALES VALUES (120, 1, 'Smartphone', 'Lucas Harris', TO_DATE('20-FEB-2023', 'DD-MON-YYYY'), 1, 800, 'Paris', 'France', 'Europe');
INSERT INTO SALES VALUES (121, 1, 'Fitness Tracker', 'Michael Lewis', TO_DATE('21-FEB-2023', 'DD-MON-YYYY'), 1, 150, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (121, 2, 'Smartwatch', 'Michael Lewis', TO_DATE('21-FEB-2023', 'DD-MON-YYYY'), 1, 250, 'Berlin', 'Germany', 'Europe');
INSERT INTO SALES VALUES (122, 1, 'Laptop', 'Chris Evans', TO_DATE('22-FEB-2023', 'DD-MON-YYYY'), 1, 1200, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (122, 2, 'Mouse', 'Chris Evans', TO_DATE('22-FEB-2023', 'DD-MON-YYYY'), 2, 25, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (122, 3, 'Keyboard', 'Chris Evans', TO_DATE('22-FEB-2023', 'DD-MON-YYYY'), 1, 45, 'New York', 'USA', 'North America');
INSERT INTO SALES VALUES (122, 4, 'Headphones', 'Chris Evans', TO_DATE('22-FEB-2023', 'DD-MON-YYYY'), 1, 100, 'New York', 'USA', 'North America');

Commit;
```

### Assignment: SQL Aggregate Functions  

**1.** Find the total number of products sold across all transactions.  

**2.** Calculate the total revenue generated from all sales.  

**3.** Identify the highest `PRICE_PER_UNIT` among all products sold.  

**4.** Determine the average `QUANTITY` of products sold per transaction.  

**5.** Find the lowest total revenue (`QUANTITY * PRICE_PER_UNIT`) for a single line item.  

**6.** Count the number of distinct customers (`CUSTOMER_NAME`) who made purchases.  

**7.** List the total `QUANTITY` of products sold for each `PRODUCT_NAME`.  

**8.** Calculate the total revenue generated by each `CUSTOMER_NAME`.  

**9.** Find the maximum `QUANTITY` sold for each `CITY`.  

**10.** Identify the `CITY` and `PRODUCT_NAME` pairs where the total `QUANTITY` sold is greater than 10.  

**11.** Calculate the total revenue generated for each `REGION` and sort the results in descending order of revenue.  

**12.** Find the average `PRICE_PER_UNIT` for each `PRODUCT_NAME`.  

**13.** List the `CUSTOMER_NAME` who purchased more than 5 products in total.  

**14.** Identify `PRODUCT_NAME` and `CUSTOMER_NAME` pairs where the total revenue generated exceeds 2000.  

**15.** Calculate the total number of transactions (`SALE_ID`) for each `COUNTRY`.  

**16.** Find the day (`SALE_DATE`) with the highest total revenue.  

**17.** Determine the `REGION` where the average `PRICE_PER_UNIT` is the lowest.  

**18.** List all `PRODUCT_NAME`s where the total number of items sold exceeds 15.  

**19.** Find the `CITY` that generated the highest revenue for the product "Laptop".  

**20.** Identify the `CUSTOMER_NAME` who bought the highest total `QUANTITY` of items.  

**21.** Calculate the total revenue generated by all single-product sales (`SALE_ID` with only one line item).  

**22.** Find the average number of products sold per transaction across all `CITY`s.  

**23.** Determine the number of distinct `PRODUCT_NAME`s purchased in each `COUNTRY`.  

**24.** List `REGION`s where the total `QUANTITY` sold is less than 50.  

**25.** Identify `CUSTOMER_NAME`s who have made purchases in more than one `CITY`.  
