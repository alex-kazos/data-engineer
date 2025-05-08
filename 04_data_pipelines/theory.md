# Section 4: Data Pipelines (ETL/ELT)

## Overview
Data pipelines automate the reliable flow of data from sources (databases, APIs, files) to destinations (data warehouses, lakes, analytics systems). ETL (Extract-Transform-Load) and ELT (Extract-Load-Transform) are foundational paradigms, each suited for different architectures and workloads.

---

## 0. Introduction
Modern organizations rely on automated pipelines for continuous data integration and analytics. ETL and ELT processes ensure data is ingested, cleansed, and made available for analysis or reporting.

## 1. What are Data Pipelines?
- **Definition:** Automated, repeatable workflows that move and transform data between systems.
- **ETL:** Extract → Transform → Load (classic approach, often for on-premises, heavy transformation before loading).
- **ELT:** Extract → Load → Transform (increasingly common with cloud data warehouses; load raw data, then transform in-place).

## 2. Pipeline Design Patterns
- **Modular:** Each step (extract, transform, load) is a discrete, reusable function or task.
- **Idempotent:** Steps produce the same result if run multiple times, avoiding duplicates/corruption.
- **Testable:** Each step can be independently tested.
- **Configurable:** Use configuration files for parameters and environment variables for secrets.
- **Observable:** Integrated logging, metrics, and alerting.

## 3. ETL Steps Explained
- **Extract:** Connect and ingest data from sources (files, APIs, DBs), handling authentication, pagination, and schema drift.
- **Transform:** Clean, validate, enrich, aggregate, and reformat data (e.g., type conversion, deduplication, joining datasets).
- **Load:** Write data to the target system (database, warehouse, data lake), optionally with upsert/merge logic.

```python
import pandas as pd
import logging

def extract(csv_path):
    logging.info(f"Extracting data from {csv_path}")
    return pd.read_csv(csv_path)

def transform(df):
    logging.info("Transforming data: dropping nulls, renaming columns")
    df = df.dropna()
    df = df.rename(columns={'old': 'new'})
    # Example transformation: remove duplicates
    df = df.drop_duplicates()
    # Example: convert date column to datetime
    if 'date' in df.columns:
        df['date'] = pd.to_datetime(df['date'], errors='coerce')
    return df

def load(df, out_path):
    logging.info(f"Loading data to {out_path}")
    df.to_csv(out_path, index=False)
    logging.info("Load complete")
```

## 4. Python Tools for Pipelines
- **Airflow:** Robust scheduling, dependency management, monitoring UI, plug-ins, strong community support. Defines workflows as DAGs (Directed Acyclic Graphs).
- **Prefect:** Modern, Pythonic API, easy local development, hybrid cloud execution, automatic retries, flow parameters.
- **Dagster:** Strong typing, asset management, integrated testing, observability, and modern developer experience.

```python
# Example Airflow DAG structure
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def sample_task():
    print("This is a scheduled task.")

with DAG(
    dag_id="sample_etl_pipeline",
    schedule_interval="@daily",
    start_date=datetime(2024, 1, 1),
    catchup=False,
) as dag:
    task = PythonOperator(
        task_id="print_task",
        python_callable=sample_task,
    )
```

## 5. Writing Modular, Maintainable ETL Code
- **Separate functions/classes:** Each ETL step should be encapsulated.
- **Pass data explicitly:** Avoid hidden state.
- **Parameterize:** Use function arguments and config files for paths/credentials.
- **Reusable components:** Generalize extraction, transformation, and load logic.

```python
import yaml

def run_pipeline(config_path):
    # Load config
    with open(config_path) as f:
        config = yaml.safe_load(f)
    df = extract(config['extract']['path'])
    df = transform(df)
    load(df, config['load']['path'])

# Example config.yaml
# extract:
#   path: input.csv
# load:
#   path: output.csv
```

## 6. Scheduling and Monitoring
- **Orchestrators** (Airflow/Prefect/Dagster) provide:
    - Scheduled runs (cron, time-based)
    - Dependency management
    - Task retries and failure alerts
    - Visualization and logs
- **Sensors:** Wait for files, events, or partitions.
- **Alerts:** Email/Slack on failure.

## 7. Logging, Error Handling, and Retries
- Log inputs, outputs, errors, and durations for every step.
- Use try/except blocks and context managers for resource management.
- Implement retries with exponential backoff on transient errors.
- Integrate with monitoring tools (Prometheus, Sentry, etc.).

```python
import time

def robust_extract(csv_path, retries=3, delay=5):
    for attempt in range(retries):
        try:
            return pd.read_csv(csv_path)
        except Exception as e:
            logging.error(f"Extract failed: {e}, retrying ({attempt+1}/{retries})")
            time.sleep(delay)
    raise RuntimeError(f"Failed to extract after {retries} attempts")
```

## 8. Best Practices
- **Simplicity:** Keep pipeline logic simple and clear.
- **Version control:** Track code, configs, and schema evolution.
- **Documentation:** Describe pipeline steps, dependencies, and configs.
- **Testing:** Unit and integration tests for each step.
- **Security:** Protect credentials with environment variables/secrets managers.
- **Data validation:** Check data quality at each stage.

## References
- [Airflow Docs](https://airflow.apache.org/)
- [Prefect Docs](https://docs.prefect.io/)
- [Dagster Docs](https://docs.dagster.io/)
- [Pandas Docs](https://pandas.pydata.org/docs/)
- [Great Expectations (Data Validation)](https://greatexpectations.io/)
