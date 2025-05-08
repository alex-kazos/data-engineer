# Section 8: Big Data & Streaming

## Overview

Big data and streaming systems enable organizations to efficiently process, analyze, and react to massive, fast, and diverse data sources. These technologies are core for real-time analytics, machine learning pipelines, IoT, fraud detection, and large-scale data integration.

---

## 1. What is Big Data?

Big Data is characterized by the "3 Vs":
- **Volume:** Massive datasets (terabytes to petabytes and beyond).
- **Velocity:** High speed of data generation and processing (real-time or near-real-time).
- **Variety:** Multiple data types and sources (structured, semi-structured, unstructured).

**Challenges:** Traditional databases and tools struggle with scale, speed, and heterogeneity. Big data frameworks provide distributed storage and parallel computation.

---

## 2. Batch vs. Streaming Processing

- **Batch Processing**
    - Processes large blocks of data at once (e.g., hourly, nightly ETL jobs).
    - High throughput, but higher latency.
    - Example Use: Data warehouse loads, historical analytics.

- **Streaming Processing**
    - Processes data continuously, as soon as it arrives.
    - Low latency, enables real-time insights and reactions.
    - Example Use: Fraud detection, IoT sensor data, live dashboards.

---

## 3. Apache Spark (PySpark)

- **Apache Spark:** A unified analytics engine for large-scale data processing.
    - Supports batch & streaming, SQL, ML, graph processing.
    - In-memory computation for speed.
    - APIs: Python (PySpark), Scala, Java, R.

### 3.1 Key Concepts

- **SparkSession:** Entry point for DataFrame and SQL functionality.
- **RDD (Resilient Distributed Dataset):** Low-level, immutable distributed collections of objects.
- **DataFrame:** High-level, schema-aware distributed table.
- **Transformations:** Lazy operations that define computation (e.g., `filter`, `select`, `groupBy`).
- **Actions:** Trigger execution and return results (e.g., `show`, `collect`, `count`).

---

### 3.2 PySpark: Reading Data

```python
from pyspark.sql import SparkSession

# Start Spark session
spark = SparkSession.builder.appName("example").getOrCreate()

# Read CSV as DataFrame
df = spark.read.csv('data.csv', header=True, inferSchema=True)
df.printSchema()
df.show(5)
```

---

### 3.3 Transformations and Actions

```python
# Transformation: Lazy operation (no computation yet)
filtered = df.filter(df['value'] > 10).select('category', 'value')

# Action: Triggers computation
print(filtered.count())
filtered.show()
```

---

### 3.4 GroupBy and Aggregation

```python
from pyspark.sql.functions import avg, max, count

agg_df = df.groupBy('category').agg(
    count('*').alias('num_rows'),
    avg('value').alias('average_value'),
    max('value').alias('max_value')
)
agg_df.show()
```

---

### 3.5 Chaining Multiple Transformations

```python
result = (
    df.filter(df['status'] == 'active')
      .withColumnRenamed('value', 'amount')
      .groupBy('category')
      .sum('amount')
      .orderBy('sum(amount)', ascending=False)
)
result.show()
```

---

## 4. Stream Processing

Stream processing frameworks handle data in real time, often with micro-batch or event-at-a-time models.

- **Kafka:** Distributed messaging system for building real-time pipelines and streaming apps.
- **Spark Structured Streaming:** Streaming analytics engine built on Spark DataFrames.
- **Apache Flink:** Low-latency, high-throughput stream processor.
- **Apache Beam:** Unified batch/stream programming model; runs on multiple engines (Spark, Flink, Dataflow).

---

### 4.1 Streaming with Kafka (Python)

```python
from kafka import KafkaProducer, KafkaConsumer

# Producer: Send messages
producer = KafkaProducer(bootstrap_servers='localhost:9092')
producer.send('demo-topic', b'hello world')
producer.flush()

# Consumer: Read messages
consumer = KafkaConsumer('demo-topic', bootstrap_servers='localhost:9092', auto_offset_reset='earliest')
for msg in consumer:
    print(msg.value)
    # break after first message for demo
    break
```

---

### 4.2 Spark Structured Streaming Example

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

# Simulate a stream of numbers (built-in 'rate' source)
stream_df = spark.readStream.format('rate').option('rowsPerSecond', 2).load()

from pyspark.sql.functions import avg

# Running average over micro-batches
agg = stream_df.groupBy().agg(avg('value').alias('running_avg'))

# Output to console
query = agg.writeStream.outputMode('complete').format('console').start()
query.awaitTermination(10)  # Run for 10 seconds
```

---

## 5. Integrating Python with Big Data Tools

- **PySpark:** Native Python API for Spark.
- **kafka-python, confluent-kafka:** Python clients for Kafka.
- **Apache Beam Python SDK:** Write batch/streaming pipelines in Python, run on various backends (Dataflow, Flink, Spark).
- **pandas, polars:** For small to medium-sized data, prototyping, or ETL pre/post-processing.

---

## 6. Performance & Scaling

- **Partitioning:** Distribute data evenly for parallelism.
- **Tuning:** Adjust memory, executor, and shuffle parameters.
- **Caching:** Persist intermediate results in memory for iterative processing.
- **Resource Monitoring:** Use Spark UI, Ganglia, or other cluster monitoring tools.
- **Fault Tolerance:** Automatic recovery from worker failures.

---

## 7. Best Practices

- Start small: Develop and test on data samples before scaling up.
- Use schema inference cautiously; specify schemas for production jobs.
- Monitor resource usage and job performance.
- Handle late, missing, or out-of-order data in streaming.
- Use checkpointing and watermarking for exactly-once or at-least-once guarantees.
- Document and automate pipeline deployments.

---

## References

- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Spark Structured Streaming Guide](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)
- [Apache Beam Python SDK](https://beam.apache.org/get-started/quickstart-py/)
- [Apache Flink](https://flink.apache.org/)
