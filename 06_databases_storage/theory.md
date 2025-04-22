# Section 6: Databases and Storage (Theory)

## Overview
Data engineers must work with a variety of storage systems: relational, NoSQL, and cloud object stores.

---

## 1. Relational Databases
- Structured tables with rows/columns
- Schema, primary/foreign keys
- Examples: PostgreSQL, MySQL, SQLite

## 2. NoSQL Databases
- **Document:** MongoDB (JSON-like docs)
- **Key-Value:** Redis
- **Wide-column, Graph:** Cassandra, Neo4j
- Use for unstructured, high-velocity, or flexible data

## 3. Cloud Storage
- **AWS S3:** Buckets, objects, versioning
- **GCS/Azure Blob:** Similar concepts
- Used for data lakes, backups, ML pipelines

## 4. Connecting from Python
- **PostgreSQL:** `psycopg2`, `SQLAlchemy`
- **MongoDB:** `pymongo`
- **Redis:** `redis-py`
- **S3:** `boto3`

## 5. CRUD Operations
- **Create, Read, Update, Delete**
- SQL: `INSERT`, `SELECT`, `UPDATE`, `DELETE`
- Mongo: `insert_one`, `find`, `update_one`, `delete_one`
- Redis: `set`, `get`, `delete`

## 6. Indexes and Performance
- Indexes speed up queries
- Use wisely (write overhead)

## 7. Transactions and Consistency
- ACID properties for RDBMS
- Mongo: multi-document transactions (since v4.0)

## 8. Backups and Security
- Regular backups
- Use least-privilege access
- Encrypt sensitive data

## 9. Best Practices
- Use connection pools
- Handle errors and retries
- Monitor performance

## References
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [MongoDB Docs](https://docs.mongodb.com/)
- [AWS S3 Docs](https://docs.aws.amazon.com/s3/)
