# Adding environment variables to your ServiceFoundry service

You can add environment variables to services by simply adding them in the `servicefoundry.yaml` file.

Example:
### servicefoundry.yaml
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
    env:
    - name: NODE_ENV
      value: prod
    - name: S3_BUCKET_NAME
      value: my-s3-bucket
```

The variables `NODE_ENV` and `S3_BUCKET_NAME` should be available in your environment on deployment.