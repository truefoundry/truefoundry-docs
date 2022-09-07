# Deploy your exisiting Dockerized service with TrueFoundry

You can deploy any of your existing Dockerized service with TrueFoundry. 

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. Keep the workspace _FQN_ handy. If you already have a workspace you can use that.

> **_NOTE:_** A workspace is a resource (CPU, Memory) bound environment where we deploy jobs, services.

You can see all the project files of the example below on [Github](https://github.com/truefoundry/truefoundry-examples/tree/main/docker-service-deployment).


Consider a simple [Gradio](https://gradio.app/) application that's packaged using Docker:

```
.
├── main.py
├── requirements.txt
└── Dockerfile
```

**`main.py`**
```python
import gradio as gr

def greet(name):
    return "Hello " + name.capitalize() + "!"

gr.Interface(fn=greet, inputs="text", outputs="text").launch(server_name='0.0.0.0', server_port=8080)
```

**`requirements.txt`**
```
gradio
```

**`Dockerfile`**

The Dockerfile contains instructions to build the image:
```dockerfile
FROM python:3.7
COPY ./requirements.txt /tmp/
RUN pip install -r /tmp/requirements.txt
COPY . ./app
WORKDIR app
ENTRYPOINT python main.py
```
You can deploy this dockerized application either using the python APIs or you can deploy using a YAML file and the `servicefoundry deploy` command.


{% tabs %}
{% tab title="Deploying using python API" %}

Here we will use the `DockerFileBuild` class from servicefoundry to indicate that this is a dockerized application.

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

from servicefoundry import Build, Service, DockerFileBuild

logging.basicConfig(level=logging.INFO)

service = Service(
    name="docker-svc",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8080}],
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```
You can deploy the app using, 
```shell
python deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

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

```

Now simply run, `sfy deploy --workspace-fqn <your-workspace-fqn>` in this folder to deploy your service. Copy your workspace FQN from the [workspaces dashboard](https://app.truefoundry.com/workspace).

{% endtab %}
{% endtabs %}


You should be able to track the deployment and find the deployed application at the [TrueFoundry dashboard](https://app.truefoundry.com/applications).