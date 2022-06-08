# log\_dataset

Log a dataset associated with a run. A dataset is a collection of `features`,
`predictions` and `actuals`. Datasets are uniquely identified by the `dataset_name`
under a `run`. They are immutable, once successfully logged, overwriting it is not allowed.
Mixed types are not allowed in `features`, `actuals` and `predictions`. However, there can be
missing data in the form of `None`, `NaN`, `NA`.

| Parameters             | Description       |
| -----------------------|-------------------|
| dataset_name (str)     | Name of the dataset. Dataset name should only contain letters, numbers, underscores and hyphens.|
| features               | Features associated with this dataset.  This should be either pandas `DataFrame` or should be of a data type which can be convered to a DataFrame.|
| predictions (Optional) | Predictions associated with this dataset and run. This should be either pandas `Series` or should be of a data type which can be convered to a `Series`. This is an optional argument.|
| actuals (Optional)     | Actuals associated with this dataset and run. This should be either pandas `Series` or should be of a data type which can be convered to a `Series`. This is an optional argument.|
| only_stats (Optional bool) | If True, then the dataset (features, predictions, actuals) is not saved. Only statistics and the dataset schema will be persisted. Default is False.

#### Example

```python
run.log_dataset(
    dataset_name="train",
    features=pd.DataFrame({"feature_1": [1, 2], "feature_2": [1.2, 3.2]}),
    actuals=[1.2, 1.3],
    predictions=[3.1, 4.5],
)
```
