Here’s an advanced SQL cheatsheet rewritten with examples in Markdown format:

---

# Advanced SQL Cheatsheet

This cheatsheet covers advanced SQL queries and techniques, with examples for each. It serves as a quick reference guide for users working on complex SQL queries and performance optimization.

## Table of Contents

1. [Advanced Data Retrieval](#advanced-find)
2. [Data Modification](#advanced-modify)
3. [Advanced Reporting](#advanced-report)
4. [Joins and Subqueries](#advanced-joins)
5. [Window Functions](#window-functions)
6. [CTEs and Recursive Queries](#cte)
7. [Transactions](#transactions)
8. [Performance Optimization](#optimization)

---

<a name="advanced-find"></a>

## 1. Advanced Data Retrieval

### **CASE**: Create conditional expressions

```sql
SELECT employee_name,
       CASE
           WHEN salary >= 50000 THEN 'High'
           WHEN salary >= 30000 THEN 'Medium'
           ELSE 'Low'
       END AS salary_range
FROM employees;
```

### **COALESCE**: Return the first non-NULL value

```sql
SELECT employee_id, COALESCE(phone_number, 'No Phone') AS contact_info
FROM employees;
```

### **EXISTS**: Check if subquery returns any records

```sql
SELECT customer_name
FROM customers
WHERE EXISTS (SELECT 1 FROM orders WHERE customers.customer_id = orders.customer_id);
```

### **ANY**: Compare values to any value in a list

```sql
SELECT product_name
FROM products
WHERE price > ANY (SELECT price FROM competitors_products);
```

### **Subquery in SELECT**: Use a query inside another query

```sql
SELECT employee_name,
       (SELECT department_name FROM departments WHERE departments.department_id = employees.department_id) AS department
FROM employees;
```

---

<a name="advanced-modify"></a>

## 2. Data Modification

### **MERGE**: Update or insert data conditionally (SQL Server)

```sql
MERGE INTO target_table AS T
USING source_table AS S
ON T.id = S.id
WHEN MATCHED THEN
    UPDATE SET T.column = S.column
WHEN NOT MATCHED THEN
    INSERT (column1, column2) VALUES (S.column1, S.column2);
```

### **INSERT INTO SELECT**: Insert data from another table

```sql
INSERT INTO archive_table (id, name)
SELECT id, name FROM active_table
WHERE created_at < '2023-01-01';
```

### **UPSERT**: Insert or update if exists (MySQL)

```sql
INSERT INTO employees (employee_id, employee_name)
VALUES (1, 'John Doe')
ON DUPLICATE KEY UPDATE employee_name = 'John Doe';
```

### **TRUNCATE TABLE**: Quickly remove all data from a table

```sql
TRUNCATE TABLE old_data;
```

---

<a name="advanced-report"></a>

## 3. Advanced Reporting

### **GROUP BY with HAVING**: Filter aggregated results

```sql
SELECT department_id, COUNT(*)
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;
```

### **ROLLUP**: Generate subtotals and grand totals

```sql
SELECT department, SUM(salary)
FROM employees
GROUP BY ROLLUP (department);
```

### **CUBE**: Generate subtotals for all combinations of columns

```sql
SELECT region, product, SUM(sales)
FROM sales
GROUP BY CUBE (region, product);
```

### **PIVOT**: Rotate data for reporting (SQL Server)

```sql
SELECT product, [2023], [2024]
FROM (SELECT product, year, revenue FROM sales_data) AS SourceTable
PIVOT (SUM(revenue) FOR year IN ([2023], [2024])) AS PivotTable;
```

---

<a name="advanced-joins"></a>

## 4. Joins and Subqueries

### **CROSS JOIN**: Returns the Cartesian product of two tables

```sql
SELECT p.product_name, c.customer_name
FROM products p
CROSS JOIN customers c;
```

### **Self Join**: Joining a table to itself

```sql
SELECT e1.employee_name, e2.manager_name
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.employee_id;
```

### **Correlated Subquery**: A subquery that references the outer query

```sql
SELECT employee_id, salary
FROM employees e
WHERE salary > (SELECT AVG(salary) FROM employees WHERE department_id = e.department_id);
```

---

<a name="window-functions"></a>

## 5. Window Functions

### **ROW_NUMBER**: Assign a unique row number to each record

```sql
SELECT employee_name, ROW_NUMBER() OVER (ORDER BY hire_date) AS row_num
FROM employees;
```

### **RANK**: Rank rows with gaps

```sql
SELECT employee_name, salary, RANK() OVER (ORDER BY salary DESC) AS rank
FROM employees;
```

### **NTILE**: Distribute rows into equal-sized groups

```sql
SELECT employee_name, salary, NTILE(4) OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

---

<a name="cte"></a>

## 6. CTEs and Recursive Queries

### **Simple CTE**

```sql
WITH DepartmentSales AS (
    SELECT department_id, SUM(sales) AS total_sales
    FROM sales
    GROUP BY department_id
)
SELECT * FROM DepartmentSales;
```

### **Recursive CTE**

```sql
WITH RECURSIVE EmployeeHierarchy AS (
    SELECT employee_id, manager_id, employee_name
    FROM employees
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.employee_id, e.manager_id, e.employee_name
    FROM employees e
    INNER JOIN EmployeeHierarchy h ON e.manager_id = h.employee_id
)
SELECT * FROM EmployeeHierarchy;
```

---

<a name="transactions"></a>

## 7. Transactions

### **BEGIN / COMMIT / ROLLBACK**: Group multiple statements into a transaction

```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

COMMIT;
```

### **TRY / CATCH (SQL Server)**: Handle errors in SQL

```sql
BEGIN TRY
    BEGIN TRANSACTION;

    -- SQL statements
    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

    COMMIT;
END TRY
BEGIN CATCH
    ROLLBACK;
    -- Error handling
END CATCH;
```

---

<a name="optimization"></a>

## 8. Performance Optimization

### **Create Index**: Speed up queries

```sql
CREATE INDEX idx_employee_name ON employees(employee_name);
```

### **Clustered Index**: Physically orders data in the table

```sql
CREATE CLUSTERED INDEX idx_department_id ON employees(department_id);
```

### **Non-Clustered Index**: Logical ordering of data

```sql
CREATE NONCLUSTERED INDEX idx_salary ON employees(salary);
```

### **Dropping Indexes**

```sql
DROP INDEX idx_employee_name ON employees;
```

---

This advanced SQL cheatsheet provides a reference for complex queries and SQL optimization techniques. Feel free to add more examples, commands, or complex queries
