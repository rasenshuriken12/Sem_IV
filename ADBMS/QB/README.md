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

Here is the consolidated list of **unique** questions from your sample paper. I have merged all repeated variations (e.g., Star Schema design, ETL processes, OLAP operations) into single, comprehensive entries so you can study efficiently without redundancy.

### **Section 1: SQL & OLAP Operations**
**Q1.** Consider an Online Sales Data Warehouse with tables: `Sales_Fact`, `Product_Dim`, `Customer_Dim`, and `Time_Dim`.
*   **Option A:** Write analytical SQL queries to find:
    1.  Total sales by product category.
    2.  Quarterly sales performance.
    3.  Region-wise revenue analysis.
    4.  Average sales per customer.
    *(Variation: Also explain how the output helps management in decision-making.)*
*   **Option B:** Explain how OLAP operations help analyze business data. Illustrate the following with examples from an Online Sales cube:
    1.  Roll-up
    2.  Drill-down
    3.  Slice
    4.  Dice
    5.  Pivot
    *(Variation: Explain how a Star Schema supports these specific OLAP operations.)*

---

### **Section 2: Data Warehouse Modeling (Star vs. Snowflake)**
**Q2.** An Online Retail Company wants to build a Data Warehouse using data from `Orders`, `Customers`, and `Products` tables.
*   **Option A:** Design a **Star Schema** for this scenario. Clearly identify:
    1.  Fact table and its attributes.
    2.  Dimension tables and their attributes.
    3.  Key attributes (Primary/Foreign Keys).
    4.  Explain how this schema supports business analytics with 3 specific examples (including 2 sample SQL queries).
*   **Option B:** Convert the above Star Schema into a **Snowflake Schema**.
    1.  Explain the normalization process used.
    2.  Discuss the advantages and disadvantages of the Snowflake schema in this specific case.
    3.  Explain situations where a Snowflake schema is preferred over a Star Schema.

---

### **Section 3: ETL Process & Challenges**
**Q3.**
*   **Option A:** Explain the **ETL (Extract, Transform, Load)** process used in Data Warehousing. Describe the activities involved in each stage with suitable examples.
    *(Variation: Design an ETL workflow for integrating data from E-commerce, Mobile App, and Offline Store sources, covering extraction, transformation, loading, and validation.)*
*   **Option B:** Describe the role of **Data Transformation** in ETL. Explain at least five common transformation techniques (e.g., cleaning, standardization, derivation) used during processing.
    *(Variation: Discuss major ETL challenges such as data quality, heterogeneous sources, missing values, consistency, and referential integrity, suggesting solutions for each.)*

---

### **Section 4: ETL Implementation, Automation & Quality**
**Q4.**
*   **Option A:** Explain how an ETL pipeline can be implemented using **Python** to:
    1.  Extract data from CSV files/APIs.
    2.  Perform transformations (remove missing values, convert dates, calculate totals).
    3.  Load cleaned data into a SQLite database.
    *(Include a simple code snippet demonstrating this process.)*
*   **Option B:** Discuss how to **automate** the ETL process for daily sales data. Your answer should cover:
    1.  Reading files/Data validation.
    2.  Loading mechanisms.
    3.  Logging and Error Handling.
    4.  Auditing and Data Reconciliation.
    5.  Scheduling the job.

---

### **Section 5: DW Architecture & Concepts**
**Q5.**
*   **Option A:** Explain the **Architecture of a Data Warehouse**. Discuss the role of:
    1.  Data Source systems.
    2.  Staging area.
    3.  Data warehouse repository.
    4.  Data marts.
    5.  Metadata Manager.
*   **Option B:** Differentiate between the following concepts (provide at least 3 differences with examples for each):
    1.  Data Warehouse vs. Traditional Database (or DBMS).
    2.  OLTP vs. OLAP systems.
    3.  SQL vs. NoSQL (or Schema vs. Schema-less Design).

---

### **Section 6: NoSQL & Schema-less Databases**
**Q6.**
*   **Option A:** What do you understand by **Schema-less database** design?
    1.  Explain the different elements of the **BASE Model**.
    2.  Differentiate between ACID and BASE models.
    3.  Explain different types of NoSQL databases (Key-Value, Document, Column-family, Graph) with examples.
*   **Option B:** Compare **Schema-less** design with Relational Schema design.
    1.  What are its advantages and limitations?
    2.  Discuss **Vertical Scaling vs. Horizontal Scaling** in the context of NoSQL/Distributed databases.
    3.  Discuss scaling, availability, performance, and latency considerations in schema-less systems.

---

### **Section 7: Advanced Analytics & Cubes**
**Q7.**
*   **Option A:** An E-commerce company wants to analyze customer behavior using a **Snowflake Schema**. Design three analytical queries to answer:
    1.  Which product categories generate the highest revenue?
    2.  Which region shows the fastest sales growth?
    3.  What is the monthly sales trend for the past year?
*   **Option B:** Explain how **OLAP Cubes** can be built from a Sales Star Schema to support multidimensional analysis. Discuss the dimensions, measures, and hierarchies involved.
