<!-- markdownlint-disable -->

<a href="../../../mlfoundry/mlfoundry_api.py#L0"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

# <kbd>module</kbd> `mlfoundry_api.py`
# TO independently test this module, you can run the example in the path python examples/sklearn/iris_train.py 

Besides running pytest 

**Global Variables**
---------------
- **TYPE_CHECKING**
- **SEARCH_MAX_RESULTS_DEFAULT**

---

<a href="../../../mlfoundry/mlfoundry_api.py#L40"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

## <kbd>function</kbd> `init_rest_tracking`

```python
init_rest_tracking(tracking_uri: str, api_key: Optional[str])
```

init_rest_tracking. 



**Args:**
 
 - <b>`tracking_uri`</b> (str):  tracking_uri 
 - <b>`api_key`</b> (Optional[str]):  api_key 


---

<a href="../../../mlfoundry/mlfoundry_api.py#L57"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

## <kbd>function</kbd> `get_client`

```python
get_client(
    tracking_uri: Optional[str] = None,
    inference_store_uri: Optional[str] = None,
    disable_analytics: bool = False,
    api_key: Optional[str] = None
) → MlFoundry
```

Initializes and returns the mlfoundry client. 



**Args:**
 
 - <b>`tracking_uri`</b> (Optional[str], optional):  Custom tracking server URL.  If not passed, by default all the run details are sent to TrueFoundry server 
 - <b>`and can be visualized at https`</b>: //app.truefoundry.com/mlfoundry. Tracking server URL can be also configured using the `MLF_HOST` environment variable. In case environment variable and argument is passed, the URL passed via this argument will take precedence. 
 - <b>`disable_analytics`</b> (bool, optional):  To turn off usage analytics collection, pass `True`.  By default, this is set to `False`. 
 - <b>`api_key`</b> (Optional[str], optional):  API key. 
 - <b>`API key can be found at https`</b>: //app.truefoundry.com/settings. API key can be also configured using the `MLF_API_KEY` environment variable. In case the environment variable and argument are passed, the value passed via this argument will take precedence. 



**Returns:**
 
 - <b>`MlFoundry`</b>:  Instance of `MlFoundry` class which represents a `run`. 



**Examples:**
 ### Get client Set the API key using the `MLF_API_KEY` environment variable. ```
export MLF_API_KEY="MY-API_KEY"
``` 

We can then initialize the client, the API key will be picked up from the environment variable. ```python
import mlfoundry

client = mlfoundry.get_client()
``` 

### Get client with API key passed via argument. ```python
import mlfoundry

API_KEY = "MY-API_KEY"
client = mlfoundry.get_client(api_key=API_KEY)
``` 


---

## <kbd>class</kbd> `MlFoundry`
MlFoundry. 

<a href="../../../mlfoundry/mlfoundry_api.py#L131"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `__init__`

```python
__init__(
    tracking_uri: Optional[str] = None,
    inference_store_uri: Optional[str] = None
)
```

__init__. 



**Args:**
 
 - <b>`tracking_uri`</b> (Optional[str], optional):  tracking_uri 
 - <b>`inference_store_uri`</b> (Optional[str], optional):  inference_store_uri 




---

<a href="../../../mlfoundry/mlfoundry_api.py#L226"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `create_run`

```python
create_run(
    project_name: str,
    run_name: Optional[str] = None,
    tags: Optional[Dict[str, Any]] = None,
    owner: Optional[str] = None,
    log_system_metrics: bool = True,
    **kwargs
) → MlFoundryRun
```

Initialize a `run`. 

In a machine learning experiment `run` represents a single experiment conducted under a project. 

**Args:**
 
 - <b>`project_name`</b> (str):  The name of the project under which the run will be created.  project_name should only contain alphanumerics (a-z,A-Z,0-9) or hyphen (-).  The user must have `ADMIN` or `WRITE` access to this project. 
 - <b>`run_name`</b> (Optional[str], optional):  The name of the run. If not passed, a randomly  generated name is assigned to the run. Under a project, all runs should have  a unique name. If the passed `run_name` is already used under a project, the  `run_name` will be de-duplicated by adding a suffix.  run name should only contain alphanumerics (a-z,A-Z,0-9) or hyphen (-). 
 - <b>`tags`</b> (Optional[Dict[str, Any]], optional):  Optional tags to attach with  this run. Tags are key-value pairs. 
 - <b>`owner`</b> (Optional[str], optional):  Owner of the project. If owner is not passed,  the current user will be used as owner. If the given owner  does not have the project, it will be created under  the current user. 
 - <b>`log_system_metrics`</b> (bool, optional):  Automatically collect system metrics. By default, this is  set to `True`. 

**kwargs:**
 



**Returns:**
 
 - <b>`MlFoundryRun`</b>:  An instance of `MlFoundryRun` class which represents a `run`. 



**Examples:**
 ### Create a run under current user. ```python
