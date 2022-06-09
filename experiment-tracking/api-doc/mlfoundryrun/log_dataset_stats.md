# log\_dataset\_stats

log\_dataset\_stats computes whylogs profile and different metrics for a dataframe and logs it. If shap\_values\_column\_names exists in the schema input then shap value is also logged for the dataframe.

| Parameters                                        | Description                                                                                                                                                                  |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**df**</mark>           | (pandas.DataFrame) Dataframe to compute the metrics and whylogs profiles                                                                                                     |
| <mark style="color:blue;">**data\_slice**</mark>  | <p>(str or mlf.DataSlice) dataslice that the dataframe belongs to.</p><p>List of available dataslice: <a data-mention href="../enums.md#dataslice">#dataslice</a></p>               |
| <mark style="color:blue;">**data\_schema**</mark> | (str or mlf\_.Schema\_) schema of the dataframe that contains the feature names and target labels. To know more about schema check [#schema](../../../concepts.md#schema "mention") |
| <mark style="color:blue;">**model\_type**</mark>  | (str or mlf.ModelType) List of available ModelType: [#modeltype](../enums.md#modeltype "mention")  |
| <mark style="color:blue;">**shap\_values**</mark>  | (default = None) If shap values is passed, it is used in mlfoundry ui to show feature importance plots.  |

### Example

#### Getting the train and test dataframes

```python
import mlfoundry as mlf
import shap

y_train_prob = clf.predict_proba(X_train)

X_train_df = pd.DataFrame(X_train, columns=iris.feature_names)
X_train_df['targets'] = y_train
X_train_df['predictions'] = y_hat_train
X_train_df['prediction_probabilities'] = list(y_train_prob)

X_test_df = pd.DataFrame(X_test, columns=iris.feature_names)
X_test_df['targets'] = y_test
X_test_df['predictions'] = y_hat_test
```

#### log\_dataset\_stats without shap values

```python
# compute and log stats for train data without shap
mlf_run.log_dataset_stats(
    X_train_df,
    data_slice="train",
    data_schema=mlf.Schema(
        feature_column_names=iris.feature_names,
        prediction_column_name="predictions",
        actual_column_name="targets",
        prediction_probability_column_name="prediction_probabilities"   # to calculate probability related metrics
    ),
    model_type="multiclass_classification",
)
```

#### log\_dataset\_stats along with shap values

```python
# shap value computation
X_train_df1 = pd.DataFrame(X_train, columns=iris.feature_names)
X_test_df1 = pd.DataFrame(X_test, columns=iris.feature_names)
explainer = shap.KernelExplainer(clf.predict_proba, X_train_df1)
shap_values = explainer.shap_values(X_test_df1)

mlf_run.log_dataset_stats(
    X_test_df,
    data_slice="test",
    data_schema=mlf.Schema(
        feature_column_names=iris.feature_names,
        prediction_column_name="predictions",
        actual_column_name="targets"
    ),
    model_type="multiclass_classification",
    ## passing the shap values
    shap_values=shap_values
)
```