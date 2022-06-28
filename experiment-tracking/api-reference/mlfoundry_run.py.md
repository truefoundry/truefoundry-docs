<!-- markdownlint-disable -->

<a href="../../../mlfoundry/mlfoundry_run.py#L0"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

# <kbd>module</kbd> `mlfoundry_run.py`






---

## <kbd>class</kbd> `MlFoundryRun`
MlFoundryRun. 

<a href="../../../mlfoundry/mlfoundry_run.py#L54"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `__init__`

```python
__init__(
    experiment_id: str,
    run_id: str,
    log_system_metrics: bool = False,
    **kwargs
)
```

__init__. 



**Args:**
 
 - <b>`experiment_id`</b> (str):  experiment_id 
 - <b>`run_id`</b> (str):  run_id 
 - <b>`log_system_metrics`</b> (bool):  log_system_metrics 


---

#### <kbd>property</kbd> dashboard_link

Get Mlfoundry dashboard link for a `run` 

---

#### <kbd>property</kbd> fqn





---

#### <kbd>property</kbd> project_name





---

#### <kbd>property</kbd> run_id





---

#### <kbd>property</kbd> run_name





---

#### <kbd>property</kbd> status







---

<a href="../../../mlfoundry/mlfoundry_run.py#L763"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `auto_log_metrics`

```python
auto_log_metrics(
    model_type: ModelType,
    data_slice: DataSlice,
    predictions: Collection[Any],
    actuals: Optional[Collection[Any]] = None,
    class_names: Optional[List[str]] = None,
    prediction_probabilities=None
) → ComputedMetrics
```

auto_log_metrics. 



**Args:**
 
 - <b>`model_type`</b> (enums.ModelType):  model_type 
 - <b>`data_slice`</b> (enums.DataSlice):  data_slice 
 - <b>`predictions`</b> (Collection[Any]):  predictions 
 - <b>`actuals`</b> (Optional[Collection[Any]]):  actuals 
 - <b>`class_names`</b> (Optional[List[str]]):  class_names prediction_probabilities: 



**Returns:**
 ComputedMetrics: 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L336"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `download_artifact`

```python
download_artifact(path: str, dest_path: Optional[str] = None) → str
```

Downloads a logged `artifact` associated with the current `run`. 



**Args:**
 
 - <b>`path`</b> (str):  Source artifact path. 
 - <b>`dest_path`</b> (Optional[str], optional):  Absolute path of the local destination  directory. If a directory path is passed, the directory must already exist.  If not passed, the artifact will be downloaded to a newly created directory  in the local filesystem. 



**Returns:**
 
 - <b>`str`</b>:  Path of the directory where the artifact is downloaded. 



**Examples:**
 ```python
import os
import mlfoundry

with open("artifact.txt", "w") as f:
    f.write("hello-world")

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project", run_name="svm-with-rbf-kernel"
)

run.log_artifact(local_path="artifact.txt", artifact_path="my-artifacts")

local_path = run.download_artifact(path="my-artifacts")
print(f"Artifacts: {os.listdir(local_path)}")

run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L1170"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `download_model`

```python
download_model(dest_path: str, step: Optional[int] = None)
```

Download logged model for the current `run` at a particular `step` in a  local directory. Downloads the latest logged run (one logged at the  largest step) if step is not passed.xs 





**Args:**
 
 - <b>`dest_path`</b> (str):  local directory where the model will be downloaded.  if `dest_path` does not exist, it will be created.  If dest_path already exist, it should be an empty directory. 
 - <b>`step`</b> (int, optional):  step/iteration at which the model is being logged  If not passed, the latest logged model (model logged with the  highest step) is downloaded 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L242"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `end`

```python
end()
```

End a `run`. 

This function marks the run as `FINISHED`. 



**Example:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
     project_name="my-classification-project", run_name="svm-with-rbf-kernel"
)
# ...
# Model training code
# ...
run.end()
``` 

In case the run was created using the context manager approach, We do not need to call this function. ```python
import mlfoundry

client = mlfoundry.get_client()
with client.create_run(
     project_name="my-classification-project", run_name="svm-with-rbf-kernel"
) as run:
     # ...
     # Model training code
     ...
# `run` will be automatically marked as `FINISHED` or `FAILED`.
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L504"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_dataset`

```python
get_dataset(dataset_name: str) → Optional[DataSet]
```

Get a logged dataset by `dataset_name`. 



