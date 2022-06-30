# Adding environment variables to your ServiceFoundry service

You can add environment variables to services by simply adding them in the `servicefoundry.yaml` file against `service.env`

Example:
### servicefoundry.yaml
```yaml
build:
  build_pack: sfy_build_pack_docker
service:
  name: my-service
  env:
    - name: NODE_ENV
      value: prod
    - name: S3_BUCKET_NAME
      value: my-s3-bucket
  workspace: v1:tfy-dub-euwe1-production:my-workspace
  replicas: 1
```

The variables `NODE_ENV` and `S3_BUCKET_NAME` should be available in your environment on deployment.

## Secret Variables
In case you want to add secrets, like private keys or passwords, to your service environment, you can do it using SecretsFoundry. First, you need to add your secret to SecretsFoundry.

1. Go to [SecretsFoundry dashboard](https://app.truefoundry.com/secrets)

2. Create a new Secret Group and add your Secret to the Secret Group

3. Copy the FQN of the Secret you just created

Now you can use the Secret in your service by setting the value of the environment variable to `tfy-secret://<FQN-of-your-secret>`. Say your Secret FQN is `my-user:my-secret-group:my-secret`, then set the value to `tfy-secret://my-user:my-secret-group:my-secret`

For example:
### servicefoundry.yaml
```yaml
build:
  build_pack: sfy_build_pack_docker
service:
  name: my-service
  env:
    - name: NODE_ENV
      value: prod
    - name: MY_SECRET
      value: tfy-secret://my-user:my-secret-group:my-secret
  workspace: v1:tfy-dub-euwe1-production:my-workspace
  replicas: 1
```

After this, you can deploy your application by running servicefoundry deploy and **the value of the secret** will be available in your service environment against the environment variable `MY_SECRET`
