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

Our build servers use the _build spec_ to build a container image from the source code defined using _build source_.

### Dockerfile build

Use _dockerfile build_ if you have already written a _Dockerfile_.

{% tabs %}
{% tab title="Python API" %}

We will use the `DockerFileBuild` class here.
```python
from servicefoundry import Service, Build, DockerFileBuild

service = Service(
    name="fastapi",
    image=Build(
        build_spec=DockerFileBuild(
            dockerfile_path="Dockerfile"
            build_context_path="./"
        ),
    ),
    ...
)
```

{% endtab %}

{% tab title="YAML definition file" %} 
The `type: dockerfile` identifies it as a _Dockerfile build_. 

```yaml
name: fastapi
components:
  - name: fastapi
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: dockerfile
        dockerfile_path: Dockerfile
        build_context_path: "./"
```

{% endtab %}
{% endtabs %}

#### Parameters
| Name | Type | Default value | Description |
|-|-|-|-|
dockerfile_path| string| "./Dockerfile" | The file path of the Dockerfile relative to project/build source root path.
build_context_path| string | "./" (project/build source root path) | Build context path for the Dockerfile relative to project/build source root path.

Our build server uses this spec to build the container image using a command equivalent to,
```shell
docker build -f dockerfile_path build_context_path
```


### Python build
If you do not have a _Dockerfile_ you can use the _Python build_ to build a container image with a specific python version and pip packages installed.

{% tabs %}
{% tab title="Python API" %}

We will use the `PythonBuild` class here.
```python
from servicefoundry import Service, Build, PythonBuild

service = Service(
    name="fastapi",
    image=Build(
        build_spec=PythonBuild(
            python_version="3.9",
            build_context_path="./",
            requirements_path="my-requirements.txt",
            pip_packages=["fastapi==0.82.0", "uvicorn"],
            command="uvicorn main:app --port 8000 --host 0.0.0.0"
        ),
    ),
    ...
)
```

{% endtab %}

{% tab title="YAML definition file" %} 
The `type: tfy-python-buildpack` identifies it as a _Python build_. 

```yaml
name: fastapi
components:
  - name: fastapi
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: tfy-python-buildpack
        python_version: 3.9
        build_context_path: "./"
        requirements_path: my-requirements.txt
        pip_packages:
          - fastapi==0.82.0
          - uvicorn
        command: uvicorn main:app --port 8000 --host 0.0.0.0
```

{% endtab %}
{% endtabs %}

#### Parameters
| Name | Type | Default value | Description |
|-|-|-|-|
python_version| string, regex: `^\d+(\.\d+){1,2}$` | "3.9" | Python version to run your code. We use this to choose the base image.
build_context_path| string | "./" (project/build source root path) | Build context path for the Dockerfile relative to project/build source root path.
requirements_path| string, nullable | `None` | Path to the `requirements.txt` file relative to project/build source root path. If there is file with the name `requirements.txt` under project root, we will automatically use that as the `requirements_path`.
pip_packages| List or array of strings | `None` | An array of pip package requirements.
command | string or array of strings | | Command to run the code. This is a required argument.

> **NOTE:** If you pass both `requirements_path` and `pip_packages` then we will install the union of the packages defined. IE: if you have `fastapi` defined in `requirements.txt` and pass `["numpy"]` in `pip_packages`, we will install both `numpy` and `fastapi`.