**Args:**
 
 - <b>`dataset_name`</b> (str):  Name of the dataset. 



**Returns:**
 
 - <b>`Optional[DataSet]`</b>:  Returns logged dataset as an instance of  the `DataSet` class. If the dataset was not logged or  `only_stats` was set to `True` while logging the dataset,  the function will return `None`. 



**Examples:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project", run_name="svm-with-rbf-kernel"
)

features = [
    {"feature_1": 1, "feature_2": 1.2, "feature_3": "high"},
    {"feature_1": 2, "feature_2": 3.5, "feature_3": "medium"},
]
run.log_dataset(
    dataset_name="train",
    features=features,
    actuals=[1.2, 1.3],
    predictions=[3.1, 4.5],
)
dataset = run.get_dataset("train")
print(dataset.features) # This will be in Pandas DataFrame type.
print(dataset.predictions) # This will be in Pandas Series type.
print(dataset.actuals) # This will be in Pandas Series type.
run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L1071"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_metrics`

```python
get_metrics(
    metric_names: Optional[Iterable[str]] = None
) → Dict[str, List[Metric]]
```

Get metrics logged for the current `run` grouped by metric name. 



**Args:**
 
 - <b>`metric_names`</b> (Optional[Iterable[str]], optional):  A list of metric names  For which the logged metrics will be fetched. If not passed, then all  metrics logged under the `run` is returned. 



**Returns:**
 
 - <b>`Dict[str, List[Metric]]`</b>:  A dictionary containing metric name to list of metrics  map. 



**Examples:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project", run_name="svm-with-rbf-kernel"
)
run.log_metrics(metric_dict={"accuracy": 0.7, "loss": 0.6}, step=0)
run.log_metrics(metric_dict={"accuracy": 0.8, "loss": 0.4}, step=1)

metrics = run.get_metrics()
for metric_name, metric_history in metrics.items():
    print(f"logged metrics for metric {metric_name}:")
    for metric in metric_history:
         print(f"value: {metric.value}")
         print(f"step: {metric.step}")
         print(f"timestamp_ms: {metric.timestamp}")
         print("--")

run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L1152"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_model`

```python
get_model(dest_path: Optional[str] = None, step: Optional[int] = None, **kwargs)
```

Deserialize and return the logged model object for the current `run`  and given step, returns the latest logged model(one logged at the  largest step) if step is not passed 



**Args:**
 
 - <b>`dest_path`</b> (Optional[str], optional):  The path where the model is downloaded before deserializing. 
 - <b>`step`</b> (int, optional):  step/iteration at which the model was logged  If not passed, the latest logged model (model logged with the  highest step) is returned 
 - <b>`kwargs`</b>:  Keyword arguments to be passed to the de-serializer. 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L1127"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_params`

```python
get_params() → Dict[str, str]
```

Get all the params logged for the current `run`. 



**Returns:**
 
 - <b>`Dict[str, str]`</b>:  A dictionary containing the parameters. The keys in the dictionary  are parameter names and the values are corresponding parameter values. 



**Examples:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project"
)
run.log_params({"learning_rate": 0.01, "epochs": 10})
print(run.get_params())

run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L699"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_tags`

```python
get_tags() → Dict[str, str]
```

Returns all the tags set for the current `run`. 



**Returns:**
 
 - <b>`Dict[str, str]`</b>:  A dictionary containing tags. The keys in the dictionary  are tag names and the values are corresponding tag values. 



**Examples:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project"
)
run.set_tags({"nlp.framework": "Spark NLP"})
print(run.get_tags())

run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L381"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_artifact`

```python
log_artifact(local_path: str, artifact_path: Optional[str] = None)
```

Logs an artifact for the current `run`. 

An `artifact` is a local file or directory. This function stores the `artifact` at the remote artifact storage. 



**Args:**
 
 - <b>`local_path`</b> (str):  Local path of the file or directory. 
 - <b>`artifact_path`</b> (Optional[str], optional):  Relative destination path where  the `artifact` will be stored. If not passed, the `artifact` is stored  at the root path of the `run`'s artifact storage. The passed  `artifact_path` should not start with `mlf/`, as `mlf/` directory is  reserved for `mlfoundry`. 



**Examples:**
 ```python
import os
import mlfoundry

with open("artifact.txt", "w") as f:
    f.write("hello-world")

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project", run_name="svm-with-rbf-kernel"
)

run.log_artifact(local_path="artifact.txt", artifact_path="my-artifacts")

run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L433"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_dataset`

