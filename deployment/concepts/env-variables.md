# Environment Variables

An environment variable is a value that affects the way the code runs and is dependent on the environment on which it is running. 
For e.g, let's say we have written fastapi code to do model inference. The fastapi code will need to download the model from somewhere. 

When we are testing locally, we want the model to be picked up from local path `./models`, however when the code is running in production, the model might be 
placed in `/mnt/models`. In this case we would like the code to remain the same and the model path to be provided as an environment variable. 

// TODO: Add sample fastapi code

The way to achieve this will be to arrange our code as follows:

// Write code with the .env file and the code separately.

## How to inject environment variables in Truefoundry

In this guide we will learn how can we inject environment variables in our deployments.

{% tabs %}
{% tab title="Python API" %}

```python
import logging

from servicefoundry import Build, Service, DockerFileBuild

logging.basicConfig(level=logging.INFO)
service = Service(
    name="my-service",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8501}],
    env={
      "NODE_ENV": "prod",
      "S3_BUCKET_NAME": "my-s3-bucket",
    },
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

{% endtab %}
{% tab title="YAML definition file and CLI command" %} 

You can add environment variables to services by adding them in the `servicefoundry.yaml` file. 
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
      NODE_ENV: prod
      S3_BUCKET_NAME: my-s3-bucket
```
{% endtab %}
{% endtabs %}

The variables `NODE_ENV` and `S3_BUCKET_NAME` should be available in your environment on deployment.