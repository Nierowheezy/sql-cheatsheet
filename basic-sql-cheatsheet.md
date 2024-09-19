# Comprehensive SQL Cheat Sheet

This cheat sheet provides a quick and easy way to reference common SQL queries with examples. It’s designed to serve as a handy tool to enhance your SQL skills. Contributions are always welcome! Feel free to submit pull requests to enrich the cheat sheet with more queries or improvements.

## Table of Contents

1. [Basic Data Retrieval](#retrieval)
2. [Data Manipulation](#manipulation)
3. [Aggregate and Reporting Functions](#aggregate)
4. [Join Operations](#joins)
5. [View Management](#views)
6. [Table Modifications](#table_modifications)
7. [Table Creation](#table_creation)

---

<a name="retrieval"></a>

## 1. Basic Data Retrieval

### **SELECT**: Retrieve data from a table.

```sql
SELECT * FROM employees;
SELECT name, salary FROM employees;
```

### **DISTINCT**: Eliminate duplicate values in your result set.

```sql
SELECT DISTINCT department FROM employees;
```

### **WHERE**: Filter results based on conditions.

```sql
SELECT name, salary FROM employees WHERE department = 'IT';
SELECT * FROM employees WHERE salary > 50000;
```

### **ORDER BY**: Sort results in ascending or descending order.

```sql
SELECT * FROM employees ORDER BY salary DESC;
SELECT * FROM employees ORDER BY department ASC, salary DESC;
```

### **LIMIT**: Limit the number of rows returned.

```sql
SELECT * FROM employees LIMIT 5;
SELECT * FROM employees LIMIT 10 OFFSET 5; -- Skip first 5 rows and fetch the next 10
```

### **LIKE**: Search for patterns within text columns.

```sql
SELECT * FROM employees WHERE name LIKE 'J%'; -- Starts with J
SELECT * FROM employees WHERE name LIKE '%son'; -- Ends with 'son'
```

### **IN**: Filter rows based on a list of possible values.

```sql
SELECT * FROM employees WHERE department IN ('HR', 'IT', 'Finance');
```

### **BETWEEN**: Select values within a range (inclusive).

```sql
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 60000;
```

### **IS NULL / IS NOT NULL**: Check for NULL values.

```sql
SELECT * FROM employees WHERE manager_id IS NULL;
SELECT * FROM employees WHERE manager_id IS NOT NULL;
```

### **AS**: Alias columns or tables for readability.

```sql
SELECT name AS employee_name, salary AS monthly_income FROM employees;
SELECT e.name, e.salary FROM employees AS e;
```

### **UNION / UNION ALL**: Combine the results of two queries.

```sql
SELECT name FROM employees WHERE department = 'IT'
UNION
SELECT name FROM employees WHERE department = 'HR';

SELECT name FROM employees WHERE department = 'IT'
UNION ALL
SELECT name FROM employees WHERE department = 'HR'; -- Allows duplicates
```

### **EXISTS**: Check if a subquery returns any results.

```sql
SELECT * FROM employees WHERE EXISTS (SELECT * FROM projects WHERE employees.id = projects.employee_id);
```

### **GROUP BY**: Group rows that have the same values into summary rows.

```sql
SELECT department, COUNT(*) FROM employees GROUP BY department;
```

### **HAVING**: Filter groups based on conditions.

```sql
SELECT department, COUNT(*) FROM employees GROUP BY department HAVING COUNT(*) > 5;
```

### **WITH**: Common Table Expressions (CTE) for more readable queries.

```sql
WITH high_salary AS (
    SELECT * FROM employees WHERE salary > 60000
)
SELECT * FROM high_salary WHERE department = 'IT';
```

---

<a name="manipulation"></a>

## 2. Data Manipulation

### **INSERT INTO**: Add new records to a table.

```sql
INSERT INTO employees (name, department, salary) VALUES ('Alice', 'HR', 55000);
INSERT INTO employees VALUES (1, 'Bob', 'IT', 70000, 2);
```

### **UPDATE**: Modify existing records in a table.

```sql
UPDATE employees SET salary = 75000 WHERE name = 'Alice';
UPDATE employees SET salary = salary * 1.05 WHERE department = 'IT';
```

### **DELETE**: Remove rows from a table.

```sql
DELETE FROM employees WHERE name = 'Bob';
DELETE FROM employees WHERE department = 'Finance' AND salary < 45000;
```

---

<a name="aggregate"></a>

## 3. Aggregate and Reporting Functions

### **COUNT()**: Return the number of rows.

```sql
SELECT COUNT(*) FROM employees;
SELECT COUNT(DISTINCT department) FROM employees;
```

### **MIN() / MAX()**: Find the smallest/largest value.

```sql
SELECT MIN(salary) FROM employees;
SELECT MAX(salary) FROM employees WHERE department = 'IT';
```

### **AVG()**: Calculate the average value of a numeric column.

```sql
SELECT AVG(salary) FROM employees WHERE department = 'HR';
```

### **SUM()**: Calculate the total sum of a numeric column.

```sql
SELECT SUM(salary) FROM employees WHERE department = 'Finance';
```

---

<a name="joins"></a>

## 4. Join Operations

### **INNER JOIN**: Return rows that have matching values in both tables.

```sql
SELECT e.name, p.project_name
FROM employees e
INNER JOIN projects p ON e.id = p.employee_id;
```

### **LEFT JOIN**: Return all rows from the left table, with matching rows from the right table.

```sql
SELECT e.name, p.project_name
FROM employees e
LEFT JOIN projects p ON e.id = p.employee_id;
```

### **RIGHT JOIN**: Return all rows from the right table, with matching rows from the left table.

```sql
SELECT e.name, p.project_name
FROM employees e
RIGHT JOIN projects p ON e.id = p.employee_id;
```

### **FULL JOIN**: Return rows when there is a match in either table.

```sql
SELECT e.name, p.project_name
FROM employees e
FULL JOIN projects p ON e.id = p.employee_id;
```

### **Self JOIN**: Join a table with itself.

```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.id;
```

---

<a name="views"></a>

## 5. View Management

### **CREATE VIEW**: Create a virtual table based on the result set of a query.

```sql
CREATE VIEW high_salary_employees AS
SELECT * FROM employees WHERE salary > 70000;
```

### **SELECT FROM VIEW**: Retrieve data from a view.

```sql
SELECT * FROM high_salary_employees;
```

### **DROP VIEW**: Remove an existing view.

```sql
DROP VIEW high_salary_employees;
```

---

<a name="table_modifications"></a>

## 6. Table Modifications

### **ALTER TABLE**: Modify the structure of a table.

#### Add a new column:

```sql
ALTER TABLE employees ADD hire_date DATE;
```

#### Change the data type of a column:

```sql
ALTER TABLE employees MODIFY salary DECIMAL(10, 2);
```

#### Remove a column:

```sql
ALTER TABLE employees DROP COLUMN hire_date;
```

---

<a name="table_creation"></a>

## 7. Table Creation

### **CREATE TABLE**: Create a new table in the database.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10, 2),
    manager_id INT
);
```

This SQL cheat sheet provides essential SQL commands for quick reference. Feel free to add more examples.
