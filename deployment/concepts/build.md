# Build

When you deploy a job or service, you need to provide either a container image or _build specification_ that servicefoundry uses to build an image. In this guide, we will go through the different concepts and components of the _build specification_.

A _build specification_ is composed of two components.
1. A _build source_. This defines the source code location.
2. A _build spec_. This defines how to build a container image out of the _build source_.

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

A _build source_ defines the source code location using which we will build a container image.

### Local build source

You will need to use _local build source_ if you want to deploy code present on your local machine (This can be your laptop, VM, CI environment, etc.). In this case, we copy the code present on your local directory to our build server.

{% tabs %}
{% tab title="Python API" %}

We will use the `LocalSource` class here.
```python
from servicefoundry import Service, Build, LocalSource

service = Service(
    name="fastapi",
    image=Build(
        build_source=LocalSource(
            project_root_path="./my_project_root_path"
        ), 
        build_spec=...,
    ),
    ...
)
```

If you do not pass the `build_source` argument, it defaults to `LocalSource(project_root_path="./")`
{% endtab %}

{% tab title="YAML definition file" %} 
The `type: local` identifies it as a _local build source_. 

```yaml
name: fastapi
components:
  - name: fastapi
    type: service
    image:
      type: build
      build_source:
        type: local
        project_root_path: "./my_project_root_path"
      build_spec:
        ...
```
If you do not pass anything as the value of `project_root_path`, it defaults to `./`.

{% endtab %}
{% endtabs %}

#### Parameters
| Name | Type | Default value | Description |
|-|-|-|-|
| project_root_path | string |"./" | The project root path. The contents of this directory will be uploaded to our build server to build the image.


### Github build source

## Build Spec

### Dockerfile build

### Python build