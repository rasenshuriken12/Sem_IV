# 1️⃣ Basic SQL Syntax Rules (Non-negotiable)

### ✔ Statement ends with `;`

```sql
SELECT * FROM Sales_Fact;
```

### ✔ Keywords are case-insensitive

```sql
SELECT, select, Select  -- all valid
```

### ✔ Strings use single quotes

```sql
WHERE Region = 'North'
```

### ✔ Aliases (temporary names)

```sql
SELECT SUM(Sales_Amount) AS Total_Sales
```

---

# 2️⃣ Constraints Rules (Data Integrity)

### ✔ PRIMARY KEY

* Unique
* Not NULL

```sql
Product_ID INT PRIMARY KEY
```

### ✔ FOREIGN KEY

* Maintains relationship

```sql
FOREIGN KEY (Product_ID) REFERENCES Product_Dim(Product_ID)
```

### ✔ NOT NULL

```sql
Customer_Name VARCHAR(50) NOT NULL
```

### ✔ UNIQUE

```sql
Email VARCHAR(100) UNIQUE
```

---

# 3️⃣ Query Order Rule (Most Important)

### Logical Execution Order:

```text
FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```

### Writing Order:

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

---

# 4️⃣ JOIN Rules

```sql
✅ Correct

FROM Sales_Fact s
JOIN Customer_Dim c 
ON s.Customer_ID = c.Customer_ID
```

```sql
❌ Dangerous (Cartesian Product)

FROM Sales_Fact, Customer_Dim
```

### Types of JOINs

| Type       | Meaning                  |
| ---------- | ------------------------ |
| INNER JOIN | Matching rows only       |
| LEFT JOIN  | All left + matched right |
| RIGHT JOIN | All right + matched left |

---

# 5️⃣ NULL Rules (Very Important)

```sql
❌ Wrong

WHERE Region = NULL
```

```sql
✅ Correct
 
WHERE Region IS NULL
```

---

# 6️⃣ WHERE vs HAVING Rule

| Clause | Use                  |
| ------ | -------------------- |
| WHERE  | Filters rows         |
| HAVING | Filters grouped data |

```sql
❌ Wrong

WHERE SUM(Sales_Amount) > 10000 
```

```sql
✅ Correct

HAVING SUM(Sales_Amount) > 10000 
```

---

# 7️⃣ GROUP BY Rules 

> ❗ Rule:
> - Every non-aggregated column in SELECT must be in GROUP BY

```sql
❌ Wrong

SELECT Region, SUM(Sales_Amount)
FROM Sales_Fact;
```

```sql
✅ Correct

SELECT Region, SUM(Sales_Amount)
FROM Sales_Fact
GROUP BY Region;
```

---

# 8️⃣ Aggregate Function Rules

| Function | Meaning        |
| -------- | -------------- |
| SUM()    | Total          |
| AVG()    | Average        |
| COUNT()  | Number of rows |
| MAX()    | Highest        |
| MIN()    | Lowest         |

---

# 9️⃣ ORDER BY Rules

```sql
ORDER BY Total_Sales DESC
```

* `ASC` → ascending (default)
* `DESC` → descending

---

# 🔟 LIMIT Rule (Top N Queries)

```sql
LIMIT 5
```

Used for:

* Top 5 products
* Top 10 regions

---

# 1️⃣1️⃣ Real Engineering Rules 

### ✔ Always use aliases

```sql
Sales_Fact s
Customer_Dim c
```

### ✔ Avoid SELECT *

```sql
SELECT *  -- bad for production
```

Use:

```sql
SELECT Product_ID, Sales_Amount
```

### ✔ Index matters (Performance)

Columns used in:

* JOIN
* WHERE
* GROUP BY

should be indexed.

---
