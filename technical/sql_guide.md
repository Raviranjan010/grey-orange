# 🗄️ SQL & Databases Preparation Guide

As a Software Support Engineer at GreyOrange, database analysis is critical. When a customer reports an issue, you must check transaction records, find stuck states in tables, and extract reports.

---

## 📋 Specific LPU DCS Email Questions & Answers

### Q1: What is the basic SQL query for searching a text in any column?
* **Answer**: You use the `LIKE` operator with wildcard characters (`%`).
  * **Syntax**:
    ```sql
    SELECT * 
    FROM table_name 
    WHERE column_name LIKE '%search_text%';
    ```
  * `%search_text%` finds any value that contains `search_text` anywhere in the column.
  * `search_text%` finds values starting with `search_text`.
  * `%search_text` finds values ending with `search_text`.

### Q2: What is the difference between an Inner Join and an Outer Join in SQL databases?
* **Answer**:
  * **Inner Join**: Returns only the rows that have matching values in both tables. If a row in Table A doesn't have a matching key in Table B, it is excluded from the result.
  * **Outer Join**: Returns matching rows from both tables *plus* unmatched rows. Unmatched fields are filled with `NULL`. There are three types of Outer Joins:
    * **Left (Outer) Join**: Returns all records from the left table, and matching records from the right table.
    * **Right (Outer) Join**: Returns all records from the right table, and matching records from the left table.
    * **Full (Outer) Join**: Returns all records when there is a match in either left or right table.

### Q3: How can we copy a whole table of SQL into one CSV file?
* **Answer**: In PostgreSQL (widely used in enterprise software), you can use the `COPY` command:
  * **Command (Server-side)**:
    ```sql
    COPY table_name TO '/path/to/destination/file.csv' WITH CSV HEADER;
    ```
  * **Command (Client-side - using psql terminal)**:
    ```sql
    \copy table_name TO '/path/to/destination/file.csv' WITH CSV HEADER;
    ```
  * In MySQL:
    ```sql
    SELECT * FROM table_name
    INTO OUTFILE '/path/to/destination/file.csv'
    FIELDS TERMINATED BY ','
    ENCLOSED BY '"'
    LINES TERMINATED BY '\n';
    ```

---

## 🤝 SQL Joins: Visual & Syntax Breakdown
Assume we have two tables:

**`Employees` Table**
| EmpID | Name | DeptID |
|---|---|---|
| 1 | Ravi | 10 |
| 2 | Priya | 20 |
| 3 | Rahul | NULL |

**`Departments` Table**
| DeptID | DeptName |
|---|---|---|
| 10 | Engineering |
| 20 | Support |
| 30 | HR |

### 1. INNER JOIN
Returns only matching records.
```sql
SELECT e.Name, d.DeptName
FROM Employees e
INNER JOIN Departments d ON e.DeptID = d.DeptID;
```
**Output**:
| Name | DeptName |
|---|---|
| Ravi | Engineering |
| Priya | Support |

*(Rahul is excluded because DeptID is NULL. HR is excluded because no employee belongs to DeptID 30).*

### 2. LEFT JOIN
Returns all employees, matching department or NULL.
```sql
SELECT e.Name, d.DeptName
FROM Employees e
LEFT JOIN Departments d ON e.DeptID = d.DeptID;
```
**Output**:
| Name | DeptName |
|---|---|
| Ravi | Engineering |
| Priya | Support |
| Rahul | NULL |

### 3. RIGHT JOIN
Returns all departments, matching employee or NULL.
```sql
SELECT e.Name, d.DeptName
FROM Employees e
RIGHT JOIN Departments d ON e.DeptID = d.DeptID;
```
**Output**:
| Name | DeptName |
|---|---|
| Ravi | Engineering |
| Priya | Support |
| NULL | HR |

### 4. FULL OUTER JOIN
Returns everything.
```sql
SELECT e.Name, d.DeptName
FROM Employees e
FULL OUTER JOIN Departments d ON e.DeptID = d.DeptID;
```
**Output**:
| Name | DeptName |
|---|---|
| Ravi | Engineering |
| Priya | Support |
| Rahul | NULL |
| NULL | HR |

---

