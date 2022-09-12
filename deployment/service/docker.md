# Deploy your exisiting Docker service with TrueFoundry


You can deploy any of your existing Dockerized service with TrueFoundry. 

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. Keep the [workspace _FQN_](../concepts/workspace.md) handy. If you already have a workspace you can use that.

> **_NOTE:_** A workspace is a resource (CPU, Memory) bound environment where we deploy jobs, services.

Consider a [Gradio](https://gradio.app/) application that's packaged using Docker:

**File Structure:**

```
.
├── main.py
├── requirements.txt
└── Dockerfile
```

**`main.py`**
```python
import gradio as gr
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

tokenizer = AutoTokenizer.from_pretrained("t5-small")
model = AutoModelForSeq2SeqLM.from_pretrained("t5-small")


def t5_model_inference(input_text: str) -> str:
    input_ids = tokenizer(input_text, return_tensors="pt").input_ids
    outputs = model.generate(input_ids)
    return tokenizer.decode(outputs[0], skip_special_tokens=True)


gr.Interface(
    fn=t5_model_inference,
    inputs="text",
    outputs="textbox",
    allow_flagging="never",
).launch(server_name="0.0.0.0", server_port=8080)
# NOTE: we need to set `server_name` to `0.0.0.0` so that this process
# can accept requests coming from outside the container.
```

**`requirements.txt`**
```
gradio==3.2
transformers==4.17.0
torch==1.12.1
```

**`Dockerfile`**

The Dockerfile contains instructions to build the image.
```dockerfile
FROM python:3.9
COPY ./requirements.txt /tmp/
RUN pip install -U pip && pip install -r /tmp/requirements.txt
COPY . ./app
WORKDIR app
ENTRYPOINT python main.py
```

### Deploying the Gradio interface

You can deploy this dockerized application either using the python APIs or you can deploy using a YAML file and the `servicefoundry deploy` command.


{% tabs %}
{% tab title="Deploying using python API" %}

* Here we will use the `Service` class from servicefoundry library to define and deploy the service.

* Here we will use the `DockerFileBuild` class from servicefoundry to indicate that this is a dockerized application. Learn more about our [build process here](../concepts/build.md).

* We need to expose the port `8080`.

**File Structure:**

```
.
├── main.py
├── requirements.txt
├── Dockerfile
└── deploy.py
```

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry import Build, Service, DockerFileBuild, Resources

logging.basicConfig(level=logging.INFO)

service = Service(
    name="docker-svc",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8080}],
    resources=Resources(memory_limit=1500, memory_request=1000),
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```
You can deploy the app using, 
```shell
python deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

* The `type: service` indicates that we are defining a service component.

* Here, the `build_spec: type: dockerfile` indicates that we have already written a `Dockerfile` for this service. Learn more about our [build process here](../concepts/build.md).

* We need to expose the port `8080`.

**File Structure:**

```
.
├── main.py
├── requirements.txt
├── Dockerfile
└── servicefoundry.yaml
```

**`servicefoundry.yaml`**
```yaml
name: docker-svc
components:
  - name: docker-svc
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: dockerfile
    ports:
      - port: 8080
    resources:
      memory_limit: 1500
      memory_request: 1000
```

You can deploy the service using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN --wait
```

{% endtab %}
{% endtabs %}


You should be able to track the deployment and find the deployed application at the [TrueFoundry dashboard](https://app.truefoundry.com/applications).