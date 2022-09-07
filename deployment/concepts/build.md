# Build

When you deploy a job or service, you need to provide either a container image or _build specification_ that servicefoundry uses to build an image. In this guide, we will go through the different concepts and components of the _build specification_.

A _build specification_ is composed of two components.
1. A build source. This defines the source code location.
2. A build spec. This defines how to build a container image out of the _build source_.

{% tabs %}
{% tab title="Python API" %}

We define the _build specification_ using the `Build` class. 

```python
from servicefoundry import Service, Build

service = Service(
    name="fastapi",
    image=Build(
        build_source=..., 
        build_spec=...,
    ),
    ...
)
```
{% endtab %}

{% tab title="YAML definition file" %} 
We define the _build specification_ under the `image` key. The `type: build` identifies it as a _build specification_.

```yaml
name: fastapi
components:
  - name: fastapi
    type: service
    image:
      type: build
      build_source:
        ...
      build_spec:
        ...
```
{% endtab %}
{% endtabs %}

## Build source

### Local build source

### Github build source

## Build Spec

### Dockerfile build

### Python build