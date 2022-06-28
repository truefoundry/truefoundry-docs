# create_service

`create_service` create a fast API REST service of your predictor. `write` should be called to create new files.

### Arguments
| Parameter  | Type             | Default | Description                                                                         |
|------------|------------------|---------|-------------------------------------------------------------------------------------|
| predictor  | str or Predictor | NA      | Either pass the file location of file with predict function or create a `predictor` |
| parameters | dict             | {}      | Pass a dict containing paramaeters `service_name`, `workspace` or `python_version`. |

### Returns

### Example:
```python
sfy.create_service(predictor, {
    "service_name": "inference-fast-api",
    "workspace": "v1:tfy-dub-euwe1-devtest:badal-test2",
    "python_version": "python:3.9"
}).write()
```
