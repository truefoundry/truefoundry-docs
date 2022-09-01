# Deploy your exisiting Docker application with TrueFoundry

You can deploy any of your existing Docker applications with TrueFoundry. Before moving ahead, make sure you have installed the `servicefoundry` library and created a workspace. If not, see how [here](./install-and-workspace.md).

You can see all the project files of the example below on [Github](https://github.com/truefoundry/truefoundry-examples/tree/main/docker-service-deployment).


Consider a simple [Gradio](https://gradio.app/) application that's packaged using Docker:

### main.py

```python
import gradio as gr

def greet(name):
    return "Hello " + name.capitalize() + "!"

gr.Interface(fn=greet, inputs="text", outputs="text").launch(server_name='0.0.0.0', server_port=8080)

```

### requirements.txt

The `requirements.txt` for this file will look like:
```
gradio
```

### Dockerfile

The Dockerfile contains instructions to build the image:
```dockerfile
FROM python:3.7
COPY ./requirements.txt /tmp/
RUN pip install -r /tmp/requirements.txt
COPY . ./app
WORKDIR app
ENTRYPOINT python main.py
```

To deploy this Docker application using TrueFoundry, you will need to provide a TrueFoundry service definition in the form of `servicefoundry.yaml`

### servicefoundry.yaml

```yaml
name: gradio-app
components:
  - name: gradio-app
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: dockerfile
    resources:
      cpu_request: 50m
      cpu_limit: 100m
      memory_request: 128Mi
      memorty_limit: 512Mi
    ports:
      - port: 8080
```

After adding the above files the directory stucture should look like,
```
.
├── main.py
├── requirements.txt
├── Dockerfile
└── servicefoundry.yaml
```

Now simply run, `sfy deploy --workspace-fqn <your-workspace-fqn>` in this folder to deploy your service. Copy your workspace FQN from the [workspaces dashboard](https://app.truefoundry.com/workspace).
You should be able to track the deployment and find the deployed application at the [TrueFoundry dashboard](https://app.truefoundry.com/applications).