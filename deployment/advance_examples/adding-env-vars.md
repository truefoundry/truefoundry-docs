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