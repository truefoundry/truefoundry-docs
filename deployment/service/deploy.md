# Deploy a Service

You can deploy services on Truefoundry using our Python SDK, or via a YAML file or using our UI. 

Let's consider the service code we wrote in the [previous section](./definition.md).

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. If you already have a workspace you can use that. Keep the [workspace _FQN_](../faq/get-workspace-fqn.md) handy. we will use it for the deployment.

{% tabs %}
{% tab title="Deploying using python API" %}

Here we will use the `Service` class from servicefoundry library to deploy the service. Note that we need to set the host to `0.0.0.0` so that  it can accept connections from outside the container.

**File Structure:**

```
.
├── main.py
├── inference.py
├── requirements.txt
└── deploy.py
```

We defined `main.py`, `inference.py` and `requirements.txt`  [in the previous section](./definition.md).

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry import Build, PythonBuild, Service

logging.basicConfig(level=logging.INFO)
service = Service(
    name="flask",
    image=Build(
        build_spec=PythonBuild(
            command="gunicorn -b 0.0.0.0:8000 main:app",
        ),
    ),
    ports=[{"port": 8000}],
)
deployment = service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

You can deploy the app using, 
```shell
python deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

Note that we need to set the host to `0.0.0.0` so that  it can accept connections from outside the container.

**File Structure:**
```
.
├── main.py
├── inference.py
├── requirements.txt
└── servicefoundry.yaml
```
We defined `main.py`, `inference.py` and `requirements.txt`  [in the previous section](./definition.md).

**`servicefoundry.yaml`**
```yaml
name: flask
components:
  - name: flask
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: tfy-python-buildpack
        command: gunicorn -b 0.0.0.0:8000 main:app
    ports:
      - port: 8000
```
You can deploy the service using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN
```
{% endtab %}
{% endtabs %}


## Build Section

// TODO: We should have a page to explain the Build part and refer to it from here. 




