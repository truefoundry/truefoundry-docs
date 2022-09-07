# Advanced service configurations

## Configuring resource (CPU/Memory) requirements

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


## Configuring GPU requirements

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

## Autoscaling

## Environment Variables
