### **Sales Data Analytics Project (SQL + Power BI)**  

#### **Project Overview**  
Developed end-to-end **sales analytics solution** by:  
- Processing raw Data Warehouse (DW) data using **T-SQL** (joins, CASE, ISNULL, data cleaning)  
- Designing interactive **Power BI dashboard** with dynamic visuals (maps, trend charts, KPIs)  
- Delivering actionable insights aligned with business requirements  

#### **Technical Implementation**  

**SQL Data Processing:**  
```sql
-- Data cleaning and transformation
SELECT 
    ISNULL(s.SalesAmount, 0) AS CleanSales,
    CASE 
        WHEN s.Region = 'EU' THEN 'Europe' 
        WHEN s.Region = 'NA' THEN 'North America'
        ELSE 'Other' 
    END AS RegionGroup,
    CONCAT(d.ProductCategory, ' - ', d.ProductName) AS ProductFullName,
    s.OrderDate
FROM Fact_Sales s
LEFT JOIN Dim_Products d ON s.ProductID = d.ProductID
WHERE s.OrderDate BETWEEN '2022-01-01' AND GETDATE()
ORDER BY s.SalesAmount DESC;

-- Budget vs Actual analysis
SELECT 
    f.SalesAmount,
    b.BudgetAmount,
    (f.SalesAmount - b.BudgetAmount) AS Variance
FROM Fact_Sales f
JOIN Fact_Budget b ON f.ProductID = b.ProductID 
                   AND YEAR(f.OrderDate) = YEAR(b.BudgetDate);
