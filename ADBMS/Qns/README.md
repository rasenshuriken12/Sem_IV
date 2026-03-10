# **Star Schema vs Snowflake Schema**

## **🌟 Star Schema**

### **Definition:**
A Star Schema has a central **fact table** surrounded by **denormalized dimension tables** directly connected to it, forming a star-like shape.

### **Structure:**
```
                    ┌─────────────┐
                    │ Product_Dim │
                    └─────────────┘
                           ↑
                           │
┌──────────────┐    ┌─────────────┐    ┌─────────────┐
│ Customer_Dim │←──→│  Sales_Fact │←──→│  Store_Dim  │
└──────────────┘    └─────────────┘    └─────────────┘
                           ↑
                           │
                     ┌────────────┐
                     │  Time_Dim  │
                     └────────────┘
```

### **Example (Star Schema):**
```
Product_Dim (denormalized)
- P_id, P_name, Category_id, Category_name

Store_Dim (denormalized)
- Store_id, Store_name, City_id, City_name, State_id, State_name

Customer_Dim (denormalized)
- Cust_id, Cust_name, City_id, City_name, Tier_id, Tier_name

Time_Dim
- Time_id, Date, Year, Quarter, Month

Sales_Fact
- Sale_id, product_ref, store_ref, customer_ref, time_ref, amount
```

```sql
-- Product Dimension (Denormalized)
CREATE TABLE Product_Dim (
    P_id INT PRIMARY KEY,
    P_name VARCHAR(50),
    Category_id INT,
    Category_name VARCHAR(50)  -- Denormalized: Category info stored here
);

-- Store Dimension (Denormalized)
CREATE TABLE Store_Dim (
    Store_id INT PRIMARY KEY,
    Store_name VARCHAR(50),
    City_id INT,
    City_name VARCHAR(50),      -- Denormalized: City info stored here
    State_name VARCHAR(50)      -- Denormalized: State info stored here
);

-- Customer Dimension
CREATE TABLE Customer_Dim (
    Cust_id INT PRIMARY KEY,
    Cust_name VARCHAR(50),
    Gender VARCHAR(10),
    Cust_phone VARCHAR(15),
    Cust_city VARCHAR(50),
);

-- Time Dimension
CREATE TABLE Time_Dim (
    Time_id INT PRIMARY KEY,
    Sale_Date DATE,
    Year INT,
    Quarter INT,
    Month INT,
);

-- Fact Table
CREATE TABLE Sales_Fact (
    Sale_id INT PRIMARY KEY,
    product_ref INT,
    store_ref INT,
    customer_ref INT,
    time_ref INT,
    sales_amount DECIMAL(10,2),
    quantity INT,
    FOREIGN KEY (product_ref) REFERENCES Product_Dim(P_id),
    FOREIGN KEY (store_ref) REFERENCES Store_Dim(Store_id),
    FOREIGN KEY (customer_ref) REFERENCES Customer_Dim(Cust_id),
    FOREIGN KEY (time_ref) REFERENCES Time_Dim(Time_id)
);
```

---

## **❄️ Snowflake Schema**

### **Definition:**
A Snowflake Schema is an extension of star schema where dimension tables are **normalized** into multiple related tables, forming a snowflake-like pattern.

### **Structure:**
```

                    ┌──────────────┐
                    │ Category_Dim │
                    └──────────────┘
                           ↑
                           │
                    ┌─────────────┐
                    │ Product_Dim │
                    └─────────────┘
                           ↑
                           │
┌──────────────┐    ┌─────────────┐    ┌─────────────┐
│ Customer_Dim │←──→│  Sales_Fact │←──→│  Store_Dim  │
└──────────────┘    └─────────────┘    └─────────────┘
                           ↑                  ↑
                           │                  │
                    ┌─────────────┐    ┌─────────────┐
                    │     Time    │    │    City     │
                    └─────────────┘    └─────────────┘
                                              ↑
                                              │
                                       ┌─────────────┐
                                       │    State    │
                                       └─────────────┘
```

### **Example (Snowflake Schema):**
```
Category_Dim
- C_id, C_name

Product_Dim
- P_id, P_name, cid → Category_Dim

City_Dim
- City_id, City_name

Store_Dim
- Store_id, Store_name, cityid → City_Dim

Customer_Dim
- Cust_id, Cust_name, Cust_city (redundant but acceptable)

Time_Dim
- Time_id, Date, Year, Quarter, Month

Sales_Fact
- Sale_id, product_ref, store_ref, customer_ref, time_ref, amount
```

