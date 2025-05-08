# Section 2: Data Manipulation with Pandas & Polars

## Overview
**Pandas** and **Polars** are two powerful Python libraries for data manipulation and analysis.  
- **Pandas**: The long-standing standard for handling structured/tabular data, with a rich API and deep integration with the Python data ecosystem.
- **Polars**: A newer, lightning-fast DataFrame library, designed for high performance and parallelism, especially with large datasets.

---

## 1. What are Pandas and Polars?
- **Pandas:** 
  - Mature, stable, and widely adopted.
  - Feature-rich for most tabular data tasks (cleaning, aggregating, reshaping, etc.).
  - Large user community and ecosystem.
- **Polars:**
  - Built in Rust, with Python bindings.
  - Faster than Pandas for many operations, especially with large or complex data.
  - Lazy execution mode for query optimization.
  - Parallelized computations by default.
- **When to use:**  
  - Choose **Pandas** for compatibility, documentation, and ecosystem.
  - Choose **Polars** for speed, scalability, and modern syntax.

---

## 2. Installation and Imports

Install via pip:
```bash
pip install pandas
pip install polars
```
Import in Python:
```python
import pandas as pd
import polars as pl
```

---

## 3. Data Structures

### DataFrame
- 2D table (rows and columns)
- Both libraries: `DataFrame` is the primary object

### Series
- 1D array (single column), only in Pandas: `pd.Series`

### Creating DataFrames
```python
# Pandas
import pandas as pd
df_pd = pd.DataFrame({'a': [1, 2], 'b': [3, 4]})
s_pd = pd.Series([1, 2, 3], name='numbers')

# Polars
import polars as pl
df_pl = pl.DataFrame({'a': [1, 2], 'b': [3, 4]})
```

### Inspection
```python
# Pandas
df_pd.head()        # First 5 rows
df_pd.info()        # Schema and non-nulls
df_pd.describe()    # Quick stats

# Polars
df_pl.head()
df_pl.describe()
df_pl.schema
```

---

## 4. Reading and Writing Data

### Reading
```python
# CSV
pd.read_csv('file.csv')
pl.read_csv('file.csv')

# Excel
pd.read_excel('file.xlsx')

# Parquet (columnar, fast)
pd.read_parquet('file.parquet')
pl.read_parquet('file.parquet')

# SQL
pd.read_sql('SELECT * FROM table', conn)

# JSON
pd.read_json('file.json')
pl.read_json('file.json')
```

### Writing
```python
# Pandas
df_pd.to_csv('output.csv', index=False)
df_pd.to_parquet('output.parquet')

# Polars
df_pl.write_csv('output.csv')
df_pl.write_parquet('output.parquet')
```

---

## 5. Data Wrangling & Transformation

### Selecting Data
```python
# Single column
df_pd['a']
df_pl['a']

# Multiple columns
df_pd[['a', 'b']]
df_pl.select(['a', 'b'])
```

### Filtering Rows
```python
df_pd[df_pd['a'] > 1]
df_pl.filter(pl.col('a') > 1)
```

### Sorting
```python
df_pd.sort_values('a', ascending=False)
df_pl.sort('a', reverse=True)
```

### Indexing
```python
df_pd.iloc[0]    # By position
df_pd.loc[0]     # By label
# Polars: use `.row(idx)` for row as tuple
```

### Adding/Modifying Columns
```python
# Pandas
df_pd['c'] = df_pd['a'] + df_pd['b']

# Polars
df_pl = df_pl.with_columns((pl.col('a') + pl.col('b')).alias('c'))
```

### Grouping and Aggregation
```python
# Pandas
df_pd.groupby('a').agg({'b': 'sum'})

# Polars
df_pl.groupby('a').agg(pl.col('b').sum())
```

### Reshaping
```python
df_pd.pivot(index='a', columns='b', values='c')
df_pd.melt(id_vars=['a'])

df_pl.pivot(index='a', columns='b', values='c')
df_pl.melt(id_vars=['a'])
```

---

## 6. Handling Missing Data

### Detecting
```python
df_pd.isnull()
df_pd.isna()
df_pl.null_count()
```

### Dropping
```python
df_pd.dropna()
df_pl.drop_nulls()
```

### Filling
```python
df_pd.fillna(0)
df_pl.fill_null(0)
```

---

## 7. Combining Data

### Concatenation
```python
pd.concat([df1, df2])
pl.concat([df1, df2])
```

### Merging/Joining
```python
pd.merge(df1, df2, on='key', how='inner')
df1.join(df2, on='key', how='inner')        # Polars

# For SQL-like joins in Polars:
df1.join(df2, on='key', how='left')
```

---

## 8. Performance Tips

- **Polars** is faster for large datasets and multi-core use.
- Use categorical (string) types to save memory:  
  `df_pd['col'] = df_pd['col'].astype('category')`
- Prefer vectorized operations (avoid Python loops).
- Avoid `df.apply()` in Pandas; use built-in vectorized methods or map.
- For huge data: try `pl.scan_csv()` for lazy evaluation in Polars.

---

## 9. Best Practices & Common Pitfalls

- Always inspect your data after loading (`head()`, `info()`, `describe()`).
- Beware of dtype mismatches when merging/joining.
- Use `.copy()` in Pandas to avoid `SettingWithCopyWarning`:
  ```python
  df2 = df1.copy()
  ```
- Handle missing values early.
- Document your pipeline steps for reproducibility.

---

## References

- [Pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/)
- [Polars Documentation](https://pola-rs.github.io/polars/py-polars/html/reference/)
- [Awesome Pandas Resources](https://github.com/tommyod/awesome-pandas)
- [Polars User Guide](https://pola-rs.github.io/polars-book/)

---
