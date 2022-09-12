# Deploy a Job

You can deploy jobs on Truefoundry using our Python SDK, or via a YAML file or using our UI. 

In this guide, we will deploy a simple training job where we train a simple classifier for sklearn iris dataset and log it using Truefoundry's [Model Registry](TODO: link to model registry here)

**You can find the code in this example [here](TODO: link to github here)**

Before we start, we will need:

1. Our Deployments SDK - `servicefoundry`. You can follow [the instructions here](quickstart/install-and-workspace.md) to install and set it up.

1. A [Workspace](TODO: link to workspace concept page, TODO link according to gitbook url ../faq/get-workspace-fqn.md) FQN - Go to [the workspace page](https://app.truefoundry.com/workspace) and create a Workspace. Note down the workspace FQN. If you already have a Workspace you can use that.

We start with a `requirements.txt` with our dependencies and a `train.py` containing our training code:

```
.
├── train.py
└── requirements.txt
```

**`requirements.txt`**
```
pandas==1.4.4
numpy==1.22.4
scikit-learn==1.1.2

# for experiment tracking and model registry
mlfoundry>=0.4.2,<0.5

# for deploying our job deployments
servicefoundry>=0.1.91,<0.2.0
```

**`train.py`**
```python
import mlfoundry
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC
from sklearn.metrics import classification_report

X, y = load_iris(as_frame=True, return_X_y=True)
X = X.rename(columns={
        "sepal length (cm)": "sepal_length",
        "sepal width (cm)": "sepal_width",
        "petal length (cm)": "petal_length",
        "petal width (cm)": "petal_width",
})

# NOTE:- You can pass these configurations via command line
# arguments, config file, environment variables.
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
pipe = Pipeline([("scaler", StandardScaler()), ("svc", SVC())])
pipe.fit(X_train, y_train)
print(classification_report(y_true=y_test, y_pred=pipe.predict(X_test)))

# You can push model to any storage, here we are using Truefoundry's Model Registry
run = mlfoundry.get_client().create_run(project_name="iris-classification")
model_version = run.log_model(
    name="iris-classifier",
    model=model,
    framework="sklearn",
    description="SVC model trained on initial data",
)
print(f"Logged model: {model_version.fqn}")
```


Since we are pushing our model to [Truefoundry Model Registry](TODO link to Model Registry) we will need to add our Truefoundry API Key as a [Secret](TODO link to secrete as a concept). Once you have configured it ([detailed guide here](TODO link according to gitbook url link to guide to getting API key and adding it as a secret)) note down the Secret FQN. 


{% tabs %}
{% tab title="Deploying using python API" %}

Here we are using the `Job` class to define the training job. We will use the FQN of the Secret containing the Truefoundry API Key and the Workspace FQN here. Replace `<YOUR_SECRET_FQN>`, `<YOUR_WORKSPACE_FQN>`  with the actual values.

Create `train_deploy.py` file in the same directory containing the `train.py` and `requirements.txt` files.

```
.
├── train.py
├── requirements.txt
└── train_deploy.py
```

**`train_deploy.py`**
```python
# Replace `<YOUR_SECRET_FQN>` with the actual value.
import logging
import argparse
from servicefoundry import Build, Job, PythonBuild

parser = argparse.ArgumentParser()
parser.add_argument("--workspace_fqn", type=str, required=True, help="fqn of the workspace to deploy to")
args = parser.parse_args()
logging.basicConfig(level=logging.INFO)


# First we define how to build our code into a Docker image
image = Build(
    build_spec=PythonBuild(
        command="python train.py",
        requirements_path="requirements.txt",
        env={"MLF_API_KEY": "tfy-secret://<YOUR_SECRET_FQN>"}
    )
)
job = Job(name="iris-train-job", image=image)
job.deploy(workspace_fqn=args.workspace_fqn)
```

We can now deploy the job by running `train_deploy.py` and providing the workspace FQN, 
```shell
python train_deploy.py --workspace_fqn <YOUR_WORKSPACE_FQN>
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

We will use the FQN of the Secret containing the Truefoundry API Key and the Workspace FQN here. Replace `<YOUR_SECRET_FQN>`, `<YOUR_WORKSPACE_FQN>`  with the actual values.

Create a `train_deploy.yaml` file  in the same directory containing the `train.py` and `requirements.txt` files.
```
.
├── train.py
├── requirements.txt
└── train_deploy.yaml
```

**`train_deploy.yaml`**
```yaml
# Replace `<YOUR_SECRET_FQN>`, with the actual values.
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
      requirements_path: requirements.txt
  env:
  - name: MLF_API_KEY
    value: tfy-secret://<YOUR_SECRET_FQN>
```

We can now deploy the training job using the command below

```shell
servicefoundry deploy --workspace-fqn <YOUR_WORKSPACE_FQN> --file train_deploy.yaml
```
> :information_source: Run the above command from the same directory containing the `train.py` and `requirements.txt` files.

{% endtab %}
{% endtabs %}

On successful deployment, the Job will be created and run immediately. 
> :information_source: We can configure a job to not run immediately. See [here](TODO link) for instructions.

We can now visit our [Applications page](https://app.truefoundry.com/applications) to check Build status, Build Logs, Runs History and monitor progress of runs.

TODO: Add screenshots of Dashboard and Grafana

## See Also
- [Checking Runs History and Monitoring Progress of a Job Run](TODO link)
- [Running a Job periodically](TODO link)
- [Re-Running a Job manually](TODO link)
- [Configuring Advanced Options for a Job](TODO link)


