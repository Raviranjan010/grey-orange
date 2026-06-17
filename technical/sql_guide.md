# 🗄️ SQL & Databases Preparation Guide

As a Software Support Engineer at GreyOrange, database analysis is critical. When a customer reports an issue, you must check transaction records, find stuck states in tables, and extract reports.

---

## 📋 Confirmed 2025 Batch Interview SQL Q&As

Please tap on any question below to expand its detailed answer.

<details>
<summary><b>Q1: How do you search for text in a database column?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
Use the <code>LIKE</code> operator with the wildcard character <code>%</code> in a <code>WHERE</code> clause.
<ul>
  <li><b>Search anywhere in text:</b>
    <pre><code>SELECT * FROM employees 
WHERE name LIKE '%Ravi%';</code></pre>
    This finds "Ravi", "Raviranjan", "Saurav", etc.
  </li>
  <li><b>Starts with text:</b>
    <pre><code>SELECT * FROM products 
WHERE code LIKE 'Grey%';</code></pre>
  </li>
  <li><b>Ends with text:</b>
    <pre><code>SELECT * FROM logs 
WHERE status LIKE '%failed';</code></pre>
  </li>
  <li><b>Case-insensitive search (PostgreSQL):</b> Use <code>ILIKE</code> instead of <code>LIKE</code>.</li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q2: What is the difference between an INNER JOIN and an OUTER JOIN?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
<ul>
  <li><b>INNER JOIN:</b> Returns rows only when there is a match in <b>both</b> joined tables. If a row in the left table does not have a matching ID in the right table, it is excluded.</li>
  <li><b>OUTER JOIN (LEFT/RIGHT/FULL):</b> Returns matching records <b>plus</b> unmatched records from one or both tables. The missing fields are populated with <code>NULL</code>.
    <ul>
      <li><code>LEFT JOIN</code>: Returns all rows from the left table, and matched rows from the right table.</li>
      <li><code>RIGHT JOIN</code>: Returns all rows from the right table, and matched rows from the left table.</li>
      <li><code>FULL OUTER JOIN</code>: Returns all rows from both tables, filling with <code>NULL</code> where there is no match.</li>
    </ul>
  </li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q3: How do you copy or export a table to a CSV file?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
Depending on the database engine, use one of the following commands:
<ul>
  <li><b>PostgreSQL (Server-side COPY):</b>
    <pre><code>COPY employees TO '/var/lib/postgresql/data/employees.csv' WITH CSV HEADER;</code></pre>
  </li>
  <li><b>PostgreSQL (psql terminal client-side \copy - no superuser required):</b>
    <pre><code>\copy employees TO 'd:/Temp/employees.csv' WITH CSV HEADER;</code></pre>
  </li>
  <li><b>MySQL (INTO OUTFILE):</b>
    <pre><code>SELECT * FROM employees
INTO OUTFILE '/var/lib/mysql-files/employees.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n';</code></pre>
  </li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q4: What is a Primary Key vs a Foreign Key?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
<ul>
  <li><b>Primary Key:</b> A column (or set of columns) that uniquely identifies each record in a table. It must contain unique values and cannot contain <code>NULL</code>. There can only be <b>one</b> primary key per table.</li>
  <li><b>Foreign Key:</b> A column (or set of columns) in one table that references the Primary Key of another table. It establishes a link between the data in the two tables, maintaining referential integrity. It can contain duplicate values and <code>NULL</code>s.</li>
</ul>
</blockquote>
</details>

---

## 📝 4 Must-Know SQL Patterns (Memorize & Write from Memory)

Interviewers frequently ask candidates to write these patterns on a whiteboard or a shared editor.

### Pattern 1: Count Employees per Department (Aggregation & Sorting)
Find how many employees are in each department, sorted with the highest count first.
```sql
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
ORDER BY total DESC;
```

### Pattern 2: INNER JOIN (Show Matching Records Only)
Display the employee's name alongside their department name. Only shows employees who belong to a valid department.
```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;
```

### Pattern 3: LEFT JOIN (Show All Records from Primary Table)
Display all employees alongside their department names. If an employee has no department (e.g. newly hired), show their name with the department name as `NULL`.
```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
```

### Pattern 4: Find Employees with Salary Above Average (Subquery)
Extract a list of employees whose salary is higher than the overall company average.
```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

---

## 🏗️ Visualizing JOIN Results

Assume we have these tables:
* **`Employees`**: Ravi (dept_id = 10), Priya (dept_id = 20), Rahul (dept_id = NULL)
* **`Departments`**: 10 (Engineering), 20 (Support), 30 (HR)

| Join Type | Result Rows Included | Rows Excluded |
|---|---|---|
| **INNER JOIN** | Ravi (Engineering), Priya (Support) | Rahul (no dept), HR (no employees) |
| **LEFT JOIN** | Ravi (Engineering), Priya (Support), Rahul (NULL) | HR (no employees) |
| **RIGHT JOIN** | Ravi (Engineering), Priya (Support), NULL (HR) | Rahul (no dept) |
| **FULL JOIN** | Ravi (Engineering), Priya (Support), Rahul (NULL), NULL (HR) | None |
