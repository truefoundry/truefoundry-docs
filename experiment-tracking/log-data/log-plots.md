# Logging Plots

Mlfoundry allows you to log custom plots under the current `run` at the given `step` using the `log_plot` function.
You can use this function to log custom matplotlib, plotly plots as shown in examples below:

```python
import mlfoundry
from sklearn.metrics import ConfusionMatrixDisplay
import matplotlib.pyplot as plt

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project",
)

ConfusionMatrixDisplay.from_predictions(["spam", "ham"], ["ham", "ham"])

run.log_plots({"confusion_matrix": plt}, step=1)
```

You can visualize the logged plots in the Mlfoundry Dashboard.

![Visualizing the logged plots](../../assets/log-plot.png)

### Logging a plotly figure
```python
import mlfoundry
import plotly.express as px

client = mlfoundry.get_client()
run = client.create_run(project_name="my-classification-project")

df = px.data.tips()
fig = px.histogram(
    df,
    x="total_bill",
    y="tip",
    color="sex",
    marginal="rug",
    hover_data=df.columns,
)

plots_to_log = {
    "distribution-plot": fig,
}

run.log_plots(plots_to_log, step=1)
run.end()
```
You can find this logged image in the dashboard.

![Plotly Image](../../assets/log-plot-plotly.png)