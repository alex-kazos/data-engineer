# Section 3: SQL for Data Engineers

## Overview
SQL (Structured Query Language) is the universal language for querying and manipulating relational databases. Every data engineer must be fluent in SQL.

---

## 1. What is SQL?
- Declarative language for working with structured data
- Used in nearly all data platforms (PostgreSQL, MySQL, BigQuery, etc.)

## 2. SQL Basics
- **SELECT:** Retrieve data
- **FROM:** Specify table
- **WHERE:** Filter rows
- **ORDER BY:** Sort results
- **LIMIT:** Restrict number of results

```sql
SELECT 
    name, 
    age 
FROM 
    users 
WHERE 
    age > 18 
ORDER BY 
    age DESC 
LIMIT 
    10;
```

## 3. Filtering, Sorting, Aggregating
- WHERE: `... WHERE col = 'value'`
- ORDER BY: `... ORDER BY col ASC|DESC`
- Aggregates: `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`

### Filtering Rows
```sql
SELECT 
    name,
    salary 
FROM 
    employees 
WHERE 
    salary > 50000;
```

### Sorting Results
```sql
SELECT 
    name, 
    salary 
FROM 
    employees 
ORDER BY 
    salary DESC;
```

### Aggregation
```sql
SELECT 
    department, 
    AVG(salary) 
FROM 
    employees 
GROUP BY 
    department;
```


## 4. JOINs
- **INNER JOIN:** Only matching rows
- **LEFT JOIN:** All from left, matched from right
- **RIGHT JOIN:** All from right, matched from left
- **FULL JOIN:** All rows from both
- **CROSS JOIN:** Cartesian product

```sql
SELECT 
    a.*, b.* 
FROM 
    a
INNER JOIN 
    b ON a.id = b.id
```

## 5. GROUP BY, HAVING, Aggregates
- Group rows by one or more columns
- HAVING filters groups after aggregation

```sql
SELECT 
    dept, 
    COUNT(*) 
FROM 
    employees 
GROUP BY 
    dept 
HAVING 
    COUNT(*) > 5;
```

## 6. Window Functions & Subqueries
- **Window:** `ROW_NUMBER()`, `RANK()`, `SUM() OVER (PARTITION BY ...)`
- **Subquery:** Query inside another query

```sql
SELECT 
    name, 
    salary, 
    RANK() OVER (ORDER BY salary DESC) 
FROM 
    employees;
```

## 7. Writing Efficient Queries
- Use indexes for fast lookups
- Avoid `SELECT *` in production
- Use `EXPLAIN` to analyze query plans

## 8. Integrating SQL with Python
- `sqlite3` (built-in, file-based)
- `psycopg2` (PostgreSQL)
- `SQLAlchemy` (ORM/abstraction)

```python
import sqlite3

conn = sqlite3.connect('db.sqlite3')
cursor = conn.cursor()
cursor.execute('SELECT * FROM table')
rows = cursor.fetchall()
```

## 9. Best Practices
- Use parameterized queries to prevent SQL injection
- Always test queries on small data first
- Document your SQL code

## References
- [SQL Tutorial](https://www.sqltutorial.org/)
- [Mode SQL Tutorial](https://mode.com/sql-tutorial/)
- [SQLAlchemy Docs](https://docs.sqlalchemy.org/)