## 📈 Aggregation & Filtering: GROUP BY vs HAVING
* **`GROUP BY`**: Groups rows that have the same values into summary rows (like finding the total sales per region).
* **`WHERE`**: Filters rows *before* grouping occurs.
* **`HAVING`**: Filters groups *after* grouping occurs. It is used with aggregate functions (e.g. `COUNT()`, `SUM()`, `AVG()`).

### Example Query
Find departments with more than 5 employees:
```sql
SELECT DeptID, COUNT(EmpID) AS EmployeeCount
FROM Employees
WHERE Status = 'Active'         -- Filter records first
GROUP BY DeptID                 -- Group them
HAVING COUNT(EmpID) > 5;        -- Filter groups
```

---

## 📝 30+ SQL Query Practice Q&As (Self-Test)

### Basic Retrievals & Wildcards
#### Q1: Select all columns from a table named `customers` sorted by `LastName` in descending order.
* **A**:
  ```sql
  SELECT * FROM customers 
  ORDER BY LastName DESC;
  ```

#### Q2: Find all products whose name starts with 'Smart' and ends with 'Phone'.
* **A**:
  ```sql
  SELECT * FROM products 
  WHERE ProductName LIKE 'Smart%Phone';
  ```

#### Q3: How do you find rows where a specific column (e.g., `Email`) is empty or contains no value?
* **A**:
  ```sql
  SELECT * FROM users 
  WHERE Email IS NULL;
  ```

#### Q4: Write a query to find all unique job titles from an `employees` table.
* **A**:
  ```sql
  SELECT DISTINCT JobTitle FROM employees;
  ```

#### Q5: Find employees whose salary is between 50,000 and 80,000.
* **A**:
  ```sql
  SELECT * FROM employees 
  WHERE Salary BETWEEN 50000 AND 80000;
  ```

### Aggregate Queries
#### Q6: How do you find the total number of orders placed in a `sales` table?
* **A**:
  ```sql
  SELECT COUNT(OrderID) AS TotalOrders FROM sales;
  ```

#### Q7: Find the highest and lowest salary from the `employees` table.
* **A**:
  ```sql
  SELECT MAX(Salary) AS MaxSalary, MIN(Salary) AS MinSalary 
  FROM employees;
  ```

#### Q8: Calculate the average price of products in each `Category`.
* **A**:
  ```sql
  SELECT Category, AVG(Price) AS AvgPrice 
  FROM products 
  GROUP BY Category;
  ```

#### Q9: Find the number of orders placed by each customer, listing only customers who placed more than 3 orders.
* **A**:
  ```sql
  SELECT CustomerID, COUNT(OrderID) AS OrderCount 
  FROM orders 
  GROUP BY CustomerID 
  HAVING COUNT(OrderID) > 3;
  ```

#### Q10: Find the total sales amount for the year 2025.
* **A**:
  ```sql
  SELECT SUM(Amount) AS TotalSales 
  FROM sales 
  WHERE SaleDate BETWEEN '2025-01-01' AND '2025-12-31';
  ```

### Joins & Subqueries
#### Q11: Join `Orders` and `Customers` tables to display the customer's name and their order date.
* **A**:
  ```sql
  SELECT c.CustomerName, o.OrderDate
  FROM Orders o
  INNER JOIN Customers c ON o.CustomerID = c.CustomerID;
  ```

#### Q12: Write a query to show all customers and any orders they have made (including customers who have placed no orders).
* **A**:
  ```sql
  SELECT c.CustomerName, o.OrderID
  FROM Customers c
  LEFT JOIN Orders o ON c.CustomerID = o.CustomerID;
  ```

#### Q13: Find all employees who earn more than the average salary of the company.
* **A**:
  ```sql
  SELECT * FROM employees 
  WHERE Salary > (SELECT AVG(Salary) FROM employees);
  ```

#### Q14: Write a query to find the department with the highest average salary.
* **A**:
  ```sql
  SELECT DeptID, AVG(Salary) AS AvgSalary
  FROM employees
  GROUP BY DeptID
  ORDER BY AvgSalary DESC
  LIMIT 1;
  ```

#### Q15: What is a Self-Join? Write a query showing employees and the name of their Manager.
* **A**: A Self-Join joins a table to itself.
  ```sql
  SELECT e.Name AS EmployeeName, m.Name AS ManagerName
  FROM Employees e
  LEFT JOIN Employees m ON e.ManagerID = m.EmpID;
  ```

