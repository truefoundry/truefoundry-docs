# Deploy a Job

You can deploy jobs on Truefoundry using our Python SDK, or via a YAML file or using our UI. 

Let's consider the training job code we wrote in the [last section](../job/definition.md). For deploying we will need the workspace FQN 
which you can copy from the workspaces page ([detailed guide here](../faq/get-workspace-fqn.md))

// TODO: Replace with training code and remove the secrets part here
{% tabs %}
{% tab title="Deploying using python API" %}

Here we are using the `Job` class to define the training job. We will use the _FQN_ of the secret containing the mlfoundry API Key and the workspace _FQN_ here. Replace `YOUR_SECRET_FQN`, `YOUR_WORKSPACE_FQN`  with the actual values.

Create `train_deploy.py` file in the same directory containing the `train.py` and `train_requirements.txt` files.

```
.
├── main.py
├── requirements.txt
└── deploy.py
```

**`train_deploy.py`**
```python
# Replace `YOUR_SECRET_FQN`, `YOUR_WORKSPACE_FQN`
# with the actual values.
import logging

from servicefoundry import (
    Build,
    Job,
    PythonBuild,
)

logging.basicConfig(level=logging.INFO)

# NOTE: Here we are defining the image build specification.
# servicefoundry uses this specification to automatically create
# a Dockerfile and build an image,
image = Build(
    build_spec=PythonBuild(
        command="python main.py",
        requirements_path="train_requirements.txt",
    )
)

job = Job(
    name="training-job",
    image=image,
)
job.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

You can deploy the job using, 
```shell
python train_deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

```
.
├── train.py
├── train_requirements.txt
└── train_deploy.yaml
```

**`train_deploy.yaml`**
```yaml
# Replace `YOUR_SECRET_FQN`, `YOUR_RUN_FQN`
# with the actual values.
name: training-job
components:
- name: training-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: python train.py
      requirements_path: train_requirements.txt
  env:
  - name: MLF_API_KEY
    value: tfy-secret://YOUR_SECRET_FQN
```

You can deploy the training job using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN --file train_deploy.yaml
```
Run the above command from the same directory containing the `train.py` and `train_requirements.txt` files.

{% endtab %}
{% endtabs %}

## Build Section

// TODO: We should have a page to explain the Build part and refer to it from here. 

## Running the same job again

TODO: // Fill up this section and add the UI part to retrigger the job also.

You can also configure jobs to run on a fixed time schedule as described in the [next section](cron-job.md)



