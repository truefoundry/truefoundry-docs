# Log Parameters

### Logging the hyperparameters
Hyperparameters are independent variables for a run used to control the learning process. We can capture the hyperparameters using the `log_params` method.
Once set, the hyperparameters are immutable.

Note that parameter values are stringified before storing.

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(project_name="sklearn-iris-example", run_name="svm-model")

run.log_params({"cache_size": 200.0, "kernel": "linear"})

run.end()
```

### Viewing logged parameter in dashboard
These logged parameters can be seen in the MLFoundry dashboard. 
![Viewing Logged Parameters](../../assets/log-params.png)

### Filtering runs bases on parameter value

To filters runs, click on top right corner of the screen to apply the required filter.

![Filter based on parameter value](../../assets/filter-params.png)

### Capturing command-line arguments

We can capture command-line arguments directly from the `argparse.Namespace` object.


```python
import argparse
import mlfoundry

parser = argparse.ArgumentParser()
parser.add_argument("-batch_size", type=int, required=True)
args = parser.parse_args()

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project")

run.log_params(args)

run.end()
```

### How can I programmatically parameters for a run?

You can use the `get_params` method. It returns a dictionary

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.get_run("run-id-of-the-run")

print(run.get_params())
```
