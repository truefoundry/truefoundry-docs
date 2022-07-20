# Adding GPUs to your ServiceFoundry service

> :warning: This is an experimental feature.

> :information_source: Support for custom GPU instances (Tesla K80, Tesla V100, Tesla T4, Ampere A100 etc) and fractional GPU allotment are on our roadmap. Please contact us if you have such requirements.

You can add GPUs to services by simply adding a limit in the `servicefoundry.yaml` file. 
```yaml
service:
  gpu:
    limit: 1
  ...
```
Only integer values are supported for now.

### Examples:

1. [Minimal FastAPI example with PyTorch using GPU](https://github.com/truefoundry/truefoundry-examples/tree/main/adding-gpu-to-service/service)
