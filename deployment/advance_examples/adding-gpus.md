# Adding GPUs to your ServiceFoundry service

> :warning: This is an experimental feature.

> :information_source: Support for custom GPU instances (Tesla K80, Tesla V100, Tesla T4, Ampere A100 etc) and fractional GPU allotment are on our roadmap. Please [contact us](https://docs.truefoundry.com/documentation/getting-help) if you have such requirements.

{% tabs %}
{% tab title="Python API" %}

```python
import logging

from servicefoundry import Build, Service, DockerFileBuild, Resources

logging.basicConfig(level=logging.INFO)
service = Service(
    name="gpu-svc",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8501}],
    resource=Resources(gpu_limit=1)
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```
Only integer values are supported for now.

{% endtab %}
{% tab title="YAML definition file and CLI command" %} 

You can add GPUs to services by simply adding a `gpu_limit` in the `servicefoundry.yaml` file. 
```yaml
name: gpu-svc
components:
  - name: gpu-svc
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: dockerfile
    ports:
     - port: 8501
    resources:
      gpu_limit: 1 # <- This field.
```
Only integer values are supported for now.
{% endtab %}
{% endtabs %}


### Examples:

1. [Minimal FastAPI example with PyTorch using GPU](https://github.com/truefoundry/truefoundry-examples/tree/main/adding-gpu-to-service/service)
