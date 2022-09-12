# Resources

For all deployments, we need to specify the resource constraints for the application so that it can be deployed accordingly 
on the cluster. The key resources to be specified are:

### CPU

CPU represents compute processing and is specifies as a number. 1 CPU unit is equivalent to 1 physical CPU core, or 1 virtual core, depending on whether the node is a physical host or a virtual machine running inside a physical machine.
We can also specify fractional CPU like `0.2` or even `0.02`. 

For CPU, we need to specify a `cpu_request` and `cpu_limit` param. 

`cpu_request` helps specify the amount of CPU that will
always be reserved for the application. This also means that the minimum cost that I incur for this application is going 
to be the cost of `cpu_request` number of CPUs. If the application always uses CPU lower than the `cpu_request`, you can 
assume it to run in a healthy way. 

`cpu_limit` helps specify the upper limit on cpu usage of the application, beyond which the application will be throttled and not allowed any more CPU usage. This helps safeguard other applications running on the same node since one misbehaving
application cannot interfere and reduce the resources available to the other applications. 

`cpu_limit` has to be always greater than or equal to `cpu_request`.

**How do you set `cpu_request` and `cpu_limit` ?**

If your application is taking, let's say 0.5 CPU in steady state and during peak times goes to 0.8 CPU, then request should be 0.5 and limit can be 0.9 (just to be safe). In general, cpu_request should be somewhere around the steady state usage and limit can account for the peak usage.

### Memory

Memory is defined as an integer and the unit is Megabytes. So a value of 1 means 1 MB of memory and 1000 means 1GB of memory.
Memory also has two fields: `memory_request` and `memory_limit`. Memory request defines the minimum amount of memory needed to run the application. If you think that your app requires at least 256MB of memory to operate, this is the request value.

`memory_limit` defines the max amount of memory that the application can use. If the application tries to use more memory, then it will be killed and OOM (Out of memory) error will show up on the pods. 

If the memory usage of your application increases during peak or because of some other events, its advisable to keep the memory limit around the peak memory usage.

> Keeping memory limit below the usual memory usage will result in OOM killing of the pods. 

### Disk size (Coming soon)

### GPU (Coming soon)

### Setting resources for truefoundry applications

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
        memorty_limit=512,
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
      memorty_limit: 512
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
