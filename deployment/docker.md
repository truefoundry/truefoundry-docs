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
build:
  build_pack: sfy_build_pack_docker
service:
  name: gradio-app
  cpu:
    required: 0.05
    limit: 0.1
  memory:
    required: 128000000
    limit: 512000000
  workspace: <paste-your-workspace-fqn>
  ports:
  - container_port: 8080
    protocol: TCP

```

We set the `build_pack` to build `sfy_build_pack_docker` since it's a Docker application. Copy your workspace FQN from the [dashboard](https://app.truefoundry.com/workspace) and paste it in the file.

Now simply run `servicefoundry deploy` to deploy your Docker application on TrueFoundry. You should be able to track the deployment and find the deployed application at the [TrueFoundry dashboard](https://app.truefoundry.com/workspace).