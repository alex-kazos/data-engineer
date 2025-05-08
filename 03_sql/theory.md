# Section 3: SQL for Data Engineers

## Overview
SQL (Structured Query Language) is the industry-standard language for querying, manipulating, and managing data in relational databases. Mastery of SQL is essential for every data engineer, as it serves as the backbone for analytics, ETL pipelines, and data warehousing.

---

## 1. What is SQL?
- **Declarative language:** You specify *what* data to retrieve, not *how* to retrieve it.
- **Portability:** Used across all major relational databases (PostgreSQL, MySQL, SQL Server, BigQuery, Snowflake, Oracle, etc.).
- **Core use cases:** Data querying, analysis, transformation, and schema definition (DDL).

---

## 2. SQL Query Structure & Basics

### Basic Query Structure
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
GROUP BY column
HAVING group_condition
ORDER BY column ASC|DESC
LIMIT number;
```

### Example
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

---

## 3. Filtering, Sorting, and Aggregation

### Filtering Rows
- Filter rows with `WHERE`:
    ```sql
    SELECT name, salary
    FROM employees
    WHERE salary > 50000 AND department = 'Engineering';
    ```

### Sorting Results
- Sort results with `ORDER BY`:
    ```sql
    SELECT name, salary
    FROM employees
    ORDER BY salary DESC, name ASC;
    ```

### Aggregation Functions
- Common aggregates: `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`
    ```sql
    SELECT 
        department, 
        AVG(salary) AS avg_salary,
        MAX(salary) AS max_salary,
        COUNT(*) AS num_employees
    FROM employees
    GROUP BY department;
    ```

---

## 4. JOINs (Combining Multiple Tables)

| JOIN Type    | Description                                        |
|--------------|----------------------------------------------------|
| INNER JOIN   | Only rows with matches in both tables              |
| LEFT JOIN    | All rows from left table, matched from right table |
| RIGHT JOIN   | All rows from right table, matched from left table |
| FULL JOIN    | All rows from both tables (with nulls where no match) |
| CROSS JOIN   | Cartesian product (every combination)              |

### Example: INNER JOIN
```sql
SELECT 
    o.order_id, o.order_date, c.name AS customer_name
FROM 
    orders o
INNER JOIN 
    customers c ON o.customer_id = c.customer_id;
```

### Example: LEFT JOIN
```sql
SELECT 
    e.name, d.name AS dept_name
FROM 
    employees e
LEFT JOIN 
    departments d ON e.dept_id = d.dept_id;
```

---

## 5. GROUP BY, HAVING, and Advanced Aggregates

- **GROUP BY:** Group rows to perform aggregation per group.
- **HAVING:** Filter aggregated groups (after GROUP BY).

### Example
```sql
SELECT 
    dept, 
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM 
    employees
GROUP BY 
    dept
HAVING 
    COUNT(*) > 5 AND AVG(salary) > 60000;
```

---

## 6. Subqueries & Window Functions

### Subqueries (Nested Queries)
- Use subqueries in `SELECT`, `FROM`, or `WHERE` clauses.
    ```sql
    SELECT name, salary
    FROM employees
    WHERE salary > (
        SELECT AVG(salary) FROM employees
    );
    ```

### Window Functions
- Analyze data across rows related to the current row.
- Syntax: `function() OVER (PARTITION BY ... ORDER BY ...)`
    ```sql
    SELECT
        name,
        department,
        salary,
        RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
    FROM employees;
    ```

- Common window functions: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `SUM()`, `AVG()`, `LAG()`, `LEAD()`

---

## 7. Writing Efficient SQL Queries

- **Indexes:** Create indexes on frequently filtered/sorted columns for faster lookups.
- **Avoid SELECT *:** Select only required columns to reduce I/O.
- **Use LIMIT/OFFSET:** Paginate large result sets.
- **Analyze Performance:** Use `EXPLAIN` or `EXPLAIN ANALYZE` to inspect query plans.
- **CTEs (Common Table Expressions):** Use `WITH` clauses to structure complex queries.
    ```sql
    WITH recent_orders AS (
        SELECT * FROM orders WHERE order_date > '2024-01-01'
    )
    SELECT * FROM recent_orders WHERE status = 'pending';
    ```

---

## 8. Integrating SQL with Python

### Using `sqlite3` (built-in, file-based)
```python
import sqlite3

conn = sqlite3.connect('db.sqlite3')
cursor = conn.cursor()
cursor.execute('SELECT name, salary FROM employees WHERE salary > ?', (50000,))
rows = cursor.fetchall()
for row in rows:
    print(row)
```

### Using `psycopg2` (PostgreSQL)
```python
import psycopg2

conn = psycopg2.connect(
    dbname='exampledb',
    user='user',
    password='password',
    host='localhost'
)
cur = conn.cursor()
cur.execute('SELECT COUNT(*) FROM orders WHERE status = %s', ('completed',))
print(cur.fetchone())
conn.close()
```

### Using SQLAlchemy (ORM & abstraction layer)
```python
from sqlalchemy import create_engine, text

engine = create_engine('sqlite:///db.sqlite3')
with engine.connect() as conn:
    result = conn.execute(text("SELECT name FROM employees WHERE salary > :salary"), {"salary": 50000})
    for row in result:
        print(row.name)
```

---

## 9. Best Practices

- **Parameterized Queries:** Always use parameters to prevent SQL injection.
- **Testing:** Test queries on small datasets before running on production data.
- **Documentation:** Comment complex SQL logic and document assumptions.
- **Consistent Style:** Use clear formatting, indentation, and naming conventions.
- **Transactions:** Use transactions (`BEGIN`, `COMMIT`, `ROLLBACK`) for multi-step operations.
- **Security:** Restrict database permissions to least privilege.

---

## References
- [SQL Tutorial](https://www.sqltutorial.org/)
- [Mode SQL Tutorial](https://mode.com/sql-tutorial/)
- [SQLBolt](https://sqlbolt.com/)
- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/)
- [PostgreSQL Official Docs](https://www.postgresql.org/docs/)
- [Modern SQL](https://modern-sql.com/)
