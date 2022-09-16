# Advanced configurations

## Configuring resource (CPU/Memory) requirements

You can customize resource (CPU/Memory) request and limit the requirements using the `resources` argument in `FunctionService` class constructor.

```python
from servicefoundry.function_service import FunctionService
from servicefoundry import Resources

service = FunctionService(
    name="t5-small",
    resources=Resources(
        cpu_request=0.2,
        cpu_limit=0.5,
        memory_request=128,
        memory_limit=512,
    ),
)
...
```

[Read more about resources here.](../concepts/resources.md)

## Locally running the defined service

You can run the defined service locally using the `run` method defined in `FunctionService` class.

Using [the example here](./function_deploy.md)

```python
import logging

from servicefoundry.function_service import FunctionService

from module import normal, uniform

logging.basicConfig(level=logging.INFO)

service = FunctionService(name="func-service")
service.register_function(normal)
service.register_function(uniform)

service.run().join()
```

Note that,
* `run()` returns an instance of [`threading.Thread`](https://docs.python.org/3.9/library/threading.html#threading.Thread). We call the `join()` method so that we wait for the daemon thread's lifetime. To stop the webserver, press `ctrl-c`.

## Environment Variables

You can inject environment variables using the `env` argument in `FunctionService` class constructor.

You can access the the value of the environmen variables using [`os.getenv`](https://docs.python.org/3.9/library/os.html#os.getenv) in the deployed function or class.

```python
from servicefoundry.function_service import FunctionService
from servicefoundry import Resources

service = FunctionService(
    name="t5-small",
    env={"MODEL_FQN": "t5-small"}
)
...
```