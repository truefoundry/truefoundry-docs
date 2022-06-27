# Adding environment variables to your ServiceFoundry service

You can add environment variables to services by simply adding them in the `servicefoundry.yaml` file. 

Example:
### servicefoundry.yaml
```yaml
template: truefoundry.com/v1/streamlit
parameters:
  service_name: my-streamlit-service
  workspace: v1:tfy-cluster:my-workspace


overwrites:
  Component.spec.container.env:
    - name: myEnvKey
      value: myEnvValue
```

## Secret Variables
In case you want to add secrets, like private keys or passwords, to your service environment, you can do it using SecretsFoundry. First, you need to add your secret to SecretsFoundry.

1. Go to [SecretsFoundry dashboard](https://app.truefoundry.com/secrets)

2. Create a new Secret Group and add your Secret to the Secret Group

3. Copy the FQN of the Secret you just created

Now you can use the Secret in your service by setting the value of the environment variable to `tfy-secret://<FQN-of-your-secret>`. Say your Secret FQN is `my-user:my-secret-group:my-secret`, then set the value to `tfy-secret://my-user:my-secret-group:my-secret`

For example:
### servicefoundry.yaml
```yaml
template: truefoundry.com/v1/streamlit
parameters:
  service_name: my-streamlit-service
  workspace: v1:tfy-cluster:my-workspace


overwrites:
  Component.spec.container.env:
    - name: mySecretEnvKey
      value: tfy-secret://my-user:my-secret-group:my-secret
```

After this, you can deploy your application by running servicefoundry deploy and the secret will be available in your service environment against the environment variable `mySecretEnvKey`
