# Section 8: Big Data & Streaming (Advanced) (Theory)

## Overview
Big data tools allow you to process massive, fast, and varied datasets at scale. Streaming enables real-time analytics.

---

## 1. What is Big Data?
- Volume (size), velocity (speed), variety (types)
- Traditional tools break down at scale

## 2. Batch vs Streaming
- **Batch:** Process large chunks on a schedule (e.g., daily ETL)
- **Streaming:** Process data as it arrives (real-time)

## 3. Apache Spark (PySpark)
- Distributed computing engine
- RDDs: Low-level, immutable distributed collections
- DataFrames: High-level, schema-aware tables
- Transformations (lazy), actions (trigger computation)
- Example:
```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
df = spark.read.csv('data.csv', header=True)
df.groupBy('col').count().show()
```

## 4. Stream Processing
- **Kafka:** Messaging system for ingesting streams
- **Apache Beam:** Unified batch/stream, Python SDK
- **Flink:** Low-latency, high-throughput streaming
- Use for fraud detection, IoT, real-time dashboards

## 5. Integrating Python
- PySpark for Spark
- kafka-python, confluent-kafka for Kafka
- Apache Beam Python SDK

## 6. Performance & Scaling
- Partition data for parallelism
- Tune memory and shuffle settings
- Monitor with built-in UIs

## 7. Best Practices
- Start with small samples
- Monitor resource usage
- Handle late/out-of-order data in streams

## References
- [PySpark Docs](https://spark.apache.org/docs/latest/api/python/)
- [Kafka Docs](https://kafka.apache.org/documentation/)
- [Apache Beam](https://beam.apache.org/)
