### Capturing tags

Tags are labels for a run. A tag is represented by a string tag name and value.

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project", run_name="my-first-run")

run.set_tags({"env": "development", "task": "classification"})
run.end()
```

### Capturing Hyperparameters

Hyperparameters are independent variables for a run used to control the learning process. We can capture the hyperparameters using the `log_params` method.
Once set, the hyperparameters are immutable.

Note that parameter values are stringified before storing.

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project", run_name="my-first-run")

run.log_params({"learning_rate": 0.01, "epochs": 10})

run.end()
```

#### Capturing command-line arguments

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

#### How can I log version control information?

MLFoundry automatically logs git infos like, commit sha, origin URI as tags. MLFoundry also logs un-commited local changes as an artifact under `mlf/git/uncommitted_changes.patch.txt` path.

```python
local_path = run.downlaod_artifacts("mlf/git/uncommitted_changes.patch.txt", "./")
print(local_path)
```

#### How can I programmatically fetch the tags and parameters for a run?

You can use the `get_tags` and `get_params` methods. Both return a dictionary.

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.get_run("run-id-of-the-run")

print(run.get_tags())
print(run.get_params())
```
