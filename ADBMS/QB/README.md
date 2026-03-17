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
