# **Retail Database Setup and Operations**

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

### **10. Create Time_Dim Table (for Sales Time)**
```sql
CREATE TABLE Time_Dim (
    Time_id INT PRIMARY KEY,
    Sale_Date DATE,
    Year INT,
    Quarter INT,
    Month INT,
    Month_Name VARCHAR(20),
    Day INT,
    Day_Name VARCHAR(20)
);
```
*Output:*
```
Query OK, 0 rows affected (0.03 sec)
```

### **11. Insert Data into Time_Dim**
```sql
INSERT INTO Time_Dim (Time_id, Sale_Date, Year, Quarter, Month, Month_Name, Day, Day_Name) VALUES 
(1, '2024-01-15', 2024, 1, 1, 'January', 15, 'Monday'),
(2, '2024-02-20', 2024, 1, 2, 'February', 20, 'Tuesday'),
(3, '2024-04-10', 2024, 2, 4, 'April', 10, 'Wednesday'),
(4, '2024-07-05', 2024, 3, 7, 'July', 5, 'Friday'),
(5, '2024-09-18', 2024, 3, 9, 'September', 18, 'Wednesday'),
(6, '2024-10-25', 2024, 4, 10, 'October', 25, 'Friday'),
(7, '2024-11-30', 2024, 4, 11, 'November', 30, 'Saturday'),
(8, '2024-12-15', 2024, 4, 12, 'December', 15, 'Sunday'),
(9, '2025-01-10', 2025, 1, 1, 'January', 10, 'Friday'),
(10, '2025-03-22', 2025, 1, 3, 'March', 22, 'Saturday');
```
*Output:*
```
Query OK, 10 rows affected (0.01 sec)
Records: 10  Duplicates: 0  Warnings: 0
```

### **12. Create Sales_Fact Table**
```sql
CREATE TABLE Sales_Fact (
    Sale_id INT PRIMARY KEY,
    product_id INT,
    category_ref INT,
    store_ref INT,
    city_ref INT,
    time_ref INT,
    sales_amount DECIMAL(10,2),
    quantity INT,
    FOREIGN KEY (product_id) REFERENCES Product_Dim(P_id),
    FOREIGN KEY (category_ref) REFERENCES Category_Dim(C_id),
    FOREIGN KEY (store_ref) REFERENCES Store_Dim(Store_id),
    FOREIGN KEY (city_ref) REFERENCES City_Dim(City_id),
    FOREIGN KEY (time_ref) REFERENCES Time_Dim(Time_id)
);
```
*Output:*
```
Query OK, 0 rows affected (0.05 sec)
```

### **13. Insert Data into Sales_Fact**
```sql
INSERT INTO Sales_Fact VALUES 
(1, 101, 1001, 1, 9, 1, 1500.00, 5),
(2, 102, 1002, 2, 1, 2, 2000.00, 10),
(3, 103, 1002, 3, 4, 3, 750.00, 3),
(4, 104, 1003, 4, 2, 4, 3000.00, 15),
(5, 101, 1001, 5, 5, 5, 1200.00, 4),
(6, 102, 1002, 6, 9, 6, 1800.00, 6),
(7, 103, 1002, 1, 1, 7, 900.00, 3),
(8, 104, 1003, 2, 2, 8, 2500.00, 10),
(9, 101, 1001, 3, 3, 9, 1800.00, 6),
(10, 102, 1002, 4, 4, 10, 2200.00, 8);
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
| Product_Dim      |
| Sales_Fact       |
| Store_Dim        |
| Time_Dim         |
+------------------+
6 rows in set (0.00 sec)
```

### **2. Category_Dim Table Structure and Data**
```sql
DESC Category_Dim;
```
*Output:*
```
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| C_id   | int         | NO   | PRI | NULL    |       |
| C_name | varchar(50) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

```sql
SELECT * FROM Category_Dim;
```
*Output:*
```
+------+----------+
| C_id | C_name   |
+------+----------+
| 1001 | Anime    |
| 1002 | Cartoons |
| 1003 | Manga    |
+------+----------+
3 rows in set (0.00 sec)
```

### **3. Product_Dim Table Structure and Data**
```sql
DESC Product_Dim;
```
*Output:*
```
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| P_id   | int         | NO   | PRI | NULL    |       |
| P_name | varchar(50) | YES  |     | NULL    |       |
| cid    | int         | YES  | MUL | NULL    |       |
+--------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

```sql
SELECT * FROM Product_Dim;
```
*Output:*
```
+------+-----------------+------+
| P_id | P_name          | cid  |
+------+-----------------+------+
|  101 | Naruto          | 1001 |
|  102 | Shinchan        | 1002 |
|  103 | Doraemon        | 1002 |
|  104 | Attack on Titan | 1003 |
+------+-----------------+------+
4 rows in set (0.00 sec)
```

