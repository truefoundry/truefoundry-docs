# Configuring resource (CPU/Memory) requirements

You can customize resource (CPU/Memory) request and limit the requirements of your service/job. Requests are what an instance of service/job is guaranteed to get. Limits ensure that the instance cannot consume more than the specified amount of resources. Request fields should always be lower or the same as limit fields.

We follow [kubernetes spec](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes) for [CPU](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#meaning-of-cpu) and [Memory](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#meaning-of-memory) resource units.

{% tabs %}
{% tab title="Python API" %}

```python
import logging

from servicefoundry import Build, Service, DockerFileBuild, Resources

logging.basicConfig(level=logging.INFO)
service = Service(
    name="service",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8501}],
    resource=Resources( # You can use this argument in `Job` too.
        cpu_request="100m",
        cpu_limit="500m",
        memory_request="128Mi",
        memorty_limit="512Mi",
    ),
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

{% endtab %}
{% tab title="YAML definition file and CLI command" %} 

```yaml
name: service
components:
  - name: service
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: dockerfile
    ports:
     - port: 8501
    resources: # You can use this block in `job` too.
      cpu_request: 50m
      cpu_limit: 100m
      memory_request: 128Mi
      memorty_limit: 512Mi
```
{% endtab %}
{% endtabs %}

We set the following defaults if you do not configure any resources field.

| Field          | Default value |
|----------------|---------------|
| cpu_request    | 200m          |
| cpu_limit      | 200m          |
| memory_request | 256Mi         |
| memory_limit   | 256Mi         |