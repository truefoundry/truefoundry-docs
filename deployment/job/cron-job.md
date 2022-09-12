# Deploy cron job

In this guide, we will deploy a cron job. A cron job runs the defined job on a repeating schedule. This can be  useful to retrain a model periodically, generate reports and more.

We will use the same setup and code that we defined [in last guide](./deploy.md) . We will modify our deployment config to schedule it as a cron job that will run every 12 hours. 

**You can find the complete code in this example [here](https://github.com/truefoundry/truefoundry-examples/tree/main/deployment/job/cron)**

Make sure you have [Prerequisites](./deploy.md#prerequisites) and [Code](./deploy.md#code-and-dependencies) setup from [the last guide](./deploy.md). 

```
.
├── train.py
└── requirements.txt
```

<details>
  <summary>**`requirements.txt`**</summary>

  ```
  pandas==1.4.4
  numpy==1.22.4
  scikit-learn==1.1.2
  mlfoundry>=0.4.2,<0.5
  servicefoundry>=0.1.91,<0.2.0
  ```

</details>

<details>
  <summary>**`train.py`**</summary>

  ```python
  import mlfoundry
  from sklearn.datasets import load_iris
  from sklearn.model_selection import train_test_split
  from sklearn.pipeline import Pipeline
  from sklearn.preprocessing import StandardScaler
  from sklearn.svm import SVC
  from sklearn.metrics import classification_report

  X, y = load_iris(as_frame=True, return_X_y=True)
  X = X.rename(columns={
          "sepal length (cm)": "sepal_length",
          "sepal width (cm)": "sepal_width",
          "petal length (cm)": "petal_length",
          "petal width (cm)": "petal_width",
  })

  # NOTE:- You can pass these configurations via command line
  # arguments, config file, environment variables.
  X_train, X_test, y_train, y_test = train_test_split(
      X, y, test_size=0.2, random_state=42, stratify=y
  )
  pipe = Pipeline([("scaler", StandardScaler()), ("svc", SVC())])
  pipe.fit(X_train, y_train)
  print(classification_report(y_true=y_test, y_pred=pipe.predict(X_test)))

  # Here we are using Truefoundry's Model Registry, you can push model to any storage 
  run = mlfoundry.get_client().create_run(project_name="iris-classification")
  model_version = run.log_model(
      name="iris-classifier",
      model=model,
      framework="sklearn",
      description="SVC model trained on initial data",
  )
  print(f"Logged model: {model_version.fqn}")
  ```

## Deploying as cron job

You can either deploy using the python APIs or you can deploy using a YAML file and the `servicefoundry deploy` command.

{% tabs %}
{% tab title="Deploying using python API" %}

Here we will use the `Job` class from servicefoundry library to deploy.

```
.
├── main.py
└── deploy.py
```

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry import (
    Build,
    Job,
    PythonBuild,
    Schedule,
)

logging.basicConfig(level=logging.INFO)

job = Job(
    name="cron-job",
    image=Build(
        build_spec=PythonBuild(command="python main.py --upto 30"),
    ),
    trigger=Schedule(schedule="0 */12 * * *"),
)

job.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

You can deploy the cron job using, 
```shell
python deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

```
.
├── main.py
└── servicefoundry.yaml
```

**`servicefoundry.yaml`**
```yaml
name: cron-job
components:
- name: cron-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: python main.py --upto 30
  trigger:
    type: scheduled
    schedule: "0 */12 * * *"
```
You can deploy the training job using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN
```
{% endtab %}
{% endtabs %}

> **_NOTE:_** As defined by `schedule="0 */12 * * *"`, this job will run every 12 hours. You can pass any custom cron expression. The cron format is explained below. 

### Understanding the cron format

The job schedule is a cron expression. It consists of five fields representing the time at which to execute a specified command.
```
* * * * *
| | | | |
| | | | |___ day of week (0-6) (Sunday is 0)
| | | |_____ month (1-12)
| | |_______ day of month (1-31)
| |_________ hour (0-23)
|___________ minute (0-59)
```

If you want to see how to set the right cron format, you can use the UI to set the cron which shows the human-readable format of what the schedule means. 

// TODO: paste UI Screenshot

### Concurrency for Cron Jobs

For cron-jobs, its possible that the previous run of the job hasn't completed while its already time for the job to run again because of the scheduled time. This can happen if we schedule a job to run every 10 mins, and for some reason one instance of the job takes more than 10 mins. At this point, we have three options:

1. Start the new instance of the job even if the previous one is running
2. Do not start the new instance of the job and skip this job run since the previous job is running. 
3. Terminate the current running job and start the new one. 

The desired behavior depends on the exact use-case, but you can achieve all the three scenarios using the `concurrency_policy` setting. The possible options are:

1. **Allow**: Allow jobs to run concurrently (#1 option)
2. **Forbid**: Do not allow conurrent runs (#2 option)
3. **Replace**: Replace the current job with the new one (#3 option)

Concurrency doesn't apply to manually triggered jobs. In that case, it always creates a new job run.

You can pass the concurrency policy to the spec as follows:

// TODO: Add code to pass the concurrency policy