```python
log_dataset(
    dataset_name: str,
    features,
    predictions=None,
    actuals=None,
    only_stats: bool = False
)
```

Log a tabular dataset for the current `run`. 

Log a dataset associated with a run. A dataset is a collection of features, predictions and actuals. Datasets are uniquely identified by the `dataset_name` under a run. They are immutable, once successfully logged, overwriting it is not allowed. Mixed types are not allowed in features, actuals and predictions. However, there can be missing data in the form of None, NaN, NA. 

The statistics can be visualized in the mlfoundry dashboard. 



**Args:**
 
 - <b>`dataset_name`</b> (str):  Name of the dataset. Dataset name should only contain  letters(a-b, A-B), numbers (0-9), underscores (_) and hyphens (-). 
 - <b>`features`</b>:  Features associated with this dataset.  This should be either pandas DataFrame or should be of a  data type which can be converted to a DataFrame. 
 - <b>`predictions`</b> (Iterable,optional):  Predictions associated with this dataset and run. This  should be either pandas Series or should be of a data type which  can be converted to a Series. This is an optional argument. 
 - <b>`actuals`</b> (Iterable, optional):  Actuals associated with this dataset and run. This  should be either pandas Series or should be of a data type which  can be converted to a Series. This is an optional argument. 
 - <b>`only_stats`</b> (bool, optional):  If True, then the dataset  (features, predictions, actuals) is not saved. Only statistics and  the dataset schema will be persisted. Default is False. 



**Examples:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project", run_name="svm-with-rbf-kernel"
)

features = [
    {"feature_1": 1, "feature_2": 1.2, "feature_3": "high"},
    {"feature_1": 2, "feature_2": 3.5, "feature_3": "medium"},
]
run.log_dataset(
    dataset_name="train",
    features=features,
    actuals=[1.2, 1.3],
    predictions=[3.1, 4.5],
)

run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L803"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_dataset_stats`

```python
log_dataset_stats(
    df,
    data_slice: DataSlice,
    data_schema: Schema,
    model_type: ModelType,
    shap_values=None
)
```

log_dataset_stats. 



**Args:**
  df: 
 - <b>`data_slice`</b> (enums.DataSlice):  data_slice 
 - <b>`data_schema`</b> (Schema):  data_schema 
 - <b>`model_type`</b> (enums.ModelType):  model_type shap_values: 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L1232"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_images`

```python
log_images(images: Dict[str, Image], step: int = 0)
```

Log images under the current `run` at the given `step`. 

Use this function to log images for a `run`. `PIL` package is needed to log images. To install the `PIL` package, run `pip install pillow`. 



**Args:**
 
 - <b>`images`</b> (Dict[str, "mlfoundry.Image"]):  A map of string image key to instance of  `mlfoundry.Image` class. The image key should only contain alphanumeric,  hyphens(-) or underscores(_). For a single key and step pair, we can log only  one image. 
 - <b>`step`</b> (int, optional):  Training step/iteration for which the `images` should be  logged. Default is `0`. 



**Examples:**
 # Logging images from different sources 

```python
import mlfoundry
import numpy as np
import PIL.Image

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project",
)

imarray = np.random.randint(low=0, high=256, size=(100, 100, 3))
im = PIL.Image.fromarray(imarray.astype("uint8")).convert("RGB")
im.save("result_image.jpeg")

images_to_log = {
    "logged-image-array": mlfoundry.Image(data_or_path=imarray),
    "logged-pil-image": mlfoundry.Image(data_or_path=im),
    "logged-image-from-path": mlfoundry.Image(data_or_path="result_image.jpeg"),
}

run.log_images(images_to_log, step=1)
run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L545"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_metrics`

```python
log_metrics(metric_dict: Dict[str, Union[int, float]], step: int = 0)
```

Log metrics for the current `run`. 

A metric is defined by a metric name (such as "training-loss") and a floating point or integral value (such as `1.2`). A metric is associated with a `step` which is the training iteration at which the metric was calculated. 



**Args:**
 
 - <b>`metric_dict`</b> (Dict[str, Union[int, float]]):  A metric name to metric value map.  metric value should be either `float` or `int`. This should be  a non-empty dictionary. 
 - <b>`step`</b> (int, optional):  Training step/iteration at which the metrics  present in `metric_dict` were calculated. If not passed, `0` is  set as the `step`. 



