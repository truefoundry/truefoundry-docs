# Adding MLFoundry to your code

After getting the api-key and logging in as described in the [setup](setup.md) section. Follow the following steps.

### Create a run and start tracking
Initialize a new run. This initialization will start tracking system metrics automatically. This run will now appear on the mlfoundry dashboard.

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(project_name="iris-demo", run_name="svm-model")
```

### Track parameters
Save hyperparameters for your experiment.

```python
run.log_params({"learning_rate": 0.001})
```

### Track metrics
Save metrics for your model.

```python
run.log_metrics({"accuracy": 0.7, "loss": 0.6})
```
### End a run and stop tracking
After completion of your experiment, you can end a run. This function marks the run as “finished” and stops tracking system metrics.

```python
run.end()
```