import mlfoundry

client = mlfoundry.get_client()

tags = {"model_type": "svm"}
run = client.create_run(
    project_name="my-classification-project", run_name="svm-with-rbf-kernel", tags=tags
)

run.end()
``` 

### Creating a run using context manager. ```python
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

### Create a run in a project owned by a different user. ```python
import mlfoundry

client = mlfoundry.get_client()

tags = {"model_type": "svm"}
run = client.create_run(
    project_name="my-classification-project",
    run_name="svm-with-rbf-kernel",
    tags=tags,
    owner="bob",
)
run.end()
``` 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L203"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_all_projects`

```python
get_all_projects() → List[str]
```

Returns a list of project ids accessible by the current user. 



**Returns:**
 
 - <b>`List[str]`</b>:  A list of project ids. 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L385"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_all_runs`

```python
get_all_runs(project_name: str, owner: Optional[str] = None) → DataFrame
```

Returns all the run name and id present under a project. 

The user must have `READ` access to the project. 

**Args:**
 
 - <b>`project_name`</b> (str):  Name of the project. 
 - <b>`owner`</b> (Optional[str], optional):  Owner of the project. If owner is not passed,  the current user will be used as owner. 

**Returns:**
 
 - <b>`pd.DataFrame`</b>:  dataframe with two columns- run_id and run_name 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L338"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_run`

```python
get_run(run_id: str) → MlFoundryRun
```

Get an existing `run` by the `run_id`. 



**Args:**
 
 - <b>`run_id`</b> (str):  run_id or fqn of an existing `run`. 



**Returns:**
 
 - <b>`MlFoundryRun`</b>:  An instance of `MlFoundryRun` class which represents a `run`. 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L363"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_run_by_fqn`

```python
get_run_by_fqn(run_fqn: str) → MlFoundryRun
```

Get an existing `run` by `fqn`. 

`fqn` stands for Fully Qualified Name. A run `fqn` has the following pattern: owner/project_name/run_name 

If user `bob` has created a run `svm` under the project `cat-classifier`, the `fqn` will be `bob/cat-classifier/svm`. 



**Args:**
 
 - <b>`run_fqn`</b> (str):  `fqn` of an existing run. 



**Returns:**
 
 - <b>`MlFoundryRun`</b>:  An instance of `MlFoundryRun` class which represents a `run`. 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L554"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `get_tracking_uri`

```python
get_tracking_uri()
```

get_tracking_uri. 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L606"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_actuals`

```python
log_actuals(model_name: str, inference_id: str, actuals: 'ValueType')
```

log_actuals. 



**Args:**
 
 - <b>`model_name`</b> (str):  model_name 
 - <b>`inference_id`</b> (str):  inference_id 
 - <b>`actuals`</b> (ValueType):  actuals 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L559"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `log_prediction`

```python
log_prediction(
    model_name: str,
    model_version: str,
    inference_id: str,
    features: 'ValueType',
    predictions: 'ValueType',
    raw_data: Optional[ForwardRef('ValueType')] = None,
    actuals: Optional[ForwardRef('ValueType')] = None,
    occurred_at: Optional[int] = None
)
```

log_prediction. 



