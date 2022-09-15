# Customize your job with advanced options

### Adding environment variables and secrets

The [Environment Variables](../concepts/env-variables.md) and [Secrets](../concepts/secrets.md) concept pages cover how to create and use them. Here we just show the usage for quick reference

{% tabs %}
{% tab title="Python API" %}

```python
from servicefoundry import Job, Build, PythonBuild

job = Job(
    name="iris-train-job",
    image=Build(
        build_spec=PythonBuild(
            command="python train.py",
            requirements_path="requirements.txt",
        )
    ),
    env={
        "DB_USER": "postgres",
      	"DB_PASSWORD": "tfy-secret://<YOUR_SECRET_FQN>"
    }
)
```

{% endtab %}
{% tab title="YAML definition file" %} 

```yaml
name: iris-train-job
components:
- name: iris-train-job
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
    DB_USER: "postgres"
    DB_PASSWORD: "tfy-secret://<YOUR_SECRET_FQN>"
```

{% endtab %}
{% endtabs %}

### Configure retry limit

You can specify the maximum number of attempts to run a Job before it is marked as failed. By default this is set to `1`

{% tabs %}
{% tab title="Python API" %}

```python
from servicefoundry import Job, Build, PythonBuild

job = Job(
    name="iris-train-job",
    image=Build(
        build_spec=PythonBuild(
            command="python train.py",
            requirements_path="requirements.txt",
        )
    ),
    retries=3
)
```

{% endtab %}
{% tab title="YAML definition file" %} 

```yaml
name: iris-train-job
components:
- name: iris-train-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: python train.py
      requirements_path: requirements.txt
  retries: 3
```

{% endtab %}
{% endtabs %}

### Configure Job Time Limit

You can specify (in seconds) the maximum amount of time for a job to run, whether it has failed or not. This will take precedence over the `retries` Retry Limit. By default this is set to `1000` seconds.

For example, if you set `retries` to `6` and a `timeout` of `480` seconds, the job will terminate after 8 minutes regardless of how many times it attempted to run.

{% tabs %}
{% tab title="Python API" %}

```python
from servicefoundry import Job, Build, PythonBuild

job = Job(
    name="iris-train-job",
    image=Build(
        build_spec=PythonBuild(
            command="python train.py",
            requirements_path="requirements.txt",
        )
    ),
    retries=6,
    timeout=480,
)
```

{% endtab %}
{% tab title="YAML definition file" %} 

```yaml
name: iris-train-job
components:
- name: iris-train-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: python train.py
      requirements_path: requirements.txt
  retries: 6
  timeout: 480
```

{% endtab %}
{% endtabs %}

### Set resources limits

You can configure the CPU and Memory resources to be allocated to each job. To understand the resources configuration in more details, please read the [Resources](../concepts/resources.md) concepts page.

For e.g. here we request `0.2` CPU and `128` MiB memory with a hard limit of `0.5` CPU and `512` MiB memory.

{% tabs %}
{% tab title="Python API" %}

```python
from servicefoundry import Job, Build, PythonBuild, Resources

job = Job(
    name="iris-train-job",
    image=Build(
        build_spec=PythonBuild(
            command="python train.py",
            requirements_path="requirements.txt",
        )
    ),
    resource=Resources(
        cpu_request=0.2,
        cpu_limit=0.5,
        memory_request=128,
        memory_limit=512,
    ),
)
```

{% endtab %}
{% tab title="YAML definition file" %} 

```yaml
name: iris-train-job
components:
- name: iris-train-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: python train.py
      requirements_path: requirements.txt
  resources:
    cpu_request: 0.2
    cpu_limit: 0.5
    memory_request: 128
    memory_limit: 512
```

{% endtab %}
{% endtabs %}

### Set job history limit

You can set how much history of jobs to retain. We have options to decide how many of the successful and the failed jobs to keep in history. This helps keep track of the previous completed / failed jobs for debugging purposes. 

{% tabs %}
{% tab title="Python API" %}

```python
from servicefoundry import Job, Build, PythonBuild

job = Job(
    name="iris-train-job",
    image=Build(
        build_spec=PythonBuild(
            command="python train.py",
            requirements_path="requirements.txt",
        )
    ),
    successful_jobs_history_limit=20,
    failed_jobs_history_limit=20,
)
```

{% endtab %}
{% tab title="YAML definition file" %} 

```yaml
name: iris-train-job
components:
- name: iris-train-job
  type: job
  image:
    type: build
    build_source:
      type: local
    build_spec:
      type: tfy-python-buildpack
      command: python train.py
      requirements_path: requirements.txt
  successful_jobs_history_limit: 20,
  failed_jobs_history_limit: 20,
```

{% endtab %}
{% endtabs %}

### 
