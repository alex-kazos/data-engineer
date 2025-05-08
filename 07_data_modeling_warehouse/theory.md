# Section 7: Data Modeling & Warehouse Concepts

## Overview
Data modeling and warehouse design are essential for building scalable, high-performance analytics systems. Good models ensure data consistency, efficient queries, and easier maintenance.

---

## 1. What is Data Modeling?
- **Definition:** The process of designing the logical and physical structure of data for storage, retrieval, and analysis.
- **Goals:**
  - Maintain data consistency and integrity
  - Enable performant, flexible analytics
  - Support business understanding and reporting needs
- **Types of Models:**
  - **Conceptual:** High-level entities and relationships (e.g., ER diagrams)
  - **Logical:** Table structure, keys, relationships (independent of technology)
  - **Physical:** Implementation in a specific database (data types, indexes, partitions)

---

## 2. Star Schema vs Snowflake Schema

### Star Schema
- **Structure:** Central fact table surrounded by denormalized dimension tables
- **Pros:** Fast queries, simple design, easy to understand
- **Cons:** Some data redundancy in dimension tables

#### Example:
```sql
-- Fact table: sales transactions
CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    date_id INT,
    product_id INT,
    customer_id INT,
    amount FLOAT
);

-- Dimension tables (denormalized)
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(100),
    brand VARCHAR(100)
);

CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    country VARCHAR(100)
);

CREATE TABLE dates (
    date_id SERIAL PRIMARY KEY,
    date DATE,
    year INT,
    month INT,
    day INT,
    weekday VARCHAR(10)
);
```

### Snowflake Schema
- **Structure:** Central fact table, normalized dimension tables (split into sub-dimensions)
- **Pros:** Reduces data redundancy, improves data consistency
- **Cons:** More complex queries (more joins), harder for end-users

#### Example:
```sql
-- Fact table: sales transactions (same as above)
CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    date_id INT,
    product_id INT,
    customer_id INT,
    amount FLOAT
);

-- Normalized dimensions
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category_id INT
);

CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    category VARCHAR(100)
);

CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    country_id INT
);

CREATE TABLE countries (
    country_id SERIAL PRIMARY KEY,
    country VARCHAR(100)
);
```

---

## 3. Fact and Dimension Tables

- **Fact Tables:**
  - Store quantitative, transactional data (e.g., sales, clicks, payments)
  - Contain foreign keys to dimensions, numeric measures, and a date/time key
  - Example columns: `sale_id`, `date_id`, `product_id`, `amount`
- **Dimension Tables:**
  - Store descriptive, contextual information (e.g., product, customer, date)
  - Often denormalized in star schema, normalized in snowflake
  - Example columns: `product_id`, `name`, `category`, `brand`

---

## 4. Normalization & Denormalization

- **Normalization:**
  - Process of organizing data to minimize redundancy (e.g., splitting addresses into a separate table)
  - Improves data integrity and reduces storage
  - Example:
    ```sql
    -- Customers reference addresses to avoid duplication
    CREATE TABLE addresses (
        address_id SERIAL PRIMARY KEY,
        street VARCHAR(100),
        city VARCHAR(50),
        state VARCHAR(50),
        zip VARCHAR(20)
    );

    CREATE TABLE customers (
        customer_id SERIAL PRIMARY KEY,
        name VARCHAR(100),
        address_id INT REFERENCES addresses(address_id)
    );
    ```
- **Denormalization:**
  - Process of merging tables to reduce joins and speed up analytic queries
  - Increases redundancy for performance
  - Example:
    ```sql
    -- Denormalized reporting table
    CREATE TABLE orders_report AS
    SELECT o.order_id, c.customer_id, c.name AS customer_name, c.email AS customer_email, o.order_date
    FROM orders o
    JOIN customers c ON o.customer_id = c.customer_id;
    ```

---

## 5. Data Warehouse Concepts

- **OLAP (Online Analytical Processing):**
  - Read-heavy, supports complex queries and aggregations
  - Used for analytics, data marts, BI tools
- **OLTP (Online Transaction Processing):**
  - Write-heavy, supports fast inserts/updates
  - Used for operational applications (e.g., banking, e-commerce)
- **Data Mart:** Subset of a data warehouse for a specific business line or team (e.g., marketing mart)
- **Data Lake:** Centralized repository for raw, unstructured, and structured data (e.g., S3, Hadoop)
- **Slowly Changing Dimensions (SCD):** Techniques to manage changes in dimension data over time (e.g., Type 1 overwrite, Type 2 history)

---

## 6. Introduction to dbt

- **What is dbt?**
  - Open-source tool for transforming data in the warehouse using modular, version-controlled SQL
  - Supports testing, documentation, and lineage tracking
- **Key Features:**
  - SQL-based modeling (SELECT statements as models)
  - Automated documentation and lineage graphs
  - Data quality tests (e.g., not_null, unique, relationships)
- **Example dbt Model:**
    ```sql
    -- models/total_sales_by_product.sql
    SELECT
        product_id,
        SUM(amount) AS total_sales
    FROM {{ ref('sales') }}
    GROUP BY product_id
    ```
- **Example dbt Test:**
    ```yaml
    version: 2
    models:
      - name: total_sales_by_product
        columns:
          - name: product_id
            tests:
              - not_null
              - unique
    ```

---

## 7. Best Practices

- Use surrogate keys (integer IDs) as primary keys in fact and dimension tables
- Document schema, relationships, and data lineage
- Apply data quality tests (nulls, uniqueness, referential integrity)
- Partition large tables by date or key for performance
- Use consistent naming conventions
- Track schema changes with version control
- Secure sensitive data (masking, RBAC)

---

## References

- [Kimball Group (Data Modeling)](https://www.kimballgroup.com/)
- [dbt Docs](https://docs.getdbt.com/)
- [Snowflake Docs](https://docs.snowflake.com/)
- [The Data Warehouse Toolkit (Kimball Book)](https://www.wiley.com/en-us/The+Data+Warehouse+Toolkit%3A+The+Definitive+Guide+to+Dimensional+Modeling%2C+3rd+Edition-p-9781118530801)
