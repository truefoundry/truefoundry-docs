# Deploy a Job using Dockerfile

We can also customize our image for the job using `Dockerfile` as building instructions.

We will use the same setup and code that we defined in the [Deploy Job guide](./deploy.md) . This time we will add a `Dockerfile`, build it and reference it when deploying.

**You can find the complete code in this example [here](https://github.com/truefoundry/truefoundry-examples/tree/main/deployment/job/dockerfile)**

Make sure you have [Prerequisites](./deploy.md#prerequisites) and [Code](./deploy.md#code-and-dependencies) setup from the [Deploy Job guide](./deploy.md). We have replicated the code here for completeness.

```
.
├── train.py
└── requirements.txt
```

<details>
  <summary>requirements.txt</summary>

  ```
  pandas==1.4.4
  numpy==1.22.4
  scikit-learn==1.1.2
  mlfoundry>=0.4.2,<0.5
  servicefoundry>=0.1.97,<0.2.0
  ```

</details>

<details>
  <summary>train.py</summary>

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

  # Here we are using Truefoundry's Model Registry, you can push model to any storage 
  run = mlfoundry.get_client().create_run(project_name="iris-classification")
  model_version = run.log_model(
      name="iris-classifier",
      model=model,
      framework="sklearn",
      description="SVC model trained on initial data",
  )
  print(f"Logged model: {model_version.fqn}")
  ```
</details>


## Add a `Dockerfile` to our source directory

```
.
├── train.py
├── requirements.txt
└── Dockerfile
```

**`Dockerfile`**

```dockerfile
FROM --platform=linux/amd64 python:3.9.14-slim
WORKDIR /job
COPY requirements.txt /tmp/
RUN pip install -U pip && pip install --no-cache-dir -r /tmp/requirements.txt
COPY . /job/
CMD python /job/train.py
```


## Deploying the job using Dockerfile
We can either deploy using the python APIs or we can deploy using a YAML file and the `servicefoundry deploy` command.

{% tabs %}
{% tab title="Deploying using python API" %}

Create `deploy.py` file in the same directory containing the `train.py` and `requirements.txt` files. Replace `<YOUR_SECRET_FQN>`, `<YOUR_WORKSPACE_FQN>`  with the actual values.

```
.
├── train.py
├── requirements.txt
├── Dockerfile
└── deploy.py
```

---

**`deploy.py`**

```python
# Replace `<YOUR_SECRET_FQN>` with the actual value.
import logging
import argparse
from servicefoundry import Build, Job, DockerFileBuild

parser = argparse.ArgumentParser()
parser.add_argument("--workspace_fqn", type=str, required=True, help="fqn of the workspace to deploy to")
args = parser.parse_args()
logging.basicConfig(level=logging.INFO)

# This time around we use `DockerFileBuild` as the build spec. By default it looks for
# ./Dockerfile
job = Job(
    name="iris-train-job",
    image=Build(build_spec=DockerFileBuild()),
    env={"MLF_API_KEY": "tfy-secret://<YOUR_SECRET_FQN>"}
)
job.deploy(workspace_fqn=args.workspace_fqn)
```

---

We can now deploy the job by running `deploy.py` and providing the workspace FQN, 

```shell
python deploy.py --workspace_fqn <YOUR_WORKSPACE_FQN>
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

Create a `servicefoundry.yaml` file  in the same directory containing the `train.py` and `requirements.txt` files. Replace `<YOUR_SECRET_FQN>`, `<YOUR_WORKSPACE_FQN>`  with the actual values.

```
.
├── train.py
├── requirements.txt
├── Dockerfile
└── servicefoundry.yaml
```

---

**`servicefoundry.yaml`**

```yaml
# Replace `<YOUR_SECRET_FQN>`, with the actual values.
name: iris-train-job
components:
- name: iris-train-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: dockerfile
  env:
    MLF_API_KEY: "tfy-secret://<YOUR_SECRET_FQN>"
```

---

We can now deploy the training job using the command below

```shell
servicefoundry deploy --workspace-fqn <YOUR_WORKSPACE_FQN>
```
> :information_source: Run the above command from the same directory containing the `train.py` and `requirements.txt` files.

{% endtab %}
{% endtabs %}

On successful deployment, the Job will be created and run immediately.  

---

# Deploy a Job from pre-built image

We can also choose to deploy a job from some pre-built image store in external registry. For this guide we will use the public [`hub.docker.com`](https://hub.docker.com/) registry. However, if you wish [you can integrate your registry with Truefoundry](../../deploy-on-own-cloud/servicefoundry-bootstrap.md#setup-a-default-docker-image-registry).

**You can find the complete code in this example [here](https://github.com/truefoundry/truefoundry-examples/tree/main/deployment/job/docker_image)**

We'll start with the same code from the above example

```
.
├── train.py
├── requirements.txt
└── Dockerfile
```

## Build the image, tag it, push it

To push to [`hub.docker.com`](https://hub.docker.com/) sign up and replace your username below.

```shell
docker login  # This will prompt for credentials
docker build . -t <YOUR_DOCKERHUB_USERNAME>/tf-job-docker-image:latest
docker push <YOUR_DOCKERHUB_USERNAME>/tf-job-docker-image:latest
```

This will build the image locally and push it to [`hub.docker.com`](https://hub.docker.com/). Once done, `<YOUR_DOCKERHUB_USERNAME>/tf-job-docker-image:latest` will be the Image URI that we can reference.

## Deploy a Job from image

We can either deploy using the python APIs or we can deploy using a YAML file and the `servicefoundry deploy` command.

{% tabs %}
{% tab title="Deploying using python API" %}

Create `deploy.py` file in the same directory containing the `train.py` and `requirements.txt` files. Replace `<YOUR_DOCKERHUB_USERNAME>`, `<YOUR_SECRET_FQN>`, `<YOUR_WORKSPACE_FQN>`  with the actual values.

```
.
├── train.py
├── requirements.txt
├── Dockerfile
└── deploy.py
```

---

**`deploy.py`**

```python
# Replace `<YOUR_DOCKERHUB_USERNAME>`, `<YOUR_SECRET_FQN>` with the actual values.
import logging
import argparse
from servicefoundry import Build, Job, Image

parser = argparse.ArgumentParser()
parser.add_argument("--workspace_fqn", type=str, required=True, help="fqn of the workspace to deploy to")
args = parser.parse_args()
logging.basicConfig(level=logging.INFO)

# This time around we use `Image` directly and give it a image_uri
job = Job(
    name="iris-train-job",
    image=Image(
        type="image",
        image_uri="<YOUR_DOCKERHUB_USERNAME>/tf-job-docker-image:latest"
    ),
    env={"MLF_API_KEY": "tfy-secret://<YOUR_SECRET_FQN>"}
)
job.deploy(workspace_fqn=args.workspace_fqn)
```

---

We can now deploy the job by running `deploy.py` and providing the workspace FQN, 

```shell
python deploy.py --workspace_fqn <YOUR_WORKSPACE_FQN>
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

Create a `servicefoundry.yaml` file  in the same directory containing the `train.py` and `requirements.txt` files. Replace `<YOUR_DOCKERHUB_USERNAME>`, `<YOUR_SECRET_FQN>`, `<YOUR_WORKSPACE_FQN>`  with the actual values.

```
.
├── train.py
├── requirements.txt
├── Dockerfile
└── servicefoundry.yaml
```

---

**`servicefoundry.yaml`**

```yaml
# Replace `<YOUR_DOCKERHUB_USERNAME>`, `<YOUR_SECRET_FQN>` with the actual values.
name: iris-train-job
components:
- name: iris-train-job
  type: job
  image:
    type: image
    image_uri: "<YOUR_DOCKERHUB_USERNAME>/tf-job-docker-image:latest"
  env:
    MLF_API_KEY: "tfy-secret://<YOUR_SECRET_FQN>"
```

---

We can now deploy the training job using the command below

```shell
servicefoundry deploy --workspace-fqn <YOUR_WORKSPACE_FQN>
```

> :information_source: Run the above command from the same directory containing the `train.py` and `requirements.txt` files.

{% endtab %}
{% endtabs %}

On successful deployment, the Job will be created and run immediately.  