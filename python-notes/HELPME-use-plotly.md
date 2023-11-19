# plotly-notes

### Add an empty facet as a separator.
```python
fig = px.density_heatmap(
    df, x='x', y='y', facet_col='facet',
    category_orders={
        'facet': ['A', 'B'] + [''] + ['C', 'D']
    }
)
```

### Remove variable name from facet.
```python
fig = px.density_heatmap(
    df, x='x', y='y', facet_col='facet'
)
fig.for_each_annotation(lambda a: a.update(text=a.text.split("=")[-1]))
```

### Don't share x axis in row facet.
```python
fig = px.scatter(
    df, x='x', y='y', facet_row='facet'
)
fig.update_xaxes(matches=None)
```
