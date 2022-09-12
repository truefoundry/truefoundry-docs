# Deploy a Gradio service

[Gradio](https://gradio.app/) is a python library using which we can quickly create web interface on top of our model inference functions. In this guide we will use Gradio to create a web interface and deploy the [T5 Small](https://huggingface.co/t5-small) model.

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. If you already have a workspace you can use that. Keep the [workspace _FQN_](../faq/get-workspace-fqn.md) handy. we will use it for the deployment.

### Writing our Gradio interface

**File Structure:**

```
.
├── main.py
└── requirements.txt
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

### Deploying the Gradio interface

{% tabs %}
{% tab title="Deploying using python API" %}


* Here we will use the `Service` class from servicefoundry library to define and deploy the service.

* We are also using the `PythonBuild` class to define that we need a python environement. Learn more about our [build process here](../concepts/build.md).

**File Structure:**

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

from servicefoundry import Build, PythonBuild, Service, Resources

logging.basicConfig(level=logging.INFO)
service = Service(
    name="gradio",
    image=Build(
        build_spec=PythonBuild(
            command="python main.py",
        ),
    ),
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

* Here, the `build_spec: type: tfy-python-buildpack` indicates that we need an python environment for this service. Learn more about our [build process here](../concepts/build.md).

**File Structure:**

```
.
├── main.py
├── requirements.txt
└── servicefoundry.yaml
```

**`servicefoundry.yaml`**
```yaml
name: gradio
components:
  - name: gradio
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: tfy-python-buildpack
        command: python main.py
    ports:
      - port: 8080
    resources:
      memory_limit: 1500
      memory_request: 1000
```
You can deploy the service using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN
```
{% endtab %}
{% endtabs %}

Note that, we are using a _Python Buildpack_ to automatically generate a _Dockerfile_. Learn more about our [build process here](../concepts/build.md).