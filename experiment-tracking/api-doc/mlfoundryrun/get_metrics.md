# get\_metrics

get\_metrics returns a list of all the metrics that are logged using MLFoundry for a particular run\_id.

| Parameters  | Description                                                                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| metric\_names | <p>(list[str], default=None) List of metrics to return. Defaults to None in which case all the logged metrics are returned.|

#### Example:

```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name=<project-name>)

metrics_dict = {}

metrics_dict['accuracy_score'] = accuracy_score(y_test, y_hat_test)
metrics_dict['f1_score'] = f1_score(y_test, y_hat_test, average='micro')

mlf_run.log_metrics(metrics_dict)

metrics = mlf_run.get_metrics()
```