### Advanced Analytics
#### Q16: Find the second highest salary from the `employees` table.
* **A**:
  ```sql
  SELECT MAX(Salary) FROM employees
  WHERE Salary < (SELECT MAX(Salary) FROM employees);
  ```
  *(Alternative using offset)*:
  ```sql
  SELECT Salary FROM employees
  ORDER BY Salary DESC
  LIMIT 1 OFFSET 1;
  ```

#### Q17: Write a query to find the N-th highest salary (general format).
* **A**: Using `LIMIT` and `OFFSET` (where offset is N-1):
  ```sql
  SELECT Salary FROM employees
  ORDER BY Salary DESC
  LIMIT 1 OFFSET N-1;
  ```

#### Q18: How do you find duplicate email addresses in a `contacts` table?
* **A**:
  ```sql
  SELECT Email, COUNT(Email) 
  FROM contacts 
  GROUP BY Email 
  HAVING COUNT(Email) > 1;
  ```

#### Q19: Delete duplicate records from a table, keeping only the one with the smallest ID.
* **A**:
  ```sql
  DELETE FROM contacts
  WHERE ID NOT IN (
      SELECT MIN(ID)
      FROM contacts
      GROUP BY Email
  );
  ```

#### Q20: What is the difference between `UNION` and `UNION ALL`?
* **A**: Both combine results of two SELECT queries. `UNION` removes duplicate rows, whereas `UNION ALL` retains all rows (including duplicates) and is faster.

### Database Concepts
#### Q21: What is a Primary Key and a Foreign Key?
* **A**:
  * **Primary Key**: A column (or set of columns) that uniquely identifies each row in a table. It cannot contain NULL values and must be unique.
  * **Foreign Key**: A column in one table that links to a Primary Key in another table, establishing a relationship between them and enforcing referential integrity.

#### Q22: What is an Index in SQL? What are its pros and cons?
* **A**: An Index is a pointer structure used to speed up data retrieval (like a book index).
  * **Pros**: Significantly faster SELECT queries.
  * **Cons**: Slows down INSERT, UPDATE, and DELETE operations because the index must be updated, and it consumes additional disk storage.

#### Q23: What is the difference between Clustered and Non-Clustered Indexes?
* **A**:
  * **Clustered Index**: Determines the physical order of data rows in the table (only 1 per table, usually the Primary Key).
  * **Non-Clustered Index**: Creates a separate structure pointing to the physical rows (multiple allowed per table).

#### Q24: What is Normalization?
* **A**: The process of organizing data in a database to reduce redundancy and improve data integrity (1NF, 2NF, 3NF, BCNF). It involves splitting tables and defining relationships.

#### Q25: Difference between `DELETE`, `TRUNCATE`, and `DROP`?
* **A**:
  * `DELETE`: DML command. Deletes specific rows using a `WHERE` clause. It can be rolled back and triggers are fired.
  * `TRUNCATE`: DDL command. Deletes all rows from a table. It is faster, cannot be rolled back easily, and does not fire triggers. Keep the table structure.
  * `DROP`: DDL command. Completely removes the table structure and its data from the database.

#### Q26: What are ACID properties in a database?
* **A**:
  * **Atomicity**: Entire transaction succeeds or fails completely.
  * **Consistency**: Transaction takes database from one valid state to another.
  * **Isolation**: Concurrent transactions execute without interfering with each other.
  * **Durability**: Once committed, changes survive system failures.

#### Q27: What is a View in SQL?
* **A**: A virtual table based on the result-set of an SQL statement. It does not store physical data itself.

#### Q28: How do you find the count of active connections in PostgreSQL?
* **A**:
  ```sql
  SELECT COUNT(*) FROM pg_stat_activity WHERE state = 'active';
  ```

#### Q29: What is the `COALESCE` function?
* **A**: It returns the first non-null value in a list of arguments. Useful for replacing nulls with default values: `SELECT COALESCE(email, 'No Email') FROM users;`.

#### Q30: What is a Subquery and what are Correlated Subqueries?
* **A**: A subquery is a query nested inside another query. A correlated subquery is a subquery that uses values from the outer query, executing once for each row processed by the outer query.
