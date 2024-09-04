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



### Render large numbers of points in px.scatter without binning.
If the dataframe has more than 1000 points, then the scatter x axes may be binned when using
the "webgl" renderer. To avoid this, switch renderer to "svg".

```python
px.scatter(df, x='x', y='y', render_mode='svg')
```



### Plot heatmap type data and scatter type data in one figure.

```python
import numpy as np
import pandas as pd
import plotly.express as px

from plotly.subplots import make_subplots

timestamps = pd.date_range('2024-01-01 10:00', '2024-01-01 11:00', freq='s')
values_srs = np.random.rand(len(timestamps))
values_mtx = np.random.rand(len(timestamps), 10)

df = pd.DataFrame({'t': timestamps, 'v': values_srs})

fig = make_subplots(
    rows=2,
    cols=1,
    subplot_titles=['Matrix Heatmap', 'Series Scatter'],
    shared_xaxes=True
)

fig.add_traces(
    px.imshow(
        values_mtx.T,
        x=df.t
    ).data,
    rows=1, cols=1
)

fig.add_traces(
    px.scatter(
        df,
        x='t',
        y='v',
        opacity=0.5,
        render_mode='svg'
    ).data,
    rows=2, cols=1
)

fig.show()
```



### Show built-in discrete color sequences.

```python
fig = px.colors.qualitative.swatches()
fig.show()
```



### Style markers with different symbols in `px.scatter`.

```python
fig = px.scatter(data, x='x', y='y', symbol_sequence=['x'])
fig.show()
```

To see a list of symbol names you can do:
```python
from plotly.validators.scatter.marker import SymbolValidator
print(SymbolValidator().values)
```

At time of writing, this is visualised in the docs [here](https://plotly.com/python/marker-style/).



### Add an "abline" to a pre-built figure. 

```python
def add_abline(a: float, b: float, fig: go.Figure, inplace=False) -> go.Figure:
    x0, x1 = fig.full_figure_for_development(warn=False).layout.xaxis.range
    fig_out = px.line(
        x=[x0, x1],
        y=[a + b * x0, a + b * x1]
    )
    if inplace:
        fig_out = fig.add_traces(
            fig_out.data
        )
    return fig_out
```



### Set a specific bin width for a px.histogram

```python
fig = px.histogram(df, x='x')
fig.update_traces(xbins_size=24)
```