**Examples:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project"
)
run.log_metrics(metric_dict={"accuracy": 0.7, "loss": 0.6}, step=0)
run.log_metrics(metric_dict={"accuracy": 0.8, "loss": 0.4}, step=1)

run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L1187"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_model`

```python
log_model(model, framework: Union[ModelFramework, str], step: int = 0, **kwargs)
```

Serialize and log a model for the current `run`. Each logged model is  associated with a step. After logging model at a particular step  we cannot overwrite it. 



**Args:**
 
 - <b>`model`</b>:  The model object 
 - <b>`framework`</b> (Union[enums.ModelFramework, str]):  Model Framework. Ex:- pytorch,  sklearn, tensorflow etc. The full list of supported frameworks can be  found in `mlfoundry.enums.ModelFramework`. 
 - <b>`step`</b> (int, optional):  step/iteration at which the model is being logged  If not passed, `0` is set as the `step`. 
 - <b>`kwargs`</b>:  Keyword arguments to be passed to the serializer. 



**Examples:**
 ### sklearn ```python
import mlfoundry
import numpy as np
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project"
)
X = np.array([[-1, -1], [-2, -1], [1, 1], [2, 1]])
y = np.array([1, 1, 2, 2])
clf = make_pipeline(StandardScaler(), SVC(gamma='auto'))
clf.fit(X, y)

run.log_model(clf, "sklearn")


---

<a href="../../../mlfoundry/mlfoundry_run.py#L600"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_params`

```python
log_params(param_dict: Union[Mapping[str, Any], Namespace])
```

Logs parameters for the run. 

Parameters or Hyperparameters can be thought of as configurations for a run. For example, the type of kernel used in a SVM model is a parameter. A Parameter is defined by a name and a string value. Parameters are also immutable, we cannot overwrite parameter value for a parameter name. 



**Args:**
 
 - <b>`param_dict`</b> (ParamsType):  A parameter name to parameter value map.  Parameter values are converted to `str`. 



**Examples:**
 ### Logging parameters using a `dict`. ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project"
)
run.log_params({"learning_rate": 0.01, "epochs": 10})

run.end()
``` 

### Logging parameters using `argparse` Namespace object ```python
import argparse
import mlfoundry

parser = argparse.ArgumentParser()
parser.add_argument("-batch_size", type=int, required=True)
args = parser.parse_args()

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project"
)
run.log_params(args)
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L1278"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_plots`

```python
log_plots(
    plots: Dict[str, Union[ForwardRef('pyplot'), ForwardRef('Figure'), ForwardRef('Figure'), Plot]],
    step: int = 0
)
```

Log custom plots under the current `run` at the given `step`. 

Use this function to log custom matplotlib, plotly plots. 



**Args:**
  plots (Dict[str, "matplotlib.pyplot", "matplotlib.figure.Figure", "plotly.graph_objects.Figure", Plot]):  A map of string plot key to the plot or figure object.  The plot key should only contain alphanumeric, hyphens(-) or  underscores(_). For a single key and step pair, we can log only  one image. 
 - <b>`step`</b> (int, optional):  Training step/iteration for which the `plots` should be  logged. Default is `0`. 





**Examples:**
 ### Logging a plotly figure ```python
import mlfoundry
import plotly.express as px

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project",
)

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

### Logging a matplotlib plt or figure ```python
import mlfoundry
from matplotlib import pyplot as plt
import numpy as np

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project",
)

t = np.arange(0.0, 5.0, 0.01)
s = np.cos(2 * np.pi * t)
(line,) = plt.plot(t, s, lw=2)

plt.annotate(
    "local max",
    xy=(2, 1),
    xytext=(3, 1.5),
    arrowprops=dict(facecolor="black", shrink=0.05),
)

plt.ylim(-2, 2)

plots_to_log = {"cos-plot": plt, "cos-plot-using-figure": plt.gcf()}

run.log_plots(plots_to_log, step=1)
run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_run.py#L663"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `set_tags`

```python
set_tags(tags: Dict[str, str])
```

Set tags for the current `run`. 

Tags are "labels" for a run. A tag is represented by a tag name and value. 



**Args:**
 
 - <b>`tags`</b> (Dict[str, str]):  A tag name to value map.  Tag name cannot start with `mlf.`, `mlf.` prefix  is reserved for mlfoundry. Tag values will be converted  to `str`. 



**Examples:**
 ```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(
    project_name="my-classification-project"
)
run.set_tags({"nlp.framework": "Spark NLP"})

run.end()
``` 


