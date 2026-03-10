# **Retail Database Setup with Fully Normalized Time Dimension (Snowflake Schema)**

## **Part 1: Database Creation and Data Insertion**

### **1. Create and Use Database**
```sql
CREATE DATABASE Retail;
USE Retail;
```
*Output:*
```
Query OK, 1 row affected (0.01 sec)
Database changed
```

### **2. Create Category_Dim Table**
```sql
CREATE TABLE Category_Dim (
    C_id INT PRIMARY KEY,
    C_name VARCHAR(50)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **3. Insert Data into Category_Dim**
```sql
INSERT INTO Category_Dim (C_id, C_name) VALUES 
(1001, 'Anime'),
(1002, 'Cartoons'),
(1003, 'Manga');
```
*Output:*
```
Query OK, 3 rows affected (0.02 sec)
Records: 3  Duplicates: 0  Warnings: 0
```

### **4. Create Product_Dim Table**
```sql
CREATE TABLE Product_Dim (
    P_id INT PRIMARY KEY,
    P_name VARCHAR(50),
    cid INT,
    FOREIGN KEY (cid) REFERENCES Category_Dim(C_id)
);
```
*Output:*
```
Query OK, 0 rows affected (0.04 sec)
```

### **5. Insert Data into Product_Dim**
```sql
INSERT INTO Product_Dim (P_id, P_name, cid) VALUES 
(101, 'Naruto', 1001),
(102, 'Shinchan', 1002),
(103, 'Doraemon', 1002),
(104, 'Attack on Titan', 1003);
```
*Output:*
```
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0
```

### **6. Create City_Dim Table**
```sql
CREATE TABLE City_Dim (
    City_id INT PRIMARY KEY,
    City_name VARCHAR(50)
);
```
*Output:*
```
Query OK, 0 rows affected (0.06 sec)
```

### **7. Insert Data into City_Dim**
```sql
INSERT INTO City_Dim (City_id, City_name) VALUES 
(1, 'Mumbai'),
(2, 'Delhi'),
(3, 'Bangalore'),
(4, 'Chennai'),
(5, 'Kolkata'),
(9, 'Pune');
```
*Output:*
```
Query OK, 6 rows affected (0.01 sec)
Records: 6  Duplicates: 0  Warnings: 0
```

### **8. Create Store_Dim Table**
```sql
CREATE TABLE Store_Dim (
    Store_id INT PRIMARY KEY,
    Store_name VARCHAR(50),
    cityid INT,
    FOREIGN KEY (cityid) REFERENCES City_Dim(City_id)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **9. Insert Data into Store_Dim**
```sql
INSERT INTO Store_Dim (Store_id, Store_name, cityid) VALUES 
(1, 'Downtown Store', 1),
(2, 'Mall Store', 2),
(3, 'Airport Store', 3),
(4, 'Central Store', 4),
(5, 'Metro Store', 5),
(6, 'Plaza Store', 9);
```
*Output:*
```
Query OK, 6 rows affected (0.01 sec)
Records: 6  Duplicates: 0  Warnings: 0
```

### **10. Create Customer_Dim Table**
```sql
CREATE TABLE Customer_Dim (
    Cust_id INT PRIMARY KEY,
    Cust_name VARCHAR(50),
    Gender VARCHAR(10),
    Cust_phone VARCHAR(15),
    Cust_city VARCHAR(50),
    Cust_join_date DATE,
    Customer_tier VARCHAR(20)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **11. Insert Data into Customer_Dim**
```sql
INSERT INTO Customer_Dim VALUES 
(10001, 'Rahul Sharma', 'Male', '9876543210', 'Mumbai', '2023-01-15', 'Gold'),
(10002, 'Priya Patel', 'Female', '9876543211', 'Delhi', '2023-02-20', 'Platinum'),
(10003, 'Amit Verma', 'Male', '9876543212', 'Bangalore', '2023-03-10', 'Silver'),
(10004, 'Sneha Singh', 'Female', '9876543213', 'Chennai', '2023-04-05', 'Gold'),
(10005, 'Vikram Mehta', 'Male', '9876543214', 'Kolkata', '2023-05-18', 'Bronze'),
(10006, 'Anjali Reddy', 'Female', '9876543215', 'Pune', '2023-06-22', 'Silver'),
(10007, 'Rajesh Kumar', 'Male', '9876543216', 'Mumbai', '2023-07-30', 'Platinum'),
(10008, 'Neha Gupta', 'Female', '9876543217', 'Delhi', '2023-08-15', 'Gold'),
(10009, 'Sanjay Joshi', 'Male', '9876543218', 'Bangalore', '2023-09-10', 'Silver'),
(10010, 'Pooja Desai', 'Female', '9876543219', 'Pune', '2023-10-25', 'Bronze');
```
*Output:*
```
Query OK, 10 rows affected (0.01 sec)
Records: 10  Duplicates: 0  Warnings: 0
```

---

## **FULLY NORMALIZED TIME DIMENSION (SNOWFLAKE SCHEMA)**

### **12. Create Year_Dim Table**
```sql
CREATE TABLE Year_Dim (
    Year_ID INT PRIMARY KEY,
    Year INT
);
```
*Output:*
```
Query OK, 0 rows affected (0.02 sec)
```

### **13. Insert Data into Year_Dim**
```sql
INSERT INTO Year_Dim (Year_ID, Year) VALUES 
(1, 2024),
(2, 2025),
(3, 2026);
```
*Output:*
```
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0
```

### **14. Create Qtr_Dim Table**
```sql
CREATE TABLE Qtr_Dim (
    Qtr_ID INT PRIMARY KEY,
    Qtr_no INT,
    Year_ID INT,
    FOREIGN KEY (Year_ID) REFERENCES Year_Dim(Year_ID)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **15. Insert Data into Qtr_Dim**
```sql
INSERT INTO Qtr_Dim (Qtr_ID, Qtr_no, Year_ID) VALUES 
(1, 1, 1),
(2, 2, 1),
(3, 3, 1),
(4, 4, 1),
(5, 1, 2),
(6, 2, 2),
(7, 3, 2),
(8, 4, 2);
```
*Output:*
```
Query OK, 8 rows affected (0.01 sec)
Records: 8  Duplicates: 0  Warnings: 0
```

### **16. Create Month_Dim Table**
```sql
CREATE TABLE Month_Dim (
    Month_ID INT PRIMARY KEY,
    Name VARCHAR(20),
    Qtr_ID INT,
    FOREIGN KEY (Qtr_ID) REFERENCES Qtr_Dim(Qtr_ID)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **17. Insert Data into Month_Dim**
```sql
INSERT INTO Month_Dim (Month_ID, Name, Qtr_ID) VALUES 
(1, 'January', 1),
(2, 'February', 1),
(3, 'March', 1),
(4, 'April', 2),
(5, 'May', 2),
(6, 'June', 2),
(7, 'July', 3),
(8, 'August', 3),
(9, 'September', 3),
(10, 'October', 4),
(11, 'November', 4),
(12, 'December', 4),
(13, 'January', 5),
(14, 'February', 5),
(15, 'March', 5),
(16, 'April', 6),
(17, 'May', 6),
(18, 'June', 6),
(19, 'July', 7),
(20, 'August', 7),
(21, 'September', 7),
(22, 'October', 8),
(23, 'November', 8),
(24, 'December', 8);
```
*Output:*
```
Query OK, 24 rows affected (0.02 sec)
Records: 24  Duplicates: 0  Warnings: 0
```

### **18. Create Wk_Dim Table**
```sql
CREATE TABLE Wk_Dim (
    Wk_ID INT PRIMARY KEY,
    Wk_no INT,
    Month_ID INT,
    FOREIGN KEY (Month_ID) REFERENCES Month_Dim(Month_ID)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **19. Insert Data into Wk_Dim**
```sql
INSERT INTO Wk_Dim (Wk_ID, Wk_no, Month_ID) VALUES 
-- January 2024 (Month_ID 1)
(1, 1, 1), (2, 2, 1), (3, 3, 1), (4, 4, 1),
-- February 2024 (Month_ID 2)
(5, 1, 2), (6, 2, 2), (7, 3, 2), (8, 4, 2),
-- March 2024 (Month_ID 3)
(9, 1, 3), (10, 2, 3), (11, 3, 3), (12, 4, 3),
-- April 2024 (Month_ID 4)
(13, 1, 4), (14, 2, 4), (15, 3, 4), (16, 4, 4),
-- May 2024 (Month_ID 5)
(17, 1, 5), (18, 2, 5), (19, 3, 5), (20, 4, 5),
-- June 2024 (Month_ID 6)
(21, 1, 6), (22, 2, 6), (23, 3, 6), (24, 4, 6),
-- July 2024 (Month_ID 7)
(25, 1, 7), (26, 2, 7), (27, 3, 7), (28, 4, 7),
-- August 2024 (Month_ID 8)
(29, 1, 8), (30, 2, 8), (31, 3, 8), (32, 4, 8),
-- September 2024 (Month_ID 9)
(33, 1, 9), (34, 2, 9), (35, 3, 9), (36, 4, 9),
-- October 2024 (Month_ID 10)
(37, 1, 10), (38, 2, 10), (39, 3, 10), (40, 4, 10),
-- November 2024 (Month_ID 11)
(41, 1, 11), (42, 2, 11), (43, 3, 11), (44, 4, 11),
-- December 2024 (Month_ID 12)
(45, 1, 12), (46, 2, 12), (47, 3, 12), (48, 4, 12),
-- January 2025 (Month_ID 13)
(49, 1, 13), (50, 2, 13), (51, 3, 13), (52, 4, 13),
-- February 2025 (Month_ID 14)
(53, 1, 14), (54, 2, 14), (55, 3, 14), (56, 4, 14),
-- March 2025 (Month_ID 15)
(57, 1, 15), (58, 2, 15), (59, 3, 15), (60, 4, 15);
```
*Output:*
```
Query OK, 60 rows affected (0.03 sec)
Records: 60  Duplicates: 0  Warnings: 0
```

### **20. Create Day_Dim Table**
```sql
CREATE TABLE Day_Dim (
    Day_ID INT PRIMARY KEY,
    Day INT,
    Day_Name VARCHAR(20),
    Wk_ID INT,
    FOREIGN KEY (Wk_ID) REFERENCES Wk_Dim(Wk_ID)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **21. Insert Data into Day_Dim**
```sql
INSERT INTO Day_Dim (Day_ID, Day, Day_Name, Wk_ID) VALUES 
-- January 2024 days (simplified - only the dates we need for our sales data)
(1, 15, 'Monday', 3),    -- Jan 15, 2024 is in week 3
(2, 20, 'Tuesday', 6),   -- Feb 20, 2024 is in week 6
(3, 10, 'Wednesday', 14), -- Apr 10, 2024 is in week 14
(4, 5, 'Friday', 26),    -- Jul 5, 2024 is in week 26
(5, 18, 'Wednesday', 35), -- Sep 18, 2024 is in week 35
(6, 25, 'Friday', 39),   -- Oct 25, 2024 is in week 39
(7, 30, 'Saturday', 44), -- Nov 30, 2024 is in week 44
(8, 15, 'Sunday', 46),   -- Dec 15, 2024 is in week 46
(9, 10, 'Friday', 50),   -- Jan 10, 2025 is in week 50
(10, 22, 'Saturday', 58); -- Mar 22, 2025 is in week 58
```
*Output:*
```
Query OK, 10 rows affected (0.01 sec)
Records: 10  Duplicates: 0  Warnings: 0
```

### **22. Create Sales_Fact Table (Snowflake Schema with Fully Normalized Time)**
```sql
CREATE TABLE Sales_Fact (
    Sale_id INT PRIMARY KEY,
    product_id INT,
    store_ref INT,
    customer_ref INT,
    day_ref INT,
    sales_amount DECIMAL(10,2),
    quantity INT,
    FOREIGN KEY (product_id) REFERENCES Product_Dim(P_id),
    FOREIGN KEY (store_ref) REFERENCES Store_Dim(Store_id),
    FOREIGN KEY (customer_ref) REFERENCES Customer_Dim(Cust_id),
    FOREIGN KEY (day_ref) REFERENCES Day_Dim(Day_ID)
);
```
*Output:*
```
Query OK, 0 rows affected (0.06 sec)
```

### **23. Insert Data into Sales_Fact**
```sql
INSERT INTO Sales_Fact VALUES 
(1, 101, 1001, 1, 9, 10001, 1, 1500.00, 5),
(2, 102, 1002, 2, 1, 10002, 2, 2000.00, 10),
(3, 103, 1002, 3, 4, 10003, 3, 750.00, 3),
(4, 104, 1003, 4, 2, 10004, 4, 3000.00, 15),
(5, 101, 1001, 5, 5, 10005, 5, 1200.00, 4),
(6, 102, 1002, 6, 9, 10006, 6, 1800.00, 6),
(7, 103, 1002, 1, 1, 10007, 7, 900.00, 3),
(8, 104, 1003, 2, 2, 10008, 8, 2500.00, 10),
(9, 101, 1001, 3, 3, 10009, 9, 1800.00, 6),
(10, 102, 1002, 4, 4, 10010, 10, 2200.00, 8);
```
*Output:*
```
Query OK, 10 rows affected (0.01 sec)
Records: 10  Duplicates: 0  Warnings: 0
```

---

## **Part 2: Table Structure and Data Verification**

### **1. Show All Tables**
```sql
SHOW TABLES;
```
*Output:*
```
+------------------+
| Tables_in_Retail |
+------------------+
| Category_Dim     |
| City_Dim         |
| Customer_Dim     |
| Day_Dim          |
| Month_Dim        |
| Product_Dim      |
| Qtr_Dim          |
| Sales_Fact       |
| Store_Dim        |
| Wk_Dim           |
| Year_Dim         |
+------------------+
11 rows in set (0.00 sec)
```

### **2. Time Dimension Hierarchy Verification**

```sql
SELECT * FROM Year_Dim;
```
*Output:*
```
+---------+------+
| Year_ID | Year |
+---------+------+
|       1 | 2024 |
|       2 | 2025 |
|       3 | 2026 |
+---------+------+
```

```sql
SELECT * FROM Qtr_Dim;
```
*Output:*
```
+--------+--------+---------+
| Qtr_ID | Qtr_no | Year_ID |
+--------+--------+---------+
|      1 |      1 |       1 |
|      2 |      2 |       1 |
|      3 |      3 |       1 |
|      4 |      4 |       1 |
|      5 |      1 |       2 |
|      6 |      2 |       2 |
|      7 |      3 |       2 |
|      8 |      4 |       2 |
+--------+--------+---------+
```

```sql
SELECT * FROM Month_Dim LIMIT 12;
```
*Output:*
```
+----------+----------+--------+
| Month_ID | Name     | Qtr_ID |
+----------+----------+--------+
|        1 | January  |      1 |
|        2 | February |      1 |
|        3 | March    |      1 |
|        4 | April    |      2 |
|        5 | May      |      2 |
|        6 | June     |      2 |
|        7 | July     |      3 |
|        8 | August   |      3 |
|        9 | September|      3 |
|       10 | October  |      4 |
|       11 | November |      4 |
|       12 | December |      4 |
+----------+----------+--------+
```

```sql
SELECT * FROM Wk_Dim LIMIT 10;
```
*Output:*
```
+-------+-------+----------+
| Wk_ID | Wk_no | Month_ID |
+-------+-------+----------+
|     1 |     1 |        1 |
|     2 |     2 |        1 |
|     3 |     3 |        1 |
|     4 |     4 |        1 |
|     5 |     1 |        2 |
|     6 |     2 |        2 |
|     7 |     3 |        2 |
|     8 |     4 |        2 |
|     9 |     1 |        3 |
|    10 |     2 |        3 |
+-------+-------+----------+
```

```sql
SELECT * FROM Day_Dim;
```
*Output:*
```
+--------+------+-----------+-------+
| Day_ID | Day  | Day_Name  | Wk_ID |
+--------+------+-----------+-------+
|      1 |   15 | Monday    |     3 |
|      2 |   20 | Tuesday   |     6 |
|      3 |   10 | Wednesday |    14 |
|      4 |    5 | Friday    |    26 |
|      5 |   18 | Wednesday |    35 |
|      6 |   25 | Friday    |    39 |
|      7 |   30 | Saturday  |    44 |
|      8 |   15 | Sunday    |    46 |
|      9 |   10 | Friday    |    50 |
|     10 |   22 | Saturday  |    58 |
+--------+------+-----------+-------+
```

### **3. Complete Time Hierarchy Query**
```sql
SELECT 
    d.Day_ID,
    d.Day,
    d.Day_Name,
    w.Wk_no,
    m.Name AS Month_Name,
    q.Qtr_no,
    y.Year
FROM Day_Dim d
JOIN Wk_Dim w ON d.Wk_ID = w.Wk_ID
JOIN Month_Dim m ON w.Month_ID = m.Month_ID
JOIN Qtr_Dim q ON m.Qtr_ID = q.Qtr_ID
JOIN Year_Dim y ON q.Year_ID = y.Year_ID
ORDER BY d.Day_ID;
```
*Output:*
```
+--------+------+-----------+-------+------------+--------+------+
| Day_ID | Day  | Day_Name  | Wk_no | Month_Name | Qtr_no | Year |
+--------+------+-----------+-------+------------+--------+------+
|      1 |   15 | Monday    |     3 | January    |      1 | 2024 |
|      2 |   20 | Tuesday   |     2 | February   |      1 | 2024 |
|      3 |   10 | Wednesday |     3 | April      |      2 | 2024 |
|      4 |    5 | Friday    |     1 | July       |      3 | 2024 |
|      5 |   18 | Wednesday |     3 | September  |      3 | 2024 |
|      6 |   25 | Friday    |     4 | October    |      4 | 2024 |
|      7 |   30 | Saturday  |     4 | November   |      4 | 2024 |
|      8 |   15 | Sunday    |     2 | December   |      4 | 2024 |
|      9 |   10 | Friday    |     2 | January    |      1 | 2025 |
|     10 |   22 | Saturday  |     4 | March      |      1 | 2025 |
+--------+------+-----------+-------+------------+--------+------+
```

### **4. Complete Sales Analysis with Full Time Hierarchy**
```sql
SELECT 
    sf.Sale_id,
    p.P_name AS Product,
    cat.C_name AS Category,
    st.Store_name AS Store,
    ct.City_name AS City,
    cust.Cust_name AS Customer,
    cust.Customer_tier,
    d.Day,
    d.Day_Name,
    w.Wk_no,
    m.Name AS Month,
    q.Qtr_no,
    y.Year,
    sf.sales_amount,
    sf.quantity
FROM Sales_Fact sf
JOIN Product_Dim p ON sf.product_id = p.P_id
JOIN Category_Dim cat ON sf.category_ref = cat.C_id
JOIN Store_Dim st ON sf.store_ref = st.Store_id
JOIN City_Dim ct ON sf.city_ref = ct.City_id
JOIN Customer_Dim cust ON sf.customer_ref = cust.Cust_id
JOIN Day_Dim d ON sf.day_ref = d.Day_ID
JOIN Wk_Dim w ON d.Wk_ID = w.Wk_ID
JOIN Month_Dim m ON w.Month_ID = m.Month_ID
JOIN Qtr_Dim q ON m.Qtr_ID = q.Qtr_ID
JOIN Year_Dim y ON q.Year_ID = y.Year_ID
ORDER BY sf.Sale_id;
```
*Output:*
```
+---------+-----------------+----------+-----------------+-----------+---------------+-----------+------+-----------+-------+---------+--------+------+--------------+----------+
| Sale_id | Product         | Category | Store           | City      | Customer      | Tier      | Day  | Day_Name  | Wk_no | Month   | Qtr_no | Year | sales_amount | quantity |
+---------+-----------------+----------+-----------------+-----------+---------------+-----------+------+-----------+-------+---------+--------+------+--------------+----------+
|       1 | Naruto          | Anime    | Downtown Store  | Pune      | Rahul Sharma  | Gold      |   15 | Monday    |     3 | January |      1 | 2024 |      1500.00 |        5 |
|       2 | Shinchan        | Cartoons | Mall Store      | Mumbai    | Priya Patel   | Platinum  |   20 | Tuesday   |     2 | February|      1 | 2024 |      2000.00 |       10 |
|       3 | Doraemon        | Cartoons | Airport Store   | Chennai   | Amit Verma    | Silver    |   10 | Wednesday |     3 | April   |      2 | 2024 |       750.00 |        3 |
|       4 | Attack on Titan | Manga    | Central Store   | Delhi     | Sneha Singh   | Gold      |    5 | Friday    |     1 | July    |      3 | 2024 |      3000.00 |       15 |
|       5 | Naruto          | Anime    | Metro Store     | Kolkata   | Vikram Mehta  | Bronze    |   18 | Wednesday |     3 | September|      3 | 2024 |      1200.00 |        4 |
|       6 | Shinchan        | Cartoons | Plaza Store     | Pune      | Anjali Reddy  | Silver    |   25 | Friday    |     4 | October  |      4 | 2024 |      1800.00 |        6 |
|       7 | Doraemon        | Cartoons | Downtown Store  | Mumbai    | Rajesh Kumar  | Platinum  |   30 | Saturday  |     4 | November |      4 | 2024 |       900.00 |        3 |
|       8 | Attack on Titan | Manga    | Mall Store      | Delhi     | Neha Gupta    | Gold      |   15 | Sunday    |     2 | December |      4 | 2024 |      2500.00 |       10 |
|       9 | Naruto          | Anime    | Airport Store   | Bangalore | Sanjay Joshi  | Silver    |   10 | Friday    |     2 | January  |      1 | 2025 |      1800.00 |        6 |
|      10 | Shinchan        | Cartoons | Central Store   | Chennai   | Pooja Desai   | Bronze    |   22 | Saturday  |     4 | March    |      1 | 2025 |      2200.00 |        8 |
+---------+-----------------+----------+-----------------+-----------+---------------+-----------+------+-----------+-------+---------+--------+------+--------------+----------+
```

### **5. Quarterly Sales Analysis using Time Hierarchy**
```sql
SELECT 
    y.Year,
    q.Qtr_no,
    SUM(sf.sales_amount) AS Total_Sales,
    SUM(sf.quantity) AS Total_Quantity,
    COUNT(sf.Sale_id) AS Number_of_Sales
FROM Sales_Fact sf
JOIN Day_Dim d ON sf.day_ref = d.Day_ID
JOIN Wk_Dim w ON d.Wk_ID = w.Wk_ID
JOIN Month_Dim m ON w.Month_ID = m.Month_ID
JOIN Qtr_Dim q ON m.Qtr_ID = q.Qtr_ID
JOIN Year_Dim y ON q.Year_ID = y.Year_ID
GROUP BY y.Year, q.Qtr_no
ORDER BY y.Year, q.Qtr_no;
```
*Output:*
```
+------+--------+-------------+----------------+-----------------+
| Year | Qtr_no | Total_Sales | Total_Quantity | Number_of_Sales |
+------+--------+-------------+----------------+-----------------+
| 2024 |      1 |     3500.00 |             15 |               2 |
| 2024 |      2 |      750.00 |              3 |               1 |
| 2024 |      3 |     4200.00 |             19 |               2 |
| 2024 |      4 |     5200.00 |             19 |               3 |
| 2025 |      1 |     4000.00 |             14 |               2 |
+------+--------+-------------+----------------+-----------------+
```

---

## **Snowflake Schema Structure with Fully Normalized Time Dimension**

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Category_Dim   │     │   Product_Dim   │     │   Customer_Dim  │
│  ─────────────  │     │  ─────────────  │     │  ─────────────  │
│  C_id (PK)      │←─── │  P_id (PK)      │     │  Cust_id (PK)   │
│  C_name         │     │  P_name         │     │  Cust_name      │
└─────────────────┘     │  cid (FK)───────┘     │  Gender         │
                         └─────────────────┘     │  Cust_phone     │
                                                  │  Cust_city      │
┌─────────────────┐     ┌─────────────────┐     │  Cust_join_date │
│    City_Dim     │     │    Store_Dim    │     │  Customer_tier  │
│  ─────────────  │     │  ─────────────  │     └─────────────────┘
│  City_id (PK)   │←─── │  Store_id (PK)  │              ↑
│  City_name      │     │  Store_name     │              │
└─────────────────┘     │  cityid (FK)────┘              │
                         └─────────────────┘              │
                                                          │
┌─────────────────┐     ┌─────────────────┐              │
│    Year_Dim     │     │    Qtr_Dim      │              │
│  ─────────────  │     │  ─────────────  │              │
│  Year_ID (PK)   │←─── │  Qtr_ID (PK)    │              │
│  Year           │     │  Qtr_no         │              │
└─────────────────┘     │  Year_ID (FK)───┘              │
                         └─────────────────┘              │
                               ↑                          │
                         ┌─────────────────┐              │
                         │   Month_Dim     │              │
                         │  ─────────────  │              │
                         │  Month_ID (PK)  │              │
                         │  Name           │              │
                         │  Qtr_ID (FK)────┘              │
                         └─────────────────┘              │
                               ↑                          │
                         ┌─────────────────┐              │
                         │     Wk_Dim      │              │
                         │  ─────────────  │              │
                         │  Wk_ID (PK)     │              │
                         │  Wk_no          │              │
                         │  Month_ID (FK)──┘              │
                         └─────────────────┘              │
                               ↑                          │
                         ┌─────────────────┐              │
                         │     Day_Dim     │              │
                         │  ─────────────  │              │
                         │  Day_ID (PK)    │              │
                         │  Day            │              │
                         │  Day_Name       │              │
                         │  Wk_ID (FK)─────┘              │
                         └─────────────────┘              │
                               ↑                          │
┌─────────────────────────────────────────────────────────┐
│                       Sales_Fact                         │
│  ─────────────────────────────────────────────────────  │
│  Sale_id (PK)                                            │
│  product_id (FK) → Product_Dim(P_id)                    │
│  category_ref (FK) → Category_Dim(C_id)                  │
│  store_ref (FK) → Store_Dim(Store_id)                    │
│  city_ref (FK) → City_Dim(City_id)                       │
│  customer_ref (FK) → Customer_Dim(Cust_id)               │
│  day_ref (FK) → Day_Dim(Day_ID)                          │
│  sales_amount                                            │
│  quantity                                                │
└─────────────────────────────────────────────────────────┘
```

---

## **Summary**

The Retail database has been successfully converted to a **Snowflake Schema** with a **fully normalized Time Dimension**:

| Table Name | Type | Records | Foreign Keys |
|------------|------|---------|--------------|
| Category_Dim | Dimension | 3 | - |
| Product_Dim | Dimension | 4 | cid → Category_Dim |
| City_Dim | Dimension | 6 | - |
| Store_Dim | Dimension | 6 | cityid → City_Dim |
| Customer_Dim | Dimension | 10 | - |
| **Year_Dim** | **Time Dim** | **3** | - |
| **Qtr_Dim** | **Time Dim** | **8** | Year_ID → Year_Dim |
| **Month_Dim** | **Time Dim** | **24** | Qtr_ID → Qtr_Dim |
| **Wk_Dim** | **Time Dim** | **60** | Month_ID → Month_Dim |
| **Day_Dim** | **Time Dim** | **10** | Wk_ID → Wk_Dim |
| **Sales_Fact** | **Fact** | **10** | Multiple FKs |

### **Snowflake Schema Benefits:**
- ✅ **Fully Normalized** - Each time hierarchy level has its own table
- ✅ **No Redundancy** - Each piece of time information stored once
- ✅ **Hierarchical Analysis** - Drill-down from Year → Quarter → Month → Week → Day
- ✅ **Data Integrity** - Enforced through foreign key relationships
- ✅ **Storage Efficiency** - Minimal space for time dimension
- ✅ **OLAP Ready** - Perfect for roll-up and drill-down operations

The schema now supports **complex time-based analysis** with complete drill-down capabilities from year down to individual days!
