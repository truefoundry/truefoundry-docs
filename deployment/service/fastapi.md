# Deploy a FastAPI service using servicefoundry

In this guide, we will deploy a [FastAPI service](https://fastapi.tiangolo.com/) using servicefoundry. FastAPI is modern, intuitive web framework for building web APIs in python.

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. Keep the workspace _FQN_ handy. If you already have a workspace you can use that.

> **_NOTE:_** A workspace is a resource (CPU, Memory) bound environment where we deploy jobs, services.

### Writing our FastAPI service

Create `main.py` in a directory.

```
.
├── main.py
└── requirements.txt
```

**`main.py`**
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

**`requirements.txt`**
```
fastapi==0.81.0
uvicorn==0.18.3
```

### Deploying the FastAPI service

{% tabs %}
{% tab title="Deploying using python API" %}

Here we will use the `Service` class from servicefoundry library to deploy the streamlit app. Note that we need to set the host to `0.0.0.0` so that  it can accept connections from outside the container.

```
.
├── main.py
├── requirements.txt
└── deploy.py
```

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry import Build, PythonBuild, Service

logging.basicConfig(level=logging.INFO)
service = Service(
    name="fastapi",
    image=Build(
        build_spec=PythonBuild(
            command="uvicorn main:app --port 8000 --host 0.0.0.0",
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

```
.
├── main.py
├── requirements.txt
└── servicefoundry.yaml
```

**`servicefoundry.yaml`**
```yaml
name: fastapi
components:
  - name: fastapi
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: tfy-python-buildpack
        command: uvicorn main:app --port 8000 --host 0.0.0.0
    ports:
      - port: 8000
```
You can deploy the training job using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN
```
{% endtab %}
{% endtabs %}