# Section 6: Databases and Storage

## Overview
Modern data engineering requires fluency with a variety of storage systems: relational (SQL), NoSQL, and cloud-based object stores. Each system is optimized for different data types, workloads, and scalability needs.

---

## 1. Relational Databases (RDBMS)
- **Structure:** Data organized into tables (rows & columns) with predefined schema.
- **Integrity:** Enforce data consistency using primary keys, foreign keys, and constraints.
- **Use Cases:** OLTP systems, analytics, transactional data.
- **Popular systems:** PostgreSQL, MySQL, SQLite, SQL Server, Oracle.

### Example: Connect to PostgreSQL with Python
```python
import psycopg2

# Connect to PostgreSQL database
conn = psycopg2.connect(
    dbname='test',
    user='user',
    password='pass',
    host='localhost',
    port=5432
)
cursor = conn.cursor()

# Execute a query
cursor.execute('SELECT * FROM users')
rows = cursor.fetchall()
for row in rows:
    print(row)

cursor.close()
conn.close()
```

---

## 2. NoSQL Databases
NoSQL systems handle unstructured, semi-structured, or rapidly changing data. Types include:

- **Document:** (e.g., MongoDB) Store JSON-like docs, dynamic schemas.
- **Key-Value:** (e.g., Redis) Simple, fast storage for caching and session data.
- **Wide-Column:** (e.g., Cassandra, HBase) Scalable, distributed storage for analytics workloads.
- **Graph:** (e.g., Neo4j) Model relationships between entities.

### Example: MongoDB and Redis in Python
```python
# MongoDB (Document)
from pymongo import MongoClient
client = MongoClient('mongodb://localhost:27017/')
db = client['test']
# Insert a document
db.users.insert_one({'name': 'Alice', 'age': 30})
# Query a document
user = db.users.find_one({'name': 'Alice'})
print(user)

# Redis (Key-Value)
import redis
r = redis.Redis(host='localhost', port=6379, db=0)
r.set('session_id', 'abc123')
print(r.get('session_id').decode())
```

---

## 3. Cloud Object Storage
- **Services:** AWS S3, Google Cloud Storage (GCS), Azure Blob Storage.
- **Concepts:** Store files (“objects”) in flat buckets. Supports metadata, versioning, lifecycle rules, and access control.
- **Typical Uses:** Data lakes, backups, ML training data, static hosting.

### Example: Upload/Download with AWS S3 (boto3)
```python
import boto3

# Create S3 client (ensure AWS credentials are configured)
s3 = boto3.client('s3')

# Upload a file
s3.upload_file('local.txt', 'my-bucket', 'remote.txt')

# Download a file
s3.download_file('my-bucket', 'remote.txt', 'downloaded.txt')
```

---

## 4. Connecting from Python
- **PostgreSQL:** [`psycopg2`](https://www.psycopg.org/), [`SQLAlchemy`](https://www.sqlalchemy.org/)
- **MongoDB:** [`pymongo`](https://pymongo.readthedocs.io/)
- **Redis:** [`redis-py`](https://redis-py.readthedocs.io/)
- **S3:** [`boto3`](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- Use environment variables or configuration files for credentials.

---

## 5. CRUD Operations (Create, Read, Update, Delete)
### SQL (PostgreSQL/MySQL)
```sql
-- Create
INSERT INTO users (name, age) VALUES ('Bob', 25);

-- Read
SELECT * FROM users WHERE age > 20;

-- Update
UPDATE users SET age = 26 WHERE name = 'Bob';

-- Delete
DELETE FROM users WHERE name = 'Bob';
```
### MongoDB
```python
db.users.insert_one({'name': 'Bob', 'age': 25})           # Create
list(db.users.find({'age': {'$gt': 20}}))                 # Read
db.users.update_one({'name': 'Bob'}, {'$set': {'age': 26}}) # Update
db.users.delete_one({'name': 'Bob'})                      # Delete
```
### Redis
```python
r.set('user:1', 'Bob')          # Create/Update
print(r.get('user:1'))          # Read
r.delete('user:1')              # Delete
```

---

## 6. Indexes and Performance Tuning
- **Indexes:** Speed up SELECT queries (at cost of slower INSERT/UPDATE).
    - RDBMS: `CREATE INDEX idx_name ON users(name);`
    - MongoDB: `db.users.create_index('name')`
    - Redis: Not applicable; design keys for access patterns.
- **Query Optimization:** Use EXPLAIN to analyze query plans.
- **Partitioning/Sharding:** Distribute data for scalability.

---

## 7. Transactions and Consistency
- **ACID Transactions** (Relational): Atomicity, Consistency, Isolation, Durability.
- **NoSQL:** Varies; MongoDB supports multi-document transactions (v4.0+), Redis supports transactions with MULTI/EXEC.
- **Isolation Levels:** Control concurrency effects.

---

## 8. Backups, Security, and High Availability
- **Backups:** Automate regular backups (database dumps, object versioning).
- **Security:** Use least-privilege access, encrypt data at rest and in transit, enable audit logs.
- **High Availability:** Replication (Postgres streaming, Mongo replica sets, Redis Sentinel), failover mechanisms.
- **Access Controls:** Roles, IAM policies, firewalls.

---

## 9. Best Practices for Data Engineers
- Use connection pools for efficient resource use.
- Handle errors, retries, and timeouts robustly.
- Monitor performance (slow query logs, metrics, dashboards).
- Automate database migrations and schema changes.
- Document schemas and access patterns.
- Regularly test restores from backups.

---

## References & Further Reading
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [MySQL Docs](https://dev.mysql.com/doc/)
- [MongoDB Docs](https://docs.mongodb.com/)
- [Redis Docs](https://redis.io/documentation)
- [AWS S3 Docs](https://docs.aws.amazon.com/s3/)
- [boto3 Docs](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- [SQLAlchemy Docs](https://docs.sqlalchemy.org/)
