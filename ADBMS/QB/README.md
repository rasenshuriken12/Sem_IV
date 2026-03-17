Q1) Consider an Online Sales Data Warehouse with the following tables:
- Sales_Fact (Product_ID, Customer_ID, Time_ID, Store_ID, Sales_Amount, Quantity)
- Product_Dim (Product_ID, Product_Name, Category)
- Customer_Dim (Customer_ID, Customer_Name, Region)
- Time_Dim (Time_ID, Month, Quarter, Year)

Write analytical SQL queries to find:

1. Total sales by product category.

```sql
SELECT 
    p.Category,
    SUM(s.Sales_Amount) AS Total_Sales
FROM Sales_Fact s
JOIN Product_Dim p 
ON s.Product_ID = p.Product_ID
GROUP BY p.Category;
```

2. Quarterly sales performance.

```sql
SELECT 
    t.Year,                  👁️‍🗨️
    t.Quarter,
    SUM(s.Sales_Amount) AS Total_Quarterly_Sales
FROM Sales_Fact s
JOIN Time_Dim t 
ON s.Time_ID = t.Time_ID
GROUP BY t.Year, t.Quarter
ORDER BY t.Year, t.Quarter;  👁️‍🗨️
```

3. Region-wise revenue analysis.

```sql
SELECT 
    c.Region,
    SUM(s.Sales_Amount) AS Total_Revenue
FROM Sales_Fact s
JOIN Customer_Dim c 
ON s.Customer_ID = c.Customer_ID
GROUP BY c.Region
ORDER BY Total_Revenue DESC;    👁️‍🗨️
```

4. Average sales per customer.

```sql
SELECT 
    c.Customer_ID,
    c.Customer_Name,             👁️‍🗨️
    AVG(s.Sales_Amount) AS Avg_Sales
FROM Sales_Fact s
JOIN Customer_Dim c 
ON s.Customer_ID = c.Customer_ID
GROUP BY c.Customer_ID, c.Customer_Name;
```

Q2) Explain how OLAP operations help analyze business data in a data warehouse. Illustrate the
following operations with examples from an Online Sales cube:

### Product_Dim
| Product_ID | Product_Name | Category |
|------------|-------------|----------|
| 101 | Laptop | Electronics |
| 102 | Mouse | Accessories |
| 103 | Keyboard | Accessories |

### Customer_Dim
| Customer_ID | Customer_Name | Region |
|-------------|---------------|--------|
| 1001 | Alice | North |
| 1002 | Bob | North |
| 1003 | Charlie | South |

### Time_Dim
| Time_ID | Month | Quarter | Year |
|---------|-------|---------|------|
| 1 | Jan | 1 | 2023 |
| 2 | May | 2 | 2023 |
| 3 | Apr | 2 | 2024 |

### Sales_Fact
| Product_ID | Customer_ID | Time_ID | Store_ID | Sales_Amount | Quantity |
|------------|-------------|---------|----------|--------------|----------|
| 104 | 1003 | 1 | 201 | 407.09 | 2 |
| 105 | 1002 | 2 | 201 | 882.29 | 4 |
| 103 | 1002 | 3 | 201 | 774.13 | 4 |

1. Roll-up → Summary view
- OLAP system summarizes the data for specific dimension/attributes. It shows less-detailed data. 

Eg. You want to see Total Sales by Year instead of by Month.

Cube Action: Aggregating Month → Quarter → Year

```sql
SELECT t.Year, SUM(f.Sales_Amount)
FROM Sales_Fact f
JOIN Time_Dim t ON f.Time_ID = t.Time_ID
GROUP BY t.Year;    --  Rolling up from Month/Day to Year
```

| Year | Sales_Amount |
|------|-------|
| 2023 | 1289.38 |
| 2024 | 774.13 |


Other Eg: Store -> City → State → Country

2. Drill-down → Detailed view

- Drill down is the opposite of the roll-up operation. Summarizes data at a lower level of a dimension hierarchy, thereby viewing data in a more detailed level within a dimension. 

```sql
SELECT t.Year, t.Month, SUM(f.Sales_Amount)
FROM Sales_Fact f
JOIN Time_Dim t ON f.Time_ID = t.Time_ID
WHERE t.Year = 2023
GROUP BY t.Year, t.Month;     -- Drilling down into 2023
```

| Year | Month | Sales_Amount |
|------|-------|--------------|
| 2023 | Jan | 407.09 |
| 2023 | APr | 882.29 |

3. Slice → One dimension filter
- selects a two-dimensional view from the OLAP cube, by fixing one dimension to a single value.

```sql
SELECT * FROM Sales_Fact f
JOIN Time_Dim t ON f.Time_ID = t.Time_ID
WHERE t.Year = 2023;     -- Slicing the cube by Year
```

| Product_ID | Customer_ID | Time_ID | Store_ID | Sales_Amount | Quantity |
|------------|-------------|---------|----------|--------------|----------|
| 104 | 1003 | 1 | 201 | 407.09 | 2 |
| 105 | 1002 | 2 | 201 | 882.29 | 4 |


4. Dice → Multi-dimension filter
- smaller subcube from an OLAP cube. selects specific values across multiple dimensions. 
- SubCube : Selected Products × Selected Locations × Selected Years

```sql
SELECT SUM(f.Sales_Amount)
FROM Sales_Fact f
JOIN Time_Dim t ON f.Time_ID = t.Time_ID
JOIN Customer_Dim c ON f.Customer_ID = c.Customer_ID
JOIN Product_Dim p ON f.Product_ID = p.Product_ID
WHERE t.Year = 2023
  AND c.Region = 'North'
  AND p.Category = 'Electronics'; -- Dicing the cube
```

5. Pivot → Change data view

- changes the dimensional orientation of the cube, allowing users to view data from different perspectives by reordering dimensions.
