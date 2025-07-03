# Task 7: Creating Views
Objective
Learn how to create and use SQL views to abstract complex queries, enhance reusability, and improve data access control and security.

## Tools Used
MySQL Workbench (recommended)

## Sample Tables Used
Using the same tables from previous tasks:

1. Customers
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    Country VARCHAR(50)
);
```

2. Orders
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderAmount DECIMAL(10,2)
);
```

### 1. Create a Simple View (Customer Orders Summary)
```sql
CREATE VIEW CustomerOrderSummary AS
SELECT 
    c.CustomerID,
    c.CustomerName,
    c.Country,
    COUNT(o.OrderID) AS TotalOrders,
    SUM(o.OrderAmount) AS TotalAmount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID, c.CustomerName, c.Country;

SELECT * FROM CustomerOrderSummary;
```
### 2. Create a View with a WHERE Condition
Only customers from India with their total order value.

```sql
CREATE VIEW IndianCustomersOrders AS
SELECT 
    c.CustomerName,
    SUM(o.OrderAmount) AS TotalSpent
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID
WHERE c.Country = 'India'
GROUP BY c.CustomerName;

SELECT * FROM IndianCustomersOrders;
```

### 3. Security-Oriented View (Hiding Customer IDs)
```sql
CREATE VIEW PublicCustomerOrders AS
SELECT 
    c.CustomerName,
    o.OrderAmount
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID;
```
Use this view if you want to restrict access to CustomerID while sharing data publicly.

### Drop a View
```sql
DROP VIEW IF EXISTS CustomerOrderSummary;
```
