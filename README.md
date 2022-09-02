# Limiting-and-Sorting-Data--SQL
Limiting and Sorting Data-SQL--

1. CREATE MENU TABLE

CREATE TABLE Menu
(ItemID INT IDENTITY (1000,1) PRIMARY KEY NOT NULL,
ItemName VARCHAR (50),
ItemType VARCHAR (50) NOT NULL,
CostToMake MONEY,
Price MONEY,
WeeklySales INT,
MonthlySales INT,
YearlySales INT)

--2. Add DATA BELOW
INSERT INTO dbo.Menu
SELECT 'Big Mac','Hamburger',1.25,3.24,1015,5000,15853
union
SELECT 'Quarter Pounder / Cheese','Hamburger',1.15,3.24,1000,4589,16095
union
SELECT 'Half Pounder / Cheese','Hamburger',1.35,3.50,500,3500,12589
union
SELECT 'Whopper','Hamburger',1.55,3.99,989,4253,13000
union
SELECT 'Kobe Cheeseburger','Hamburger',2.25,5.25,350,1500,5000
union
SELECT 'Grilled Stuffed Burrito','Burrito',.75,5.00,2000,7528,17896
union
SELECT 'Bean Burrito','Burrito',.50,1.00,1750,7000,18853
union
SELECT '7 layer Burrito','Burrito',.78,2.50,350,1000,2563
union
SELECT 'Dorrito Burrito','Burrito',.85,1.50,600,2052,9857
union
SELECT 'Turkey and Cheese Sub','Sub Sandwich',1.75,5.50,1115,7878,16853
union
SELECT 'Philly Cheese Steak Sub','Sub Sandwich',2.50,6.00,726,2785,8000
union
SELECT 'Tuna Sub','Sub Sandwich',1.25,4.50,825,3214,13523
union
SELECT 'Meatball Sub','Sub Sandwich',1.95,6.50,987,4023,15287
union
SELECT 'Italian Sub','Sub Sandwich',2.25,7.00,625,1253,11111
union
SELECT '3 Cheese Sub','Sub Sandwich',.25,6.00,815,3000,11853

--- Successful Load

--3.CREATE A BACKUP MENU FILE

SELECT *
INTO Menu_Backup
FROM Menu

---Succeful build
---4. The 3 Cheese Sub is now made with 4 Cheeses. The new name will be 4 Cheese Sub

Select *
FROM Menu_Backup

UPDATE Menu_Backup
SET ItemName = '4 Cheese Sub'
--SELECT FROM Menu_Backup
WHERE ItemName = '3 Cheese Sub'

--5. Italian Sub Monthly Sales were reported incorrectly. There were really 1353 Sales.

UPDATE Menu_Backup
SET MonthlySales = '1353'
--SELECT FROM Menu_Backup
WHERE MonthlySales = '1253'

--6. The Whopper increased it’s price to $4.25

UPDATE Menu_Backup
SET Price = '4.25'
--SELECT FROM Menu_Backup
WHERE Price = '3.99'

--7. It now cost $2.75 to make the 7 layer Burrito

UPDATE Menu_Backup
SET CostToMake = '2.75', ItemName= '7 Layer Burrito'
--SELECT FROM Menu_Backup
WHERE ItemID =1001;

--8. The prices of tortillas have gone up. All Burrito prices should increase 10%

UPDATE Menu_Backup
SET CostToMake = CostToMake * 1.1
--SELECT FROM Menu_Backup
WHERE ItemType ='Burrito';

--9. All products that bring in < $1.00 profit per purchase need to be deleted

DELETE FROM Menu_Backup
WHERE (Price-CostToMake<= 1.00)

--10.10. We will be discontinuing any products that didn’t clear $10,000 in YearlySales Profit. (delete).

DELETE FROM Menu_Backup
WHERE (YearlySales <=10000)

--11. We just found out all the previous changes were incorrect. Truncate the dbo.menu_backup table.

TRUNCATE TABLE Menu_Backup

-------- LAB TWO--------

