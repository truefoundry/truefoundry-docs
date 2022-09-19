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

After you deploy the service, you will get an output like below,

```console
deployment/function/simple-function-deployment on  main [?] via 🐍 v3.9.13 (venv) took 10s
❯ python deploy.py
INFO:servicefoundry:Function 'normal' from module 'module' will be deployed on path 'POST /normal'.
INFO:servicefoundry:Function 'uniform' from module 'module' will be deployed on path 'POST /uniform'.
INFO:servicefoundry:Deploying application 'func-service' to 'v1:local:my-ws-2'
INFO:servicefoundry:Uploading code for service 'func-service'
INFO:servicefoundry:Uploading contents of '/Users/debajyotichatterjee/work/truefoundry-examples/deployment/function/simple-function-deployment'
INFO:servicefoundry:.sfyignore not file found! We recommend you to create .sfyignore file and add file patterns to ignore
INFO:servicefoundry:Deployment started for application 'func-service'. Deployment FQN is 'v1:local:my-ws-2:func-service:v3'
INFO:servicefoundry:Service 'func-service' will be available at
'https://func-service-my-ws-2.tfy-ctl-euwe1-devtest.devtest.truefoundry.tech'
after successful deployment
INFO:servicefoundry:You can find the application on the dashboard:- 'https://app.devtest.truefoundry.tech/applications/cl84s4md500921qruhysegp5x?tab=deployments'
```

You can find the host of the deployed service in the following section in the above logs.
```console
INFO:servicefoundry:Service 'func-service' will be available at
'https://func-service-my-ws-2.tfy-ctl-euwe1-devtest.devtest.truefoundry.tech'
after successful deployment
```

> **NOTE:** Swagger API documentation will be available on the root path. Click on the host link to open the docs.

You can send requests to the deployed service by using the code snippet below. Pass the host using the `--host` command line argument.

**File Structure:**

```
.
├── module.py
├── requirements.txt
├── deploy.py
└── send_request.py
```
**`send_request.py`**
```python
import argparse
from urllib.parse import urljoin

import requests

parser = argparse.ArgumentParser()
parser.add_argument("--host", required=True, type=str)
args = parser.parse_args()

response = requests.post(
    urljoin(args.host, "/normal"), json={"loc": 0, "scale": 1, "size": [12, 1]}
)
print(response.json())

response = requests.post(
    urljoin(args.host, "/uniform"), json={"low": 0, "high": 1, "size": [12, 1]}
)
print(response.json())
```