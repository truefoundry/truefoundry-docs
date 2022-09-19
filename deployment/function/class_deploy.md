# Deploy instance of a class as service using servicefoundry

In this guide, we will learn how to deploy an instance of class as a service using servicefoundry.

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Select a workspace on the [the workspace page](https://app.truefoundry.com/workspace). Copy the [workspace _FQN_](../concepts/workspace.md). We will use the workspace _FQN_ to deploy the service in that workspace.

### Writing the class that we will deploy

**File Structure:**
```
.
├── inference.py
└── requirements.txt
```

**`inference.py`**
```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM


class Model:
    def __init__(self, model_fqn: str):
        self.tokenizer = AutoTokenizer.from_pretrained(model_fqn)
        self.model = AutoModelForSeq2SeqLM.from_pretrained(model_fqn)

    def infer(self, input_text: str) -> str:
        input_ids = self.tokenizer(input_text, return_tensors="pt").input_ids
        outputs = self.model.generate(input_ids)
        return self.tokenizer.decode(outputs[0], skip_special_tokens=True)

```

**`requirements.txt`**
```
transformers==4.17.0
torch==1.12.1
```

### Deployment

* Here we will use the `FunctionService` class from servicefoundry library to define and deploy the service.
* We can use the `register_class` method to deploy the instance of the class.
* All the public methods of the instance will be deployed as HTTP POST APIs.
    > **NOTE:** The instance properties, attributes, and methods starting with `_` (Ex:- `def _infer(self,...)`) will not be deployed.
* While deploying, we automatically install the requirements defined in the `requirements.txt` file.

**File Structure:**

```
.
├── inference.py
├── requirements.txt
└── deploy.py
```

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry.function_service import FunctionService
from servicefoundry import Resources

from inference import Model

logging.basicConfig(level=logging.INFO)

service = FunctionService(
    name="t5-small",
    resources=Resources(memory_request=1000, memory_limit=1500),
)

service.register_class(Model, init_kwargs={"model_fqn": "t5-small"}, name="t5-small")

service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
# NOTE:- You can run the service locally using the code snippet below,
# service.run().join()
```

You can deploy using the below command, 
```shell
python deploy.py
```

> **NOTE:** The above command will only upload the contents of the **current** directory. Therefore, when you import the class/module for registration, ensure that the import statement is relative to the directory from where you are deploying.


After you deploy the service, you will get an output like below,

```console
INFO:servicefoundry:Method 'infer' from `Model:t5-small` will be deployed on path 'POST /t5-small/infer'.
INFO:servicefoundry:Deploying application 't5-small' to 'v1:local:my-ws-2'
INFO:servicefoundry:Uploading code for service 't5-small'
INFO:servicefoundry:Uploading contents of '/Users/debajyotichatterjee/work/truefoundry-examples/deployment/function/hf_inference_function_deployment'
INFO:servicefoundry:.sfyignore not file found! We recommend you to create .sfyignore file and add file patterns to ignore
INFO:servicefoundry:Deployment started for application 't5-small'. Deployment FQN is 'v1:local:my-ws-2:t5-small:v5'
INFO:servicefoundry:Service 't5-small' will be available at
'https://t5-small-my-ws-2.tfy-ctl-euwe1-devtest.devtest.truefoundry.tech'
after successful deployment
INFO:servicefoundry:You can find the application on the dashboard:- 'https://app.devtest.truefoundry.tech/applications/cl84s4a9z008x1qrubflzflg8?tab=deployments'
```

You can find the host of the deployed service in the following section in the above logs.
```console
INFO:servicefoundry:Service 't5-small' will be available at
'https://t5-small-my-ws-2.tfy-ctl-euwe1-devtest.devtest.truefoundry.tech'
after successful deployment
```

You can send requests to the deployed service by using the code snippet below. Pass the host using the `--host` command line argument.

**File Structure:**

```
.
├── inference.py
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
    urljoin(args.host, "/t5-small/infer"),
    json={"input_text": "translate English to German: Hello world."},
)
print(response.json())
```