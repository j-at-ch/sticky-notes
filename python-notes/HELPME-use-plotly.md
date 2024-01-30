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

### Use Plotly discrete colours.
```python
plotly_colors = px.colors.qualitative.Plotly  # a list of Plotly color scheme hex codes
d3_colors = px.colors.qualitative.D3  # a list of D3 color scheme hex codes
```

### Make subplots and add `px`-style traces using `add_traces`.
```python
from plotly.subplots import make_subplots

fig = make_subplots(
    rows=2,
    cols=1,
    subplot_titles=['Subtitle 1', 'Subtitle 2'],
    row_heights=[0.5, 0.5],
    shared_xaxes=True
)

fig.add_traces(
    px.scatter(df, x='x', y='y').data,
    rows=1, 
    cols=1
)

fig.add_traces(
    px.scatter(df, x='x', y='y').data,
    rows=2, 
    cols=1
)
```

### Add hover data to a px plot.
```python
px.scatter(
    df, x='x', y='y', hover_data=['col-name']
)
```

### Add hover reference x-axis dropdown lines.
```python
# make your fig
fig.update_xaxes(showspikes=True)
```


### Add legend groups to subplots.
Note that this is only effective when both legend types are discrete. Issues arise when mixing 
discrete and continuous.
If you want to hide all grouped traces on a single click, remove the `update_layout` "groupclick" option. 

```python
fig.update_traces(
    {'legendgroup': 'group1', 'legendgrouptitle_text':"First Group Title"}, 
    row=1, col=1
)

fig.update_traces(
    {'legendgroup': 'group2', 'legendgrouptitle_text':"Second Group Title"}, 
    row=2, col=1
)

fig.update_layout(legend=dict(groupclick="toggleitem"))
```