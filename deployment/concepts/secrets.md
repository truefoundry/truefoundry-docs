# Secrets

## What are secrets?

We should not store confidential information like API keys, secret keys for encryption, database passwords, etc., in plain text format in the application code or configuration files in a version control system.

Instead, use Truefoundry to securely store and control access to them. Truefoundry also helps you seamlessly mount these secrets as environment variables.

## How to store secrets in Truefoundry?
To store secrets in Truefoundry, follow the steps below:

1. Go to [SecretsFoundry dashboard](https://app.truefoundry.com/secrets).

2. Create a new Secret Group and add your Secret to the Secret Group.
    > **Note:** Suppose your backend service needs to load a database password and an API key for an external service. You can create a secret group for that backend service and add the database password and the API key as secrets under that secret group.

3. Copy the _FQN_ of the Secret you just created. We use the _FQN_ to inject secrets in applications.

## Injecting Secrets as Environment Variables in application

Now you can use the Secret in your service by setting the value of the environment variable to `tfy-secret://<FQN-of-your-secret>`. Say your Secret FQN is `user:my-secret-group:my-secret`, then set the value to `tfy-secret://user:my-secret-group:my-secret`

{% tabs %}
{% tab title="Python API" %}

```python
import logging

from servicefoundry import Build, Service, DockerFileBuild

logging.basicConfig(level=logging.INFO)
service = Service(
    name="my-service",
    image=Build(build_spec=DockerFileBuild()),
    ports=[{"port": 8501}],
    env={
      "NODE_ENV": "prod",
      # The value of tfy-secret://user:my-secret-group:my-secret
      # will be mapped to the value of MY_SECRET environment variable.
      "MY_SECRET": "tfy-secret://user:my-secret-group:my-secret",
    },
)
service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

{% endtab %}
{% tab title="YAML definition file and CLI command" %} 

You can inject secrets as environment variables to services by adding them in the `servicefoundry.yaml` file. 
```yaml
name: my-service
components:
  - name: my-service
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: dockerfile
    ports:
     - port: 8501
    env:
      NODE_ENV: prod
      MY_SECRET: tfy-secret://user:my-secret-group:my-secret
```
{% endtab %}
{% endtabs %}

After this, you can deploy your application by running servicefoundry deploy and **the value of the secret** will be available in your service environment against the environment variable `MY_SECRET`