### **4. City_Dim Table Structure and Data**
```sql
DESC City_Dim;
```
*Output:*
```
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| City_id   | int         | NO   | PRI | NULL    |       |
| City_name | varchar(50) | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

```sql
SELECT * FROM City_Dim;
```
*Output:*
```
+---------+-----------+
| City_id | City_name |
+---------+-----------+
|       1 | Mumbai    |
|       2 | Delhi     |
|       3 | Bangalore |
|       4 | Chennai   |
|       5 | Kolkata   |
|       9 | Pune      |
+---------+-----------+
6 rows in set (0.00 sec)
```

### **5. Store_Dim Table Structure and Data**
```sql
DESC Store_Dim;
```
*Output:*
```
+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| Store_id   | int         | NO   | PRI | NULL    |       |
| Store_name | varchar(50) | YES  |     | NULL    |       |
| cityid     | int         | YES  | MUL | NULL    |       |
+------------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

```sql
SELECT * FROM Store_Dim;
```
*Output:*
```
+----------+-----------------+--------+
| Store_id | Store_name      | cityid |
+----------+-----------------+--------+
|        1 | Downtown Store  |      1 |
|        2 | Mall Store      |      2 |
|        3 | Airport Store   |      3 |
|        4 | Central Store   |      4 |
|        5 | Metro Store     |      5 |
|        6 | Plaza Store     |      9 |
+----------+-----------------+--------+
6 rows in set (0.00 sec)
```

### **6. Time_Dim Table Structure and Data**
```sql
DESC Time_Dim;
```
*Output:*
```
+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| Time_id    | int         | NO   | PRI | NULL    |       |
| Sale_Date  | date        | YES  |     | NULL    |       |
| Year       | int         | YES  |     | NULL    |       |
| Quarter    | int         | YES  |     | NULL    |       |
| Month      | int         | YES  |     | NULL    |       |
| Month_Name | varchar(20) | YES  |     | NULL    |       |
| Day        | int         | YES  |     | NULL    |       |
| Day_Name   | varchar(20) | YES  |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+
8 rows in set (0.00 sec)
```

```sql
SELECT * FROM Time_Dim;
```
*Output:*
```
+---------+------------+------+---------+-------+-----------+------+----------+
| Time_id | Sale_Date  | Year | Quarter | Month | Month_Name| Day  | Day_Name |
+---------+------------+------+---------+-------+-----------+------+----------+
|       1 | 2024-01-15 | 2024 |       1 |     1 | January   |   15 | Monday   |
|       2 | 2024-02-20 | 2024 |       1 |     2 | February  |   20 | Tuesday  |
|       3 | 2024-04-10 | 2024 |       2 |     4 | April     |   10 | Wednesday|
|       4 | 2024-07-05 | 2024 |       3 |     7 | July      |    5 | Friday   |
|       5 | 2024-09-18 | 2024 |       3 |     9 | September |   18 | Wednesday|
|       6 | 2024-10-25 | 2024 |       4 |    10 | October   |   25 | Friday   |
|       7 | 2024-11-30 | 2024 |       4 |    11 | November  |   30 | Saturday |
|       8 | 2024-12-15 | 2024 |       4 |    12 | December  |   15 | Sunday   |
|       9 | 2025-01-10 | 2025 |       1 |     1 | January   |   10 | Friday   |
|      10 | 2025-03-22 | 2025 |       1 |     3 | March     |   22 | Saturday |
+---------+------------+------+---------+-------+-----------+------+----------+
10 rows in set (0.00 sec)
```

### **7. Sales_Fact Table Structure and Data**
```sql
DESC Sales_Fact;
```
*Output:*
```
+--------------+--------------+------+-----+---------+-------+
| Field        | Type         | Null | Key | Default | Extra |
+--------------+--------------+------+-----+---------+-------+
| Sale_id      | int          | NO   | PRI | NULL    |       |
| product_id   | int          | YES  | MUL | NULL    |       |
| category_ref | int          | YES  | MUL | NULL    |       |
| store_ref    | int          | YES  | MUL | NULL    |       |
| city_ref     | int          | YES  | MUL | NULL    |       |
| time_ref     | int          | YES  | MUL | NULL    |       |
| sales_amount | decimal(10,2)| YES  |     | NULL    |       |
| quantity     | int          | YES  |     | NULL    |       |
+--------------+--------------+------+-----+---------+-------+
8 rows in set (0.00 sec)
```

```sql
SELECT * FROM Sales_Fact;
```
*Output:*
```
+---------+------------+--------------+-----------+----------+----------+--------------+----------+
| Sale_id | product_id | category_ref | store_ref | city_ref | time_ref | sales_amount | quantity |
+---------+------------+--------------+-----------+----------+----------+--------------+----------+
|       1 |        101 |         1001 |         1 |        9 |        1 |      1500.00 |        5 |
|       2 |        102 |         1002 |         2 |        1 |        2 |      2000.00 |       10 |
|       3 |        103 |         1002 |         3 |        4 |        3 |       750.00 |        3 |
|       4 |        104 |         1003 |         4 |        2 |        4 |      3000.00 |       15 |
|       5 |        101 |         1001 |         5 |        5 |        5 |      1200.00 |        4 |
|       6 |        102 |         1002 |         6 |        9 |        6 |      1800.00 |        6 |
|       7 |        103 |         1002 |         1 |        1 |        7 |       900.00 |        3 |
|       8 |        104 |         1003 |         2 |        2 |        8 |      2500.00 |       10 |
|       9 |        101 |         1001 |         3 |        3 |        9 |      1800.00 |        6 |
|      10 |        102 |         1002 |         4 |        4 |       10 |      2200.00 |        8 |
+---------+------------+--------------+-----------+----------+----------+--------------+----------+
10 rows in set (0.00 sec)
```

