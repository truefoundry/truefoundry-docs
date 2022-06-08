# get\_dataset

get_dataset returns an already logged dataset.

| Parameters  | Description                                                                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataset_name (str)|  Name of the dataset |

#### Returns

Returns `DataSet` object which will contain
the `features`, `predictions`, `actuals` associated with the dataset.
If `only_stats` was set to `True` while logging the dataset,
this function will return `None`.
If the dataset was not logged, this function will return `None`.

```python
dataset = run.get_dataset("my-dataset")
print(dataset.features) # This will be in Pandas DataFrame type.
print(dataset.predictions) # This will be in Pandas Series type.
print(dataset.actuals) # This will be in Pandas Series type.
```
