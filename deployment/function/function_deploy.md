# Deploy functions as service using servicefoundry

In this guide, we will learn how to deploy functions a service using servicefoundry.

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Select a workspace on the [the workspace page](https://app.truefoundry.com/workspace). Copy the [workspace _FQN_](../concepts/workspace.md). We will use the workspace _FQN_ to deploy the service in that workspace.

### Writing the functions we will deploy

**File Structure:**
```
.
├── module.py
└── requirements.txt
```

**`module.py`**
```python
from typing import List

import numpy as np


def normal(loc: float, scale: float, size: List[int]):
    return np.random.normal(loc=loc, scale=scale, size=size).tolist()


def uniform(low: float, high: float, size: List[int]):
    return np.random.uniform(low=low, high=high, size=size).tolist()
```

**`requirements.txt`**
```
numpy==1.23.2
```

### Deploying the functions as service

* Here we will use the `FunctionService` class from servicefoundry library to define and deploy the service.
* We can use the `register_function` method to register the functions we want to deploy.
* In the example below, the deployed service will have two different HTTP POST APIs. One for `normal` and another for the `uniform` function.
* While deploying, we automatically install the requirements defined in the `requirements.txt` file.

**File Structure:**

```
.
├── module.py
├── requirements.txt
└── deploy.py
```

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry.function_service import FunctionService

from module import normal, uniform

logging.basicConfig(level=logging.INFO)

service = FunctionService(name="func-service")
service.register_function(normal)
service.register_function(uniform)

service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
# NOTE:- You can run the service locally using the code snippet below,
# service.run().join()
```

You can deploy using the below command, 
```shell
python deploy.py
```

> **NOTE:** The above command will only upload the contents of the **current** directory. Therefore, when you import the function/module for registration, ensure that the import statement is relative to the directory from where you are deploying.