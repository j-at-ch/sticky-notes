# polars-notes

### Set-up
```python
import numpy as np
import polars as pl

df = pl.DataFrame({
    'w': np.arange(100), 
    'x': np.random.rand(100), 
    'y': np.random.choice(['a', 'b'], 100),
    'z': np.random.randint(10, size=100)
})
```


### Permissively cast multiple dataframe columns

```python
dtypes = {
    'w': pl.datatypes.Int32,
    'x': pl.datatypes.Float64,
    'y': pl.datatypes.Utf8,
    'z': pl.datatypes.Int16
}

df = df.with_columns(*[pl.col(k).cast(v) for k, v in dtypes.items()])
```

### Transform iterative windows into a single cell value
Here we use `w` as in index columns and at each row, we transform the 7 
previous values into a cell using `group_by_dynamic`.

```python
(
    df
    .sort(by='w')
    .group_by_dynamic(
        index_column="w", 
        every='1i', 
        period="7i",  
        closed="right", 
        label="right"
    ).agg(
        pl.col('x'),
        pl.col('y')
    )
)
```

### Summarise a newly created column

```python
df.with_columns(
    (pl.col('x') - pl.col('z')).abs().alias('a')
).select('a').mean()
```