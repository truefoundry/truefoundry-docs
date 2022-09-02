# Adding environment variables to your ServiceFoundry service

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
    env=[
        {"name": "NODE_ENV", "value": "prod"},
        {"name": "S3_BUCKET_NAME", "value": "my-s3-bucket"},
    ],
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
    - name: NODE_ENV
      value: prod
    - name: S3_BUCKET_NAME
      value: my-s3-bucket
```
{% endtab %}
{% endtabs %}

The variables `NODE_ENV` and `S3_BUCKET_NAME` should be available in your environment on deployment.