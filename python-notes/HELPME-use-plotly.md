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

### Add opacity to line.
```python
# helper function
def hex_to_rgba(
    h: str,
    alpha: float
) -> str:
    """Convert colour value in hex format to rgba with alpha.

    :param h: Colour hex value.
    :param alpha: Colour alpha parameter.
    :return: RGBA string presentation of the colour.
    """
    rgb = [int(h.lstrip('#')[i:i+2], 16) for i in (0, 2, 4)]
    return 'rgba' + str(tuple(rgb + [alpha]))

# plotting code
fig = px.line(
    df, x='x', y='y'
)
fig.update_traces(line_color=hex_to_rgba('#636EFA', 0.5))
```

### Add markers to a `px.line` plot.
```python
fig = px.line(
    df, x='x', y='y', markers=True
)
```