```sql
-- Category Dimension
CREATE TABLE Category_Dim (
    C_id INT PRIMARY KEY,
    C_name VARCHAR(50)
);

-- Product Dimension (Normalized - references Category)
CREATE TABLE Product_Dim (
    P_id INT PRIMARY KEY,
    P_name VARCHAR(50),
    cid INT,
    FOREIGN KEY (cid) REFERENCES Category_Dim(C_id)
);

-- State Dimension
CREATE TABLE State_Dim (
    State_id INT PRIMARY KEY,
    State_name VARCHAR(50)
);

-- City Dimension
CREATE TABLE City_Dim (
    City_id INT PRIMARY KEY,
    City_name VARCHAR(50),
    stateid INT
    FOREIGN KEY (stateid) REFERENCES City_Dim(State_id)
);

-- Store Dimension (Normalized - references City)
CREATE TABLE Store_Dim (
    Store_id INT PRIMARY KEY,
    Store_name VARCHAR(50),
    cityid INT,
    FOREIGN KEY (cityid) REFERENCES City_Dim(City_id)
);

-- Fact Table
CREATE TABLE Sales_Fact (
    Sale_id INT PRIMARY KEY,
    product_ref INT,
    store_ref INT,
    customer_ref INT,
    time_ref INT,
    sales_amount DECIMAL(10,2),
    quantity INT,
    FOREIGN KEY (product_ref) REFERENCES Product_Dim(P_id),
    FOREIGN KEY (store_ref) REFERENCES Store_Dim(Store_id),
    FOREIGN KEY (customer_ref) REFERENCES Customer_Dim(Cust_id),
    FOREIGN KEY (time_ref) REFERENCES Time_Dim(Time_id)
);
```

---

## **📋 COMPARISON TABLE**

| Aspect | Star Schema | Snowflake Schema |
|--------|-------------|------------------|
| **Structure** | Denormalized dimensions | Normalized dimensions |
| **Number of Tables** | Fewer (dimensions + fact) | More (multiple normalized tables) |
| **Join Complexity** | Simple (fact to dimension only) | Complex (multiple joins needed) |
| **Query Performance** | Faster (fewer joins) | Slower (more joins required) |
| **Data Redundancy** | High (duplicate data) | Low (each fact stored once) |
| **Data Integrity** | Risk of anomalies | Better (normalized structure) |
| **Storage Space** | More (data redundancy) | Less (normalized, no redundancy) |
| **ETL Complexity** | Simple | Complex |
| **OLAP Cube** | Better for OLAP | More complex for OLAP |

---

📌 Simple Analogy

Think of a university system.

**Star Schema** - Everything in one table.
```
Student_Dim
--------------------
Student_ID
Student_Name
Department
Department_HOD
Department_Building
```

**Snowflake Schema** - Department information stored separately.

```
Student_Dim        |   Department_Dim
----------------   |   ----------------
Student_ID         |   Department_ID
Student_Name       |   Department_Name
Department_ID      |   HOD
                   |   Building

```

---

## **🎯 QUERY COMPARISON**

### **Star Schema Query:**
```sql
-- Find total sales by category and city
SELECT 
    p.Category_name,
    s.City_name,
    SUM(f.sales_amount) AS Total_Sales
FROM Sales_Fact f
JOIN Product_Dim p ON f.product_ref = p.P_id      -- Single join to product
JOIN Store_Dim s ON f.store_ref = s.Store_id      -- Single join to store
GROUP BY p.Category_name, s.City_name;
```
**Joins:** 2 (direct to denormalized dimensions)

### **Snowflake Schema Query:**
```sql
-- Find total sales by category and city
SELECT 
    c.Category_name,
    ct.City_name,
    SUM(f.sales_amount) AS Total_Sales
FROM Sales_Fact f
JOIN Product_Dim p ON f.product_ref = p.P_id      -- Join 1
JOIN Category_Dim c ON p.cid = c.C_id             -- Join 2 (snowflake p -> c)
JOIN Store_Dim s ON f.store_ref = s.Store_id      -- Join 3
JOIN City_Dim ct ON s.cityid = ct.City_id         -- Join 4 (snowflake s -> ct)
GROUP BY c.Category_name, ct.City_name;
```
**Joins:** 4 (additional joins through normalized tables)

---

## **📈 WHEN TO USE**

### **Use Star Schema When:**
- **Query Performance** is the top priority
- **Business Users** need to understand and query directly
- **OLAP Cubes** are being built
- **Data Volume** is manageable with redundancy
- **Reporting Tools** work better with denormalized data
- **Decision:** Most data warehouses use star schema

### **Use Snowflake Schema When:**
- **Storage Space** is limited or expensive
- **Data Integrity** is critical
- **Hierarchies** are deep and complex
- **ETL Processes** can handle complexity
- **Queries are predefined** and can be optimized
- **Decision:** Rare in production; more academic/theoretical

---

**Remember:** The choice depends on your specific requirements for **performance, storage, maintenance, and query complexity**. Most real-world data warehouses use a **hybrid approach** - star schema for most dimensions, snowflake only where necessary!
