# Section 6: Databases and Storage

## Overview
Data engineers must work with a variety of storage systems: relational, NoSQL, and cloud object stores.

---

## 1. Relational Databases
- Structured tables with rows/columns
- Schema, primary/foreign keys
- Examples: PostgreSQL, MySQL, SQLite

Here is an example of how to connect to a PostgreSQL database and execute a query:
```python
import psycopg2

conn = psycopg2.connect(dbname='test', user='user', password='pass')
cursor = conn.cursor()
cursor.execute('SELECT * FROM users')
rows = cursor.fetchall()
```


## 2. NoSQL Databases
- **Document:** MongoDB (JSON-like docs)
- **Key-Value:** Redis
- **Wide-column, Graph:** Cassandra, Neo4j
- Use for unstructured, high-velocity, or flexible data

Here is an example of how to connect to a MongoDB database and execute a query:
```python
# MongoDB
from pymongo import MongoClient
client = MongoClient()
db = client.test
db.users.find_one()

# Redis
import redis
r = redis.Redis()
r.set('key', 'value')
print(r.get('key'))
```

## 3. Cloud Storage
- **AWS S3:** Buckets, objects, versioning
- **GCS/Azure Blob:** Similar concepts
- Used for data lakes, backups, ML pipelines

Here is an example of how to connect to an S3 bucket and upload a file:
```python
import boto3
s3 = boto3.client('s3')
s3.upload_file('local.txt', 'bucket', 'remote.txt')
```

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