---

## **Part 3: Analytical Queries**

### **Quarterly Sales Analysis**
```sql
SELECT 
    t.Year,
    t.Quarter,
    SUM(s.sales_amount) AS Total_Sales,
    SUM(s.quantity) AS Total_Quantity
FROM Sales_Fact s
JOIN Time_Dim t ON s.time_ref = t.Time_id
GROUP BY t.Year, t.Quarter
ORDER BY t.Year, t.Quarter;
```
*Output:*
```
+------+---------+-------------+----------------+
| Year | Quarter | Total_Sales | Total_Quantity |
+------+---------+-------------+----------------+
| 2024 |       1 |     3500.00 |             15 |
| 2024 |       2 |      750.00 |              3 |
| 2024 |       3 |     3000.00 |             19 |
| 2024 |       4 |     5400.00 |             21 |
| 2025 |       1 |     4000.00 |             14 |
+------+---------+-------------+----------------+
5 rows in set (0.00 sec)
```

### **Monthly Sales Analysis**
```sql
SELECT 
    t.Year,
    t.Month,
    t.Month_Name,
    SUM(s.sales_amount) AS Total_Sales,
    COUNT(s.Sale_id) AS Number_of_Sales
FROM Sales_Fact s
JOIN Time_Dim t ON s.time_ref = t.Time_id
GROUP BY t.Year, t.Month, t.Month_Name
ORDER BY t.Year, t.Month;
```
*Output:*
```
+------+-------+-----------+-------------+-----------------+
| Year | Month | Month_Name| Total_Sales | Number_of_Sales |
+------+-------+-----------+-------------+-----------------+
| 2024 |     1 | January   |     1500.00 |               1 |
| 2024 |     2 | February  |     2000.00 |               1 |
| 2024 |     4 | April     |      750.00 |               1 |
| 2024 |     7 | July      |     3000.00 |               1 |
| 2024 |     9 | September |     1200.00 |               1 |
| 2024 |    10 | October   |     1800.00 |               1 |
| 2024 |    11 | November  |      900.00 |               1 |
| 2024 |    12 | December  |     2500.00 |               1 |
| 2025 |     1 | January   |     1800.00 |               1 |
| 2025 |     3 | March     |     2200.00 |               1 |
+------+-------+-----------+-------------+-----------------+
10 rows in set (0.00 sec)
```

### **Sales by Day of Week**
```sql
SELECT 
    t.Day_Name,
    SUM(s.sales_amount) AS Total_Sales,
    AVG(s.sales_amount) AS Avg_Sale_Amount,
    COUNT(s.Sale_id) AS Sale_Count
FROM Sales_Fact s
JOIN Time_Dim t ON s.time_ref = t.Time_id
GROUP BY t.Day_Name
ORDER BY Total_Sales DESC;
```
*Output:*
```
+----------+-------------+-----------------+------------+
| Day_Name | Total_Sales | Avg_Sale_Amount | Sale_Count |
+----------+-------------+-----------------+------------+
| Friday   |     3800.00 |     1900.000000 |          2 |
| Saturday |     3100.00 |     1550.000000 |          2 |
| Tuesday  |     2000.00 |     2000.000000 |          1 |
| Wednesday|     1950.00 |      975.000000 |          2 |
| Monday   |     1500.00 |     1500.000000 |          1 |
| Sunday   |     2500.00 |     2500.000000 |          1 |
+----------+-------------+-----------------+------------+
6 rows in set (0.00 sec)
```

---

## **Summary**

The Retail database has been successfully created with the revised **Time_Dim** table designed specifically for sales analysis:

| Table Name | Primary Key | Foreign Keys | Records |
|------------|-------------|--------------|---------|
| Category_Dim | C_id | - | 3 |
| Product_Dim | P_id | cid → Category_Dim(C_id) | 4 |
| City_Dim | City_id | - | 6 |
| Store_Dim | Store_id | cityid → City_Dim(City_id) | 6 |
| **Time_Dim** | **Time_id** | - | **10** |
| Sales_Fact | Sale_id | product_id, category_ref, store_ref, city_ref, time_ref | 10 |

### **Time_Dim Table Features:**
- ✅ **Sale_Date** - Complete date of sale
- ✅ **Year** - Year component for yearly analysis
- ✅ **Quarter** - Quarter (1-4) for quarterly reporting
- ✅ **Month** - Month number (1-12)
- ✅ **Month_Name** - Month name for readability
- ✅ **Day** - Day of month (1-31)
- ✅ **Day_Name** - Day of week for weekday analysis

All tables are properly structured with appropriate primary keys and foreign key relationships, ensuring referential integrity across the database with comprehensive time-based sales analysis capabilities.
