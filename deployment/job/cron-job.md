# Deploy cron job

In this guide, we will deploy a simple cron job using servicefoundry. A cron job runs the defined job on a repeating schedule. This is useful if you want to retrain your model periodically.

// TODO: Refer to the code in the previous section and mention that we need to run it on a schedule. 

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. Keep the workspace _FQN_ handy. If you already have a workspace you can use that.

> **_NOTE:_** A workspace is a resource (CPU, Memory) bound environment where we deploy jobs, services.

### Defining the job

Here we will write a simple python script which prints numbers starting from `0` to a given end.

```
.
└── main.py
```

**`main.py`**
```python
import argparse
import time


def print_numbers(upto: int):
    for i in range(1, upto + 1):
        print(i)
        time.sleep(1)


if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--upto", type=int)
    args = parser.parse_args()

    print_numbers(args.upto)
```

### Deploying the script as cron job

We will deploy the script we wrote as a cron job that will run every 12 hours. You can either deploy using the python APIs or you can deploy using a YAML file and the `servicefoundry deploy` command.

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
    ScheduledTrigger,
)

logging.basicConfig(level=logging.INFO)

job = Job(
    name="cron-job",
    image=Build(
        build_spec=PythonBuild(command="python main.py --upto 30"),
    ),
    trigger=ScheduledTrigger(schedule="0 */12 * * *"),
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

> **_NOTE:_** As defined by `schedule="0 */12 * * *"`, this job will run every 12 hours. You can pass any custom cron expression.