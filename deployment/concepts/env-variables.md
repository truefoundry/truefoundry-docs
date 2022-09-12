# Environment Variables

An environment variable is a value that affects the way the code runs and is dependent on the environment on which it is running.

For example, you have written a ML model API service that,

1. Downloads the model from somewhere.
2. Loads the model from disk.
2. Serves a `/infer` route, which calls the model's inference function.

Now, your service code may not change if you re-train and update the model. In this case, we can pass the model path via an environment variable.

**`main.py`**
```python
import os

from fastapi import FastAPI
import mlfoundry as mlf
client = mlf.get_client()

MODEL_FQN = os.getenv("MODEL_FQN")
model = client.get_model(MODEL_FQN).load()

app = FastAPI()


@app.get("/infer")
def infer(...):
    ...
```

You can then run this service and inject the environment variable value like below,

```shell
MODEL_FQN="YOUR MODEL FQN" uvicorn main:app --port 8000 --host 0.0.0.0
```

You can also use a `.env` file on your local dev environment and use [python-dotenv](https://pypi.org/project/python-dotenv/).
**`.env`**
```
MODEL_FQN="YOUR MODEL FQN FOR LOCAL RUN"
```


## How to inject environment variables in Truefoundry

In this guide we will learn how can we inject environment variables in our deployments in Truefoundry.

{% tabs %}
{% tab title="Python API" %}

* Both `Service ` and `Job` classes have an argument `env` where you can pass a dictionary. The dictionary keys will be assumed as environment variable names and the values will be the environment variable values.

```python
import logging

from servicefoundry import Build, Service, DockerFileBuild

logging.basicConfig(level=logging.INFO)
service = Service(
    name="my-service",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8501}],
    env={
      "MODEL_FQN": "YOUR MODEL FQN",
      "S3_BUCKET_NAME": "my-s3-bucket"
    },
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

{% endtab %}
{% tab title="YAML definition file and CLI command" %} 

* You can add your environment variables under `env:` section as key value pairs.

```yaml
name: my-service
components:
  - name: my-service
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: dockerfile
    ports:
     - port: 8501
    env:
      MODEL_FQN: "YOUR MODEL FQN"
      S3_BUCKET_NAME: my-s3-bucket
```
{% endtab %}
{% endtabs %}

The variables `MODEL_FQN` and `S3_BUCKET_NAME` should be available in your environment on deployment.