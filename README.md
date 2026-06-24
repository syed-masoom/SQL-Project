# SQL-Project
-- ===================================================================================================================
-- 1 Timeline Check: What is the exact start and end date of our data?
-- ===================================================================================================================


SELECT

	MIN(DATE(Order_Timestamp)) AS FirstDate,
	MAX(DATE(Order_Timestamp)) AS LastDate,
    DATEDIFF(MAX(DATE(Order_Timestamp)), MIN(DATE(Order_Timestamp))) AS Diff

FROm Orders;

-- 2. KPIs 
-- Total Ad Spend, Total Traffic, Total Bounce, Conversion rate, Total Customers, Total Orders
-- (Scaler Sub Query)


SELECT 
    -- 1. Total Ad Spend
	
    (SELECT ROUND(SUM(Ad_Spend_USD), 2) FROM Campaign) AS Total_Ad_Spend_USD,
    
    -- 2. Total Traffic (Sessions)
    (SELECT COUNT(SessionID) FROM Traffic) AS Total_Traffic,
    
    -- 3. Total Bounces (People who left immediately)
    (SELECT SUM(Bounce_Flag) FROM Traffic) AS Total_Bounces,
    
    -- 4. Conversion Rate (Orders divided by Traffic)
    ROUND(
        ( (SELECT COUNT(OrderID) FROM Orders) / 
          (SELECT COUNT(SessionID) FROM Traffic) ) * 100
    , 2) AS Conversion_Rate_Pct,
    
    -- 5. Total Customers (Unique accounts created)
    (SELECT COUNT(DISTINCT CustomerID) FROM Customers) AS Total_Customers,
    
    -- 6. Total Orders Placed
    (SELECT COUNT(OrderID) FROM Orders) AS Total_Orders;
