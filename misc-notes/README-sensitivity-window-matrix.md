# sensitivity-window-matrix

## Description
+ For an alerting system which fires at a regular frequency of T, 
we can measure its sensitivity across various detection windows prior 
to an event of interest by treating the start of the detection window
and the end of the detection window as coordinates in a plot, and
calculating the sensitivity for each of these coordinates.

## Code
```python
class WindowMatrix:
    def __init__(
        self, 
        alert_matrix: np.ndarray
    ):
        self.alert_matrix = alert_matrix
        self.n_series = self.alert_matrix.shape[0]
        self.series_length = self.alert_matrix.shape[1]
        
    def get_window_matrix(self, stride: int) -> np.ndarray:
        out = np.empty(shape=(self.series_length // stride, self.series_length // stride))
        for i, w0 in enumerate(range(0, self.series_length, stride)):
            for j, w1 in enumerate(range(0, self.series_length, stride)):
                if w0 >= w1:
                    out[i, j] = np.nan
                else:    
                    cnt = (alert_matrix[:, w0:w1+1].sum(axis=1) > 0) / alert_matrix.shape[0] * 100
                    out[i, j] = sum(cnt)
        return out
    
    def create_figure(self, stride: int = 1):
        window_matrix = self.get_window_matrix(stride=stride)
        
        fig = px.imshow(
            window_matrix.T,
            height=600,
            x=np.arange(-self.series_length, 0),
            y=np.arange(-self.series_length, 0),
            zmin=0, zmax=100
        )
        
        fig.update_layout(
            title=f'Sensitivity Window Matrix (n={self.n_series})',
            xaxis_title="Window Start", 
            yaxis_title="Window End",
            color_continuous_scale=px.colors.sequential.RdBu_r
        )
        
        return fig
```

## Visualisation

For an alerter that randomly alerts at each T according to a Bernoulli(0.1):
```python
alert_matrix = np.random.binomial(n=1, p=0.1, size=(200, 100))
```

![sensitivity-window-matrix-random-1.png](images%2Fsensitivity-window-matrix-random-1.png)

For an alerter that randomly alerts at each T in the final 50T according to a Bernoulli(0.05):
```python
alert_matrix = np.hstack([
    np.zeros((200, 50)),
    np.random.binomial(n=1, p=0.05, size=(200, 50))
])
```

![sensitivity-window-matrix-random-2.png](images%2Fsensitivity-window-matrix-random-2.png)
