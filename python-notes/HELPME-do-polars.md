# polars-notes

### Set-up
```python
import numpy as np
import polars as pl

df = pl.DataFrame({
    'x': np.random.rand(100), 
    'y': np.random.choice(['a', 'b'], 100),
    'z': np.random.randint(10, size=100)
})
```


### Permissively cast multiple dataframe columns

```python
dtypes = {
    'x': pl.datatypes.Float64,
    'y': pl.datatypes.Utf8,
    'z': pl.datatypes.Int16
}

df = df.with_columns(*[pl.col(k).cast(v) for k, v in dtypes.items()])
```
