```sql
# Query Template

SELECT column_list / aggregations
FROM table1
JOIN table2 ON condition
WHERE row_conditions
GROUP BY columns
HAVING group_conditions
ORDER BY columns
LIMIT number;
```

```sql
# Top 3 regions with highest sales in 2024

SELECT 
    c.Region,
    SUM(s.Sales_Amount) AS Total_Sales
FROM Sales_Fact s
JOIN Customer_Dim c 
ON s.Customer_ID = c.Customer_ID
JOIN Time_Dim t 
ON s.Time_ID = t.Time_ID
WHERE t.Year = 2024
GROUP BY c.Region
HAVING SUM(s.Sales_Amount) > 10000
ORDER BY Total_Sales DESC
LIMIT 3;
```

### ⚠️ Common Mistakes 

1. ❌ Using aggregate in WHERE

```sql 
WHERE SUM(Sales_Amount) > 10000   ❌

HAVING SUM(Sales_Amount) > 10000  ✅ 


2. ❌ Missing GROUP BY

```sql 
SELECT Region, SUM(Sales_Amount) 
...
GROUP BY Region    # include ✅           
```
