# Concepts

### Model: 
A model is a machine learning model logged in truefoundry. A model is identified by a unique `model_fqn`. Every model has a name which must be unique per user. 
### Model Version: 
Every model can have multiple versions identified by an integer. Each model version can be uniquely identified by the `model_version_fqn`. 

### Features:
* These are the input features for a model. Each feature is defined by a `name` and `type`.
* Feature type can be int, float or string
```
feature = {
    "name": "feature_name",
    "type": "float"
}
```
* **Note**: int or float type features are considered numeri, whereas string type are considered categorical. For eg for a binary value feature (0 or 1). Feature should ideally be defined as string to c

### Predictions and Actuals: 
Truefoundry currently supports monitoring for two type s of prediction:

* **Categorical**: Use this when the output is a set of values. For eg: binary classification problems, multi-class classification problems etc. 
* **Numeric**: Use this when the output of a model is numeric value. For eg. regression problems. 

### Model Schema: 
* Model Schema comprises of the input features (names and data types) and the prediction type of the model.
* Here is a sample model schema
```
model_schema = {
    "features": [
        {
            "name": "feature1",
            "type": "float"
        },
        {
            "name": "feature2",
            "type": "int"
        },
        {
            "name": "feature3",
            "type": "string"
        }
    ],
    "prediction_type": "categorical"
}
```
* Model schema is associated to a model version.
* **Editing the Schema:** Model Schema of a model version can be edited (only addition of features allowed)
* **Changing Shema Across Model Versions**: Model Schema can be modified across different versions but the union of model schema's across all the versions should not be conflicting i.e. adding/deleting features is allowed, but not changing the data/type for a feature. Similarly chaning the prediction type is not allowed.
### Custom Metrics:
Custom Metrics are the metrics that can be used to measure the performance of models. These are different for different prediction type of models.

Currently we support adding a few loss functions as custom metrics for categorical and numeric prediction types. 

Supported Custom Metrics are:

* Categorical Prediction: `mean_square_error` and `mean_absolute_error`
* Numeric Prediction: `log_loss`