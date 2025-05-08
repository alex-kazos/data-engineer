# Section 4: Data Pipelines (ETL/ELT)

## Overview
Data pipelines automate the flow of data from source to destination. ETL (Extract-Transform-Load) and ELT (Extract-Load-Transform) are the core paradigms.

---

## 0. Introduction
Data pipelines automate the movement and transformation of data. ETL (Extract, Transform, Load) and ELT (Extract, Load, Transform) are the main paradigms.

## 1. What are Data Pipelines?
- Automated workflows for moving and transforming data
- ETL: Extract → Transform → Load
- ELT: Extract → Load → Transform (common in cloud)

## 2. Pipeline Design Patterns
- **Modular:** Break into steps/tasks
- **Idempotent:** Safe to run multiple times
- **Testable:** Each step can be tested in isolation

## 3. ETL Steps
- **Extract:** Get data from source (API, DB, file)
- **Transform:** Clean, enrich, reshape
- **Load:** Store in destination (DB, data lake, warehouse)

```python
# Extract
import pandas as pd
data = pd.read_csv('input.csv')

# Transform
cleaned = data.dropna().rename(columns={'old': 'new'})

# Load
cleaned.to_csv('output.csv', index=False)
```

## 4. Python Tools for Pipelines
- **Airflow:** Most popular, UI, scheduling, DAGs
- **Prefect:** Simpler, modern, Pythonic
- **Dagster:** Strong on testing, type safety, assets

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
```

## 5. Writing Modular ETL Code
- Use functions/classes for each step
- Pass data between steps
- Use configs for parameters

```python
def extract(path):
    return pd.read_csv(path)
def transform(df):
    return df.dropna()
def load(df, path):
    df.to_csv(path, index=False)
```

## 6. Scheduling and Monitoring
- Airflow, Prefect, and Dagster have UIs and schedulers
- Use sensors, triggers, alerts

## 7. Logging, Error Handling, Retries
- Log every step
- Catch errors, retry failed steps
- Alert on failure

## 8. Best Practices
- Keep pipelines simple and maintainable
- Use version control
- Document dependencies and configs

## References
- [Airflow Docs](https://airflow.apache.org/)
- [Prefect Docs](https://docs.prefect.io/)
- [Dagster Docs](https://docs.dagster.io/)
