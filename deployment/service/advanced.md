# Advanced service configurations

## Configuring resource (CPU/Memory) requirements

You can customize resource (CPU/Memory) request and limit the requirements of your service/job. Requests are what an instance of service/job is guaranteed to get. Limits ensure that the instance cannot consume more than the specified amount of resources. Request fields should always be lower or the same as limit fields.

For CPU, we expect positive floating point values. `1` means one CPU core. You can request fractional CPU too, like `0.5`.
For memory, we expect positive integer values and we use **megabytes** as our unit. `256` means `256MB`.

{% tabs %}
{% tab title="Python API" %}

* `Service` and `Job`, both has a `resource` argument where you can either pass an instance of the `Resources` class or a `dict`.

```python
import logging

from servicefoundry import Build, Service, DockerFileBuild, Resources

logging.basicConfig(level=logging.INFO)
service = Service(
    name="service",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8501}],
    resource=Resources( # You can use this argument in `Job` too.
        cpu_request=0.2,
        cpu_limit=0.5,
        memory_request=128,
        memory_limit=512,
    ),
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

{% endtab %}
{% tab title="YAML definition file and CLI command" %} 

* You can defined the resource fields as a key-value pair under the `resources` field.

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
      cpu_request: 0.2
      cpu_limit: 0.5
      memory_request: 128
      memory_limit: 512
```
{% endtab %}
{% endtabs %}

We set the following defaults if you do not configure any resources field.

| Field          | Default value |
|----------------|---------------|
| cpu_request    | 0.2          |
| cpu_limit      | 0.5          |
| memory_request | 200          |
| memory_limit   | 500          |


## Health Checks

Health checks allow you to detect when the service is healthy or not. This helps to route the incoming traffic to only the healthy instances and restart or terminate the containers that are not healthy. We can currently configure two types of health checks - the liveness probe and readiness probe.

### Liveness Probe

Liveness probe checks whether the service is currently healthy by making a request to an endpoint of the service. If the service is not healthy, the container will be terminated and another one will be restarted. We can configure all parameters of the liveness probe according to our needs:

### Readiness Probe

Readiness probe checks whether the container is ready to receive traffic. Until the readiness probe succeeds, no incoming traffic will be routed to this container. Like the liveness probe, this is also achieved by making a request to an endpoint and checking if the endpoint responds with a successful response(any HTTP code >= 200 and < 400). This is usually useful when the service is doing some heavy work like loading a model which can take significant time - during this period, we don't want to route any traffic to this container since the model is not loaded yet.  

HealthChecks can be configured using the the following parameters:

- **HttpRequest Configuration** (Response is considered successful if http status code is >=200 and < 400)
  - port: Set the port to send the HTTP request to.
  - path: The endpoint path to send the request to
- **initial_delay_seconds**: Number of seconds after the container is started before the first probe is initiated. Defaults to 0.
- **period_seconds** - How often, in seconds, to execute the probe. Defaults to 10.
- **timeout_seconds** - Number of seconds after which the probe times out. (Defaults to 1)
- **success_threshold** - Minimum consecutive successes for the probe to be considered successful after having failed (Defaults to 1)
- **failure_threshold** - Number of consecutive failures required to determine the container is not alive for liveness probe or not ready for readiness probe (Defaults to 3)

### Example

**File Structure:**
```
.
└── main.py
```

**`main.py`**
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/livez")
def liveness():
    return True


@app.get("/readyz")
def readyness():
    return True


@app.get("/")
async def root():
    return {"message": "Hello World"}

```

{% tabs %}
{% tab title="Deploying using python API" %}

Here we will use the `HttpProbe` and `HealthProbe` classes from servicefoundry to define the health check configurations.

**File Structure:**

```
.
├── main.py
└── deploy.py
```

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry import (
    Build,
    PythonBuild,
    Service,
    HttpProbe,
    HealthProbe,
)

logging.basicConfig(level=logging.INFO)
service = Service(
    name="svc-health",
    image=Build(
        build_spec=PythonBuild(
            command="uvicorn main:app --port 8000 --host 0.0.0.0",
            pip_packages=["fastapi==0.81.0", "uvicorn==0.18.3"],
        ),
    ),
    ports=[{"port": 8000}],
    liveness_probe=HealthProbe(
        config=HttpProbe(path="/livez", port=8000),
        initial_delay_seconds=0,
        period_seconds=10,
        timeout_seconds=1,
        success_threshold=1,
        failure_threshold=3,
    ),
    readiness_probe=HealthProbe(
        config=HttpProbe(path="/readyz", port=8000),
        period_seconds=5,
    ),
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

**File Structure:**

```
.
├── main.py
└── servicefoundry.yaml
```

**`servicefoundry.yaml`**
```yaml
name: svc-health
components:
  - name: svc-health
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: tfy-python-buildpack
        command: uvicorn main:app --port 8000 --host 0.0.0.0
        pip_packages:
          - fastapi==0.81.0
          - uvicorn==0.18.3
    ports:
      - port: 8000
    liveness_probe:
      config:
        type: http
        path: /livez
        port: 8000
      initial_delay_seconds: 0
      period_seconds: 10
      timeout_seconds: 1
      success_threshold: 1
      failure_threshold: 3
    readiness_probe:
      config:
        type: http
        path: /readyz
        port: 8000
      period_seconds: 5
```

{% endtab %}
{% endtabs %}

## Environment Variables

You can inject environemt variables to a service by changing the deployment python script or the YAML file.

* [Injecting environment variables to a service](../concepts/env-variables.md)
* [Injecting secrets as environment variables to a service](../concepts/secrets.md)

## Autoscaling

Coming soon!