-- Using the Menu table created in Lab One,Run the following script to populate the table
INSERT INTO dbo.Menu
SELECT 'Big Mac','Hamburger',1.25,3.24,1015,5000,15853
union
SELECT 'Quarter Pounder / Cheese','Hamburger',1.15,3.24,1000,4589,16095
union
SELECT 'Half Pounder / Cheese','Hamburger',1.35,3.50,500,3500,12589
union
SELECT 'Whopper','Hamburger',1.55,3.99,989,4253,13000
union
SELECT 'Kobe Cheeseburger','Hamburger',2.25,5.25,350,1500,5000
union
SELECT 'Grilled Stuffed Burrito','Burrito',.75,5.00,2000,7528,17896
union
SELECT 'Bean Burrito','Burrito',.50,1.00,1750,7000,18853
union
SELECT '7 layer Burrito','Burrito',.78,2.50,350,1000,2563
union
SELECT 'Dorrito Burrito','Burrito',.85,1.50,600,2052,9857
union
SELECT 'Turkey and Cheese Sub','Sub Sandwich',1.75,5.50,1115,7878,16853
union
SELECT 'Philly Cheese Steak Sub','Sub Sandwich',2.50,6.00,726,2785,8000
union
SELECT 'Tuna Sub','Sub Sandwich',1.25,4.50,825,3214,13523
union
SELECT 'Meatball Sub','Sub Sandwich',1.95,6.50,987,4023,15287
union
SELECT 'Italian Sub','Sub Sandwich',2.25,7.00,625,1253,11111
union
SELECT '3 Cheese Sub','Sub Sandwich',.25,6.00,815,3000,11853

--3. Retrieve all Burritos and sort by Price

SELECT *
FROM Menu
WHERE ItemType = 'Burrito'
ORDER BY Price

--4. Retrieve all items that Cost more than $1.00 to make and sort by WeeklySales

SELECT *
FROM Menu
WHERE CostToMake> '1.00'
ORDER BY WeeklySales

--5. What’s the sum of total profit by ItemType

SELECT ItemType ,SUM ( Price-CostToMake ) AS Profit
FROM Menu
GROUP BY ItemType

-- 6.Retrieve Total Weekly Sales by ItemType of only items with more than 3000 weekly Sales. Sort by Total Weekly Sales descending.

SELECT ItemType, SUM (WeeklySales) AS TotalWeeklySales
FROM Menu
GROUP BY ItemType
HAVING SUM (WeeklySales)>3000
ORDER BY TotalWeeklySales DESC

-- 7.Find out the profit made Weekly, Monthly and Yearly on Big Mac’s

SELECT ItemName,(Price-CostToMake)*WeeklySales AS WeeklyProfit,(Price-CostToMake)MonthlySales AS MonthlyProfit,(Price-CostToMake) YearlySales AS YearProfit
FROM Menu
WHERE ItemID =1018

--8.Retrieve the ItemType has more than $20,000 in Monthly Sales

SELECT Itemtype, ( MonthlySales *Price) AS MonthlySales
FROM Menu
WHERE (MonthlySales * Price)> 20000

--9.Retrieve the ItemType that had the best Profit from MonthlySales

SELECT ItemType,(Price-CostToMake)* MonthlySales AS ProfitPerMonth
FROM Menu
ORDER BY ProfitPerMonth DESC

--ADVENTUREwORKS QUIZ ON LIMITING AND SORTING
--1. Use Sales.SalesOrderHeader to get
--- What is the AVG Sales( Total Due) and SUM Total Sales ( TotalDue) By TerritoryID,sorted by the most sales.

SELECT TerritoryID,AVG (TotalDue/TerritoryID) AS TotalDuePerTerritory ,SUM (TotalDue) AS TotalDue
FROM Sales.SalesOrderHeader
GROUP BY TerritoryID
ORDER BY TotalDuePerTerritory DESC

--USE Production.Product to find what is the Product Count,MIN List price , MAX List Price and TOTAL list Price for each Product Color, sorted by COUNT of Products per Color ( Only Show details for product with color).

SELECT Color,COUNT( ProductID )AS [PRODUCT PER COLOR], MAX (ListPrice/ProductID) AS MaxListPrice,SUM ( ListPrice) AS TotalListPrice
FROM Production.Product
WHERE [Color]<> 'NULL'
GROUP BY COLOR


