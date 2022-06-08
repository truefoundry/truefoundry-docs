# Logging Tabular Datasets

You can log tabular datasets for a run as an artifact using the `log_dataset` function. This also captures statistics about your dataset which you can visualize on the dashboard.

You can find more details [here](../../api-doc/experiment-tracking/mlfoundryrun/log_dataset.md).

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project")

features = [
    {"feature_1": 1, "feature_2": 1.2, "feature_3": "high"},
    {"feature_1": 2, "feature_2": 3.5, "feature_3": "medium"},
    {"feature_1": None, "feature_2": -1, "feature_3": "low"},
]
run.log_dataset(
    dataset_name="train",
    features=features,
    actuals=[1.2, 1.3, 2],
    predictions=[3.1, 4.5, 2],
)

run.end()
```
This is how it looks like on the dashboard.

![Dataset Stats](/assets/guide_exp_dataset_stats.png)

#### Example of storing the Iris dataset

```python
import mlfoundry

from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project")

iris_dataset = load_iris(as_frame=True)

features = iris_dataset.data
actuals = iris_dataset.target.apply(lambda class_index: iris_dataset.target_names[class_index])

X_train, X_test, y_train, y_test = train_test_split(features, actuals, test_size=0.2, stratify=actuals, random_state=42)

run.log_dataset(features=X_train, actuals=y_train, dataset_name="train")
run.log_dataset(features=X_test, actuals=y_test, dataset_name="test")

run.end()
```


#### Can I overwrite an already logged dataset?

No. Datasets once logged are immutable. You have to use a different dataset name to log the updated dataset.

#### How can I load a logged dataset?

You load a dataset logged under a run by using the `get_dataset` function.


```python
import mlfoundry

client = mlfoundry.get_client()
run = client.get_run("run-id-of-the-run")
dataset = run.get_dataset(dataset_name="my-dataset")

print(dataset.features) # This will be in Pandas DataFrame type.
print(dataset.predictions) # This will be in Pandas Series type.
print(dataset.actuals) # This will be in Pandas Series type.
```
