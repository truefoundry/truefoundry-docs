# Train and deploy a model using servicefoundry.

In this guide, we will

1. We will create a training job and deploy it using servicefoundry jobs.
2. Save the model using _mlfoundry_, our experiment tracking library.
3. We will create a REST API service using _FastAPI_ and deploy it with servicefoundry service.

We will use the _Iris_ dataset and the _sklearn_ library to create the model for brevity. However, you can use any ML framework. We will also use _mlfoundry_, which provides a seamless way to track your ML experiments, to save the model; you can use any other experiment tracking/model registry library or blob storage.

> **_NOTE:_** A service should always be up and running. In comparison, a Job is intended to execute and successfully terminated.

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. Keep the workspace _FQN_ handy. If you already have a workspace you can use that.

> **_NOTE:_** A workspace is a resource (CPU, Memory) bound environment where we deploy jobs, services.

3. We need to create an API key and save it as a secret. This will help us securely inject the API key as an environment variable.
    1. Go to [the settings page](https://app.truefoundry.com/settings) and copy an API key.
    2. Go to [the secrets page](https://app.truefoundry.com/secrets) and click on _Add Secret Group_. Give a name (_Ex:- mlfoundry_) and click on _Save Changes_.
    3. Click on _New Secret_ under the newly created secret group and paste the API key in the value field. You can provide a _key_ name of your choice (_Ex:- api_key_) and save it.
    4. Each _Secret_ under _Secret Group_ will have a _Fully Qualified Name_ (FQN). Keep this handy for the secret we just created. We will use this to securely mount and access the API key in our training job and API service. 

## Creating a training job

### Writing model training code and requirements

Create the `train.py` and `train_requirements.txt` files in a directory.
```
.
├── train.py
└── train_requirements.txt
```
**`train.py`**
```python
import mlfoundry
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC

x, y = load_iris(as_frame=True, return_X_y=True)
x = x.rename(
    columns={
        "sepal length (cm)": "sepal_length",
        "sepal width (cm)": "sepal_width",
        "petal length (cm)": "petal_length",
        "petal width (cm)": "petal_width",
    }
)

# NOTE:- You can pass these configurations via command line
# arguments, config file, environment variables.
train_x, test_x, train_y, test_y = train_test_split(
    x, y, test_size=0.2, random_state=42, stratify=y
)

pipe = Pipeline([("scaler", StandardScaler()), ("svc", SVC())])
pipe.fit(train_x, train_y)

run = mlfoundry.get_client().create_run(project_name="iris-classification")
# NOTE:- We are saving the model using mlfoundry.
# You can use any other model registry or blob storage.
run.log_model(pipe, framework="sklearn")
run.log_metrics({"accuracy": pipe.score(test_x, test_y)})
```

**`train_requirements.txt`**
```
mlfoundry==0.3.34
scikit-learn==1.1.2
```

### Deploying the training job

In this section, we will deploy the model training code we defined in the above section. You can either deploy using the python APIs or you can deploy using a YAML file and the `servicefoundry deploy` command. After deployment, the job will run immediately.


{% tabs %}
{% tab title="Deploying using python API" %}

Here we are using the `Job` class to define the training job. We will use the _FQN_ of the secret containing the mlfoundry API Key and the workspace _FQN_ here. Replace `YOUR_SECRET_FQN`, `YOUR_WORKSPACE_FQN`  with the actual values.

Create `train_deploy.py` file in the same directory containing the `train.py` and `train_requirements.txt` files.

```
.
├── train.py
├── train_requirements.txt
└── train_deploy.py
```

**`train_deploy.py`**
```python
# Replace `YOUR_SECRET_FQN`, `YOUR_WORKSPACE_FQN`
# with the actual values.
import logging

from servicefoundry import (
    Build,
    Job,
    PythonBuild,
)

logging.basicConfig(level=logging.INFO)

# NOTE: Here we are defining the image build specification.
# servicefoundry uses this specification to automatically create
# a Dockerfile and build an image,
image = Build(
    build_spec=PythonBuild(
        command="python train.py",
        requirements_path="train_requirements.txt",
    )
)
env = {
    # NOTE:- We will automatically map the secret value to the environment variable.
    "MLF_API_KEY": "tfy-secret://YOUR_SECRET_FQN",
}


job = Job(
    name="training-job",
    image=image,
    env=env,
)
job.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

You can deploy the job using, 
```shell
python train_deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

```
.
├── train.py
├── train_requirements.txt
└── train_deploy.yaml
```

**`train_deploy.yaml`**
```yaml
# Replace `YOUR_SECRET_FQN`, `YOUR_RUN_FQN`
# with the actual values.
name: training-job
components:
- name: training-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: python train.py
      requirements_path: train_requirements.txt
  env:
    MLF_API_KEY: tfy-secret://YOUR_SECRET_FQN
```

You can deploy the training job using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN --file train_deploy.yaml
```
Run the above command from the same directory containing the `train.py` and `train_requirements.txt` files.

{% endtab %}
{% endtabs %}

## Creating a REST API service

In this section we will use the model saved in the training job. Keep the _run FQN_ of the training job handy. You can find the _run FQN_ from the training job logs.

### Writing inference service code and requirements

Create the `inference_api.py` and `inference_requirements.txt` files in a directory. 

```
.
├── inference_api.py
└── inference_requirements.txt
```

**`inference_api.py`**
```python
import os

import mlfoundry
import pandas as pd
from fastapi import FastAPI

RUN_FQN = os.getenv("MLF_RUN_FQN")
run = mlfoundry.get_client().get_run(RUN_FQN)
model = run.get_model()

app = FastAPI()


@app.get("/predict")
def predict(
    sepal_length: float, sepal_width: float, petal_length: float, petal_width: float
):
    data = dict(
        sepal_length=sepal_length,
        sepal_width=sepal_width,
        petal_length=petal_length,
        petal_width=petal_width,
    )
    return {"prediction": int(model.predict(pd.DataFrame([data]))[0])}
```

**`inference_requirements.txt`**
```
fastapi==0.81.0
uvicorn==0.18.3
mlfoundry==0.3.34
scikit-learn==1.1.2
```

### Deploying the inference API

 You can either deploy using the python APIs or you can deploy using a YAML file and the `servicefoundry deploy` command.

{% tabs %}
{% tab title="Deploying using python API" %}

Here we are using the `Service` class to define the service that we will deploy. Replace `YOUR_SECRET_FQN`, `YOUR_RUN_FQN` and `YOUR_WORKSPACE_FQN` with the actual values.

Create `inference_api_deploy.py` file in the same directory containing the `inference_api.py` and `inference_requirements.txt` files.

```
.
├── inference_api.py
├── inference_requirements.txt
└── inference_api_deploy.py
```

**`inference_api_deploy.py`**
```python
# Replace `YOUR_SECRET_FQN`, `YOUR_WORKSPACE_FQN`
# with the actual values.
import logging

from servicefoundry import Build, Service, PythonBuild

logging.basicConfig(level=logging.INFO)

image = Build(
    build_spec=PythonBuild(
        command="uvicorn inference_api:app --port 8000 --host 0.0.0.0",
        requirements_path="inference_requirements.txt",
    ),
)
env = {
    "MLF_API_KEY": "tfy-secret://YOUR_SECRET_FQN",
    "MLF_RUN_FQN": "YOUR_RUN_FQN",
}

service = Service(
    name="inference-svc",
    image=image,
    ports=[{"port": 8000}],
    env=env,
)
deployment = service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

You can deploy the API service using, 
```shell
python inference_api_deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

Replace `YOUR_SECRET_FQN`, `YOUR_RUN_FQN` and `YOUR_WORKSPACE_FQN` with the actual values.

```
.
├── inference_api.py
├── inference_requirements.txt
└── inference_api_deploy.yaml
```

**`inference_api_deploy.yaml`**
```yaml
# Replace `YOUR_SECRET_FQN`, `YOUR_RUN_FQN`
# with the actual values.
name: inference-svc
components:
- name: inference-svc
  type: service
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: uvicorn inference_api:app --port 8000 --host 0.0.0.0
      requirements_path: inference_requirements.txt
  ports:
    - port: 8000
  env:
    MLF_API_KEY: tfy-secret://YOUR_SECRET_FQN
    MLF_RUN_FQN: YOUR_RUN_FQN
```

You can deploy the inference API service using the command below,
```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN --file inference_api_deploy.yaml
```
Run the above command from the same directory containing the `inference_api.py` and `inference_requirements.txt` files.
{% endtab %}
{% endtabs %}