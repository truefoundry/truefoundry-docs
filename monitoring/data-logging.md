# Logging Predictions and Actuals

The inference data (predictions and actuals) can be logged into truefoundry with the `mlfoundry client`

**Note**: Each prediction or actual data packet is associated with a `model_version`. User needs to pass `model_version_fqn` with each log request which can be found from the experimentation tracking section of the truefoundry's dashboard.

```python
import mlfoundry as mlf
client = mlf.get_client()
data_id = uuid.uuid4().hex

client.log_predictions(
    model_version_fqn = "",
    predictions = [
        mlf.Prediction(
            data_id = data_id,
            features = {
                "feature1": 3.33,
                "feature2": "class1",
            },
            prediction_data = {
                "value": "pred_class1",
                "probabilities": {
                    "pred_class1": 0.2,
                    "pred_class2": 0.8
                },
                "shap_values": {
                    "feature1": 0.23,
                    "feature2": 0.77
                }
            },
            occurred_at = datetime.utcnow(),
            raw_data = {"data": "any_data"}
        )
    ]
)

client.log_actuals(
    model_version_fqn = "",
    actuals = [
        mlf.Actual(
            data_id = data_id,
            value = "pred_class2"
        )
    ]
)
```

### Prediction Data Packet
The prediction data packet `mlf.Prediction` has following elements:
* data_id: A unique data id representing a unique inference event.
* features: A dictionary of feature names as keys and feature values as values.
* prediction_data: This comprises of prediction value, prediction_probabilities (only for categorical data), shap_values(optional).
* occurred_at: timestamp at which inference occured. If not passed it is set to the current timestamp at which prediction is getting logged.
* raw_data: this gives user the freedom to log any metadata or additional information associated with the inference.

### Actual Data Packet
The actual data packet `mlf.Actual` has following elements:
* data_id: A unique data id representing a unique inference event for which actual is being logged.
* value: the actual value for the event.

**Note**
* Prediction Data Packet corresponding to a data_id must be logged before logging the actual.
* Actual value may or may not be logged in all the cases. If logged, user can have better visualizations on the monitoring dashboard.
* The data_id must be maintained and saved by the user (truefoundry platform's user) in order to log the actuals correctly. This task can be cumbersome in a few cases, so it can be dealt with some cases with API descibed below.

### Generating data_id from hash of features
This function generates unique id by from with the help of a hash on features and timestamp (optional).

```python
import mlfoundry as mlf
client = mlf.get_client()
data_id = mlf.generate_hash_from_data(
    features = {
        "features1": 1.22,
        "feature2" : "class2"
    }
)
data_id_with_ts = mlf.generate_hash_from_data(
    features = {
        "features1": 1.22,
        "feature2" : "class2"
    },
    timestamp = datetime.now()
)
```
This API can be useful for users who do not want to maintain the data_id. The user can log the actuals by generating data_id from the same features and timestamp(optional) that were used at the time of logging predictions.

