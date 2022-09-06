# Using Secrets as Environment Variables
In case you want to add secrets, like private keys or passwords, to your service environment, you can do it using SecretsFoundry. First, you need to add your secret to SecretsFoundry.

1. Go to [SecretsFoundry dashboard](https://app.truefoundry.com/secrets)

2. Create a new Secret Group and add your Secret to the Secret Group

3. Copy the FQN of the Secret you just created

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
