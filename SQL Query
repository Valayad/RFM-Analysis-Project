# RFM-Analysis-Project
select * from sales.SalesOrderDetail
select* from sales.SalesOrderHeader
select*from sales.Customer
------ Business challenge To Calculate RFM value....R(recency or Last Purchase Date), F(Frequency), M(monetary value), 
Customer Retention...,
Marketing Campaign optimization( To segment customers by recent purchase behaviour)----
Customer Life time Value(CLV) Estimation: To calculate CLV for each customer

select * from sales.SalesOrderDetail SOD
select* from sales.SalesOrderHeader SOH
select*from sales.Customer C
select*from sales.SalesTerritory
select * from Person.Person

SELECT  
 C.CustomerID, 
 MAX(SOH.OrderDate) AS Recency,
COUNT(SOD.SalesOrderDetailID) AS Frequency,
SUM(SOD.UnitPrice * SOD.OrderQty) AS Monetary_Value,ST.Name AS Region,

CASE 
WHEN MAX(SOH.OrderDate) >= '2014-01-01' THEN 'Active'
WHEN MAX(SOH.OrderDate) BETWEEN '2013-01-01' AND '2013-12-31' THEN 'Low Risk Churn'
WHEN MAX(SOH.OrderDate) BETWEEN '2012-01-01' AND '2012-12-31' THEN 'Medium Risk Churn'
ELSE 'High Risk Churn' 
END AS Recency_Segmentation,

CASE 
WHEN COUNT(SOD.SalesOrderDetailID) > 10 THEN 'Platinum'
 WHEN COUNT(SOD.SalesOrderDetailID) > 5 THEN 'Gold'
 ELSE 'Silver'
END AS Frequency_Segmentation,

 CASE
 WHEN SUM(SOD.UnitPrice * SOD.OrderQty) > 5000 THEN 'High Value Customer'
 WHEN SUM(SOD.UnitPrice * SOD.OrderQty) > 1000 THEN 'Medium Value Customer'
 ELSE 'Low Value Customer'
END AS Monetary_Segmentation,

 CASE
WHEN (MAX(SOH.OrderDate) >= '2014-01-01') 
AND (COUNT(SOD.SalesOrderDetailID) > 10) 
AND (SUM(SOD.UnitPrice * SOD.OrderQty) > 5000) 
THEN 'Loyal'        
WHEN (MAX(SOH.OrderDate) BETWEEN '2013-01-01' AND '2013-12-31') 
AND (COUNT(SOD.SalesOrderDetailID) > 5) 
AND (SUM(SOD.UnitPrice * SOD.OrderQty) > 1000) 
 THEN 'Potential Loyalist'
WHEN (MAX(SOH.OrderDate) BETWEEN '2012-01-01' AND '2012-12-31') 
AND (COUNT(SOD.SalesOrderDetailID) <= 5) 
AND (SUM(SOD.UnitPrice * SOD.OrderQty) <= 1000) 
THEN 'Promising Loyalist'
ELSE 'Lost Customers'
END AS RFM_Score
FROM 
sales.Customer C
INNER JOIN 
sales.SalesOrderHeader SOH ON C.CustomerID = SOH.CustomerID
INNER JOIN 
sales.SalesOrderDetail SOD ON SOH.SalesOrderID = SOD.SalesOrderID
INNER join sales.SalesTerritory ST ON C.TerritoryID = ST.TerritoryID
GROUP BY 
C.CustomerID,ST.Name;




select * from Production.Product

-----Business Challenge 
1.Cross selling and upselling( To identify frequently purchase product pairs)
2.Inventory management(To identify popular products among high value customers)


-- CTE for Total Sales and Revenue per Product


WITH ProductSales AS (
    SELECT 
        SOD.ProductID, 
        P.Name AS Product_Name,
        SUM(SOD.OrderQty) AS Total_Sold,
        SUM(SOD.UnitPrice * SOD.OrderQty) AS Total_Revenue
    FROM 
        sales.SalesOrderDetail SOD
    LEFT JOIN 
        Production.Product P ON SOD.ProductID = P.ProductID
    LEFT JOIN 
        SALES.SalesOrderHeader SOH ON SOD.SalesOrderID = SOH.SalesOrderID
    LEFT JOIN 
        SALES.Customer C ON SOH.CustomerID = C.CustomerID
    GROUP BY 
        SOD.ProductID, P.Name
),

-- CTE for Frequently Purchased Product Pairs

ProductPairs AS (
    SELECT  
        SOD1.ProductID AS PRODUCT_A,
        SOD2.ProductID AS PRODUCT_B,
        COUNT(*) AS PurchaseCount
    FROM 
        sales.SalesOrderDetail SOD1
    LEFT JOIN 
        sales.SalesOrderDetail SOD2 
        ON SOD1.SalesOrderID = SOD2.SalesOrderID 
        AND SOD1.ProductID < SOD2.ProductID
    WHERE 
        SOD2.ProductID IS NOT NULL
    GROUP BY 
        SOD1.ProductID, SOD2.ProductID
)

-- Combine the results from both CTEs

SELECT 
    PS.ProductID,
    PS.Product_Name,
    PS.Total_Sold,
    PS.Total_Revenue,
    PP.PRODUCT_A,
    PP.PRODUCT_B,
    PP.PurchaseCount
FROM 
    ProductSales PS
LEFT JOIN 
    ProductPairs PP ON PS.ProductID = PP.PRODUCT_A
ORDER BY 
    PS.Total_Revenue DESC, 
    PP.PurchaseCount DESC;

Select * from sales_data



Business Challenge 7: …Customer Profitability Score(CPS) - To calculate CPS for each customer
--Customer Profitability Score = Total Revenue - Total Cost
 
Queries:
 
Select * from Sales.Customer C
Select * from Sales.SalesOrderHeader SOH
Select * from Sales.SalesOrderDetail SOD
Select * from Sales.SalesTerritory ST
 
Select C.CustomerID,
SUM(ST.SalesYTD) AS Total_Revenue,
SUM(SOH.TotalDue) AS Total_Cost,
SUM(ST.SalesYTD)-SUM(SOH.TotalDue) AS CPS
FROM Sales.Customer C
INNER JOIN Sales.SalesOrderHeader SOH ON C.CustomerID = SOH.CustomerID
INNER JOIN Sales.SalesTerritory ST ON SOH.TerritoryID = ST.TerritoryID
GROUP BY C.CustomerID
