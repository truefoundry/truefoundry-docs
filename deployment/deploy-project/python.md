# Deploy your exisiting Python project with TrueFoundry

You can deploy any of your existing Python projects with TrueFoundry. Before moving ahead, make sure you have installed the `servicefoundry` library and created a workspace. If not, see how [here](./install-and-workspace.md).

You can see all the project files of the example below on [Github](https://github.com/truefoundry/truefoundry-examples/tree/main/python-project-deployment).

Consider a simple Python Flask application:

### main.py

```python
from flask import Flask
 
app = Flask(__name__)
 
@app.route('/')
def hello_world():
    return 'Hello World'
 
if __name__ == '__main__':
    app.run(host='0.0.0.0', port='8080')

```

### requirements.txt

Create the `requirements.txt` for this application:
```
flask
```

To deploy this Python application using TrueFoundry, you will need to provide a TrueFoundry service definition in the form of `servicefoundry.yaml` and add `Procfile` to [declare what command to run](https://stackoverflow.com/questions/16128395/what-is-procfile-and-web-and-worker).

### servicefoundry.yaml
We set the `build_pack` to build `sfy_build_pack_python` since it's a Python project. Copy your workspace FQN from the [dashboard](https://app.truefoundry.com/workspace) and paste it in the yaml.

```yaml
  build_pack: sfy_build_pack_python
  options:
    python_version: python:3.9.0
service:
  name: flask-app
  cpu:
    required: 0.05
    limit: 0.1
  memory:
    required: 128000000
    limit: 512000000
  workspace: <your-workspace-fqn>
  ports:
  - container_port: 8080
    protocol: TCP
```

### Procfile

Simply provide the command to start the web server inside `Procfile`:
```
web: python main.py
```

Now simply run `servicefoundry deploy` to deploy your Flask application on TrueFoundry. You should be able to track the deployment and find the deployed application at the [TrueFoundry dashboard](https://app.truefoundry.com/workspace).