**Args:**
 
 - <b>`model_name`</b> (str):  model_name 
 - <b>`model_version`</b> (str):  model_version 
 - <b>`inference_id`</b> (str):  inference_id 
 - <b>`features`</b> (ValueType):  features 
 - <b>`predictions`</b> (ValueType):  predictions 
 - <b>`raw_data`</b> (Optional[ValueType]):  raw_data 
 - <b>`actuals`</b> (Optional[ValueType]):  actuals 
 - <b>`occurred_at`</b> (Optional[int]):  occurred_at 

---

<a href="../../../mlfoundry/mlfoundry_api.py#L441"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `search_runs`

```python
search_runs(
    project_name: str,
    filter_string: str = '',
    run_view_type: str = 'ACTIVE_ONLY',
    order_by: Sequence[str] = ('attribute.start_time DESC',),
    owner: Optional[str] = None
) → Generator[MlFoundryRun, NoneType, NoneType]
```

The user must have `READ` access to the project. Returns an Generator that returns a MLFoundryRun on each next call. All the runs under a project which matches the filter string and the run_view_type are returned. 



**Args:**
 
 - <b>`project_name`</b> (str):  Name of the project. filter_string (str, optional):  Filter query string, defaults to searching all runs. Identifier required in the LHS of a search expression. 
 - <b>`Signifies an entity to compare against. An identifier has two parts separated by a period`</b>:  the type of the entity and the name of the entity. The type of the entity is metrics, params, attributes, or tags. The entity name can contain alphanumeric characters and special characters. 
 - <b>`You can search using two run attributes `</b>:  status and artifact_uri. Both attributes have string values. When a metric, parameter, or tag name contains a special character like hyphen, space, period, and so on, enclose the entity name in double quotes or backticks, params."model-type" or params.`model-type` 


 - <b>`run_view_type`</b> (str, optional):  one of the following values "ACTIVE_ONLY", "DELETED_ONLY", or "ALL" runs. order_by (List[str], optional):  List of columns to order by (e.g., "metrics.rmse"). Currently supported values  are metric.key, parameter.key, tag.key, attribute.key. The ``order_by`` column  can contain an optional ``DESC`` or ``ASC`` value. The default is ``ASC``.  The default ordering is to sort by ``start_time DESC``. owner (Optional[str], optional):  Owner of the project. If owner is not passed, the current user will be used as owner. 



**Examples:**
 ```python
    import mlfoundry as mlf

    client = mlf.get_client()
    with client.create_run(project_name="my-project", run_name="run-1") as run1:
         run1.log_metrics(metric_dict={"accuracy": 0.74, "loss": 0.6})
         run1.log_params({"model": "LogisticRegression", "lambda": "0.001"})

    with client.create_run(project_name="my-project", run_name="run-2") as run2:
         run2.log_metrics(metric_dict={"accuracy": 0.8, "loss": 0.4})
         run2.log_params({"model": "SVM"})

    # Search for the subset of runs with logged accuracy metric greater than 0.75
    filter_string = "metrics.accuracy > 0.75"
    runs = client.search_runs(project_name="my-project", filter_string=filter_string)

    # Search for the subset of runs with logged accuracy metric greater than 0.7
    filter_string = "metrics.accuracy > 0.7"
    runs = client.search_runs(project_name="my-project", filter_string=filter_string)

    # Search for the subset of runs with logged accuracy metric greater than 0.7 and model="LogisticRegression"
    filter_string = "metrics.accuracy > 0.7 and params.model = 'LogisticRegression'"
    runs = client.search_runs(project_name="my-project", filter_string=filter_string)

    # Search for the subset of runs with logged accuracy metric greater than 0.7 and order by accuracy in Descending  order
    filter_string = "metrics.accuracy > 0.7"
    order_by = ["metric.accuracy DESC"]
    runs = client.search_runs(
         project_name="my-project", filter_string=filter_string, order_by=order_by
    )

    ``` 



**Returns:**
 
 - <b>`Genarator[MlFoundryRun, None, None]`</b>:  MLFoundryRuns matching the search query. 


