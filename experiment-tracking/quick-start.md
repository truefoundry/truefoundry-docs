### Set up mlfoundry

1. [Sign-up](https://app.truefoundry.com/signup) and login into your account.

2. Install mlfoundry library on your system.

3. Login to the mlfoundry library on your system. The CLI will prompt for an API key. Find your API key [here](https://app.truefoundry.com/settings).

```
pip install mlfoundry
mlfoundry login
```
On a notebook environment you can use the `login` function.

```python
import mlfoundry
mlfoundry.login()
```

### Create a run and start tracking
Initialize a new run. This initialization will start tracking system metrics automatically. This run will now appear on the mlfoundry dashboard.

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project", run_name="my-first-run")
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
