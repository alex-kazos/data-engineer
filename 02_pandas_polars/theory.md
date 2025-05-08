# Section 2: Data Manipulation with Pandas & Polars

## Overview
Pandas and Polars are powerful Python libraries for data manipulation and analysis. Pandas is the de facto standard for tabular data; Polars is a modern, high-performance alternative.

---

## 1. What are Pandas and Polars?
- **Pandas:** Mature, feature-rich, widely used for tabular data (DataFrames).
- **Polars:** Newer, optimized for speed and parallelism, especially with big data.
- **When to use:** Use Pandas for compatibility and ecosystem; Polars for speed and scalability.

## 2. Installing and Importing
```python
# Pandas
pip install pandas
import pandas as pd

# Polars
pip install polars
import polars as pl
```

## 3. DataFrames and Series
- **DataFrame:** 2D table (rows/columns)
- **Series:** 1D array (single column)

```python
# Pandas
import pandas as pd
df = pd.DataFrame({'a': [1,2], 'b': [3,4]})
s = pd.Series([1,2,3])

# Polars
import polars as pl
df = pl.DataFrame({'a': [1,2], 'b': [3,4]})
```
- Inspect: `df.head()`, `df.info()` (pandas), `df.describe()`

## 4. Reading and Writing Data
- **CSV:** `pd.read_csv()`, `pl.read_csv()`
- **Excel:** `pd.read_excel()`
- **Parquet:** `pd.read_parquet()`, `pl.read_parquet()`
- **SQL:** `pd.read_sql()`
- **JSON:** `pd.read_json()`, `pl.read_json()`
- Write: `df.to_csv()`, `df.write_csv()`

## 5. Data Wrangling
- **Selecting:** `df['col']`, `df[['a','b']]`, `df.iloc[0]`, `df.loc[0]`
- **Filtering:** `df[df['a'] > 1]`
- **Sorting:** `df.sort_values('a')`, `df.sort('a')` (Polars)
- **Grouping:** `df.groupby('col').agg({'x':'sum'})`
- **Reshaping:** `df.pivot()`, `df.melt()`

## 6. Handling Missing Data
- Detect: `df.isnull()`, `df.isna()`
- Drop: `df.dropna()`
- Fill: `df.fillna(0)`

## 7. Merging/Joining Data
- `pd.merge(df1, df2, on='key')`
- `pl.concat([df1, df2])`, `df.join(df2, on='key')`

## 8. Performance Tips
- Use Polars for large/complex data
- Use categorical types for memory savings
- Vectorized ops over loops
- Avoid `apply` in Pandas for speed

## 9. Best Practices & Gotchas
- Always inspect data after loading
- Watch for dtype mismatches
- Use `copy()` to avoid SettingWithCopyWarning

## References
- [Pandas Docs](https://pandas.pydata.org/)
- [Polars Docs](https://pola-rs.github.io/polars/py-polars/html/reference/)
- [Awesome Pandas](https://github.com/tommyod/awesome-pandas)
