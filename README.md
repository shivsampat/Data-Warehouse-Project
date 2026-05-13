# 📊 Data Warehouse Project (SQL Server + ETL + Data Modeling)

## 📌 Overview
This project demonstrates the design and implementation of a **Data Warehouse** using **SQL Server**.  
It covers:
- **Data Modeling** (Star Schema, Snowflake Schema)
- **ETL Workflows** (Extract, Transform, Load)
- **Data Integration** from multiple sources
- **Analytics-ready tables** for reporting and BI tools

The goal is to simulate a real-world retail analytics environment where raw transactional data is transformed into structured warehouse tables for insights.

---

## 🏗️ Architecture
1. **Source Systems**
   - CSV files (Sales, Customers, Products)
   - External APIs (optional)
   - Operational Databases

2. **ETL Layer**
   - **Extract**: Load raw data from sources
   - **Transform**: Clean, validate, and standardize data
   - **Load**: Insert into staging and warehouse schemas

3. **Data Warehouse**
   - **Staging Area**: Temporary raw data storage
   - **Warehouse Schema**: Star schema with fact and dimension tables
   - **Analytics Layer**: Views for BI dashboards

---


---

## 🗄️ Data Modeling
- **Fact Table**: `FactSales`
  - Measures: Sales Amount, Quantity
  - Keys: CustomerID, ProductID, DateID

- **Dimension Tables**:
  - `DimCustomer` → Customer demographics
  - `DimProduct` → Product categories, brand
  - `DimDate` → Calendar hierarchy (day, month, quarter, year)

---

## ⚙️ ETL Workflow
1. **Extract**
   - Import CSVs into staging tables (`stg_customers`, `stg_products`, `stg_sales`).

2. **Transform**
   - Clean null values, standardize formats (dates, currency).
   - Apply business rules (e.g., map product categories).
   - Deduplicate records.

3. **Load**
   - Insert transformed data into dimension tables.
   - Populate fact tables with surrogate keys.

---

## 📊 Example Queries
```sql
-- Total Sales by Product Category
SELECT p.Category, SUM(f.SalesAmount) AS TotalSales
FROM FactSales f
JOIN DimProduct p ON f.ProductID = p.ProductID
GROUP BY p.Category;

-- Monthly Sales Trend
SELECT d.Year, d.Month, SUM(f.SalesAmount) AS MonthlySales
FROM FactSales f
JOIN DimDate d ON f.DateID = d.DateID
GROUP BY d.Year, d.Month
ORDER BY d.Year, d.Month;

