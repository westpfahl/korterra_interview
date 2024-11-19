# Korterra Technical Questions
***
# SQL / Stored Procedure Challenge:

### **Objective:** Create a stored procedure that retrieves customer orders from the past 30 days and calculates the total sales amount per customer.

 

### **Requirements:**

1. Write a stored procedure named `GetCustomerOrders` that takes no parameters.
2. The procedure should retrieve customer orders placed in the last 30 days.
3. For each customer, return the customer ID, name, and the total sales amount.
4. Ensure the procedure handles any potential null values gracefully (e.g., missing customer names).
#### Sample Table Structures:

`Customers`: `CustomerID`, `CustomerName`
`Orders`: `OrderID`, `CustomerID`, `OrderDate`, `OrderAmount` 
#### Expected Output:
| CustomerID | CustomerName | TotalSales |
|------------|--------------|------------|
| 1          | John Doe     | 500.00     |
| 2          | Jane Smith   | 300.00     |

 
### SQL Script:
```sql
CREATE PROCEDURE GetCustomerOrders
AS
BEGIN
    -- Procedure body will go here
END
```
