# Fetching Inference Data

This section describes how a user can fetch the inference data with a python API which can be used to re-train the model on real time data.

For example, user can download the the inference data of past one month in which actual values are also logged with the following API.

```python
import mlfoundry as mlf
client = mlf.get_client()

inference_data = client.get_inference_dataset(
    model_fqn="",
    start_time=datetime.now() - timedelta(days=30),
    end_time=datetime.now(),
    actual_value_required=True
)
```

The returned object `inference_data` is of class `GetDatasetResponse` defined below:

```python
class GetDatasetResponse(BaseModel):
    total_rows: int
    data: List[DatasetData]

class DatasetData(BaseModel):
    data_id: str
    features: Dict[str, Union[int, float, str]]
    actual: Dict[str, Union[int, float, str]]
    raw_data: Dict
    created_at: datetime
```


