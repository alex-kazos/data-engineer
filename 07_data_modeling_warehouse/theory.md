# Section 7: Data Modeling & Warehouse Concepts

## Overview
Data modeling and warehouse design are foundational for scalable, performant analytics.

---

## 1. What is Data Modeling?
- Designing the structure of data for storage and analysis
- Ensures consistency, performance, and clarity

## 2. Star Schema vs Snowflake Schema
- **Star:** Central fact table, denormalized dimensions
- **Snowflake:** Normalized dimensions (more tables, less redundancy)

## 3. Fact and Dimension Tables
- **Fact:** Quantitative data (sales, clicks, etc.)
- **Dimension:** Qualitative context (date, product, user)

Here is an example of a star schema:
```sql
-- Fact table example
CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    date_id INT,
    product_id INT,
    amount FLOAT
);

-- Dimension table example
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(100)
);
```

Here is an example of a snowflake schema:
```sql
-- Fact table example
CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    date_id INT,
    product_id INT,
    amount FLOAT
);

-- Dimension table example
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(100)
);
```

## 4. Normalization & Denormalization
- **Normalization:** Reduce redundancy, improve integrity
- **Denormalization:** Improve query speed, more redundancy

## 5. Data Warehouse Concepts
- **OLAP:** Analytical (read-heavy, aggregates)
- **OLTP:** Transactional (write-heavy)
- **Data marts:** Subset of warehouse for specific teams
- **Data lake:** Raw, unstructured storage (often on S3)

## 6. Introduction to dbt
- dbt: Tool for analytics engineering (SQL-based transformations, testing, documentation)
- Encourages modular, version-controlled SQL code

## 7. Best Practices
- Use surrogate keys for facts
- Document schemas and relationships
- Test data quality

## References
- [Kimball Group](https://www.kimballgroup.com/)
- [dbt Docs](https://docs.getdbt.com/)
- [Snowflake Docs](https://docs.snowflake.com/)
