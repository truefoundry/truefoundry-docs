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

### Example:
Here is a minimal fastapi example with PyTorch that you can deploy using GPUs.

#### `servicefoundry.yaml`
```yaml
build:
  build_pack: sfy_build_pack_docker
service:
  name: gpu-test
  cpu:
    required: 0.05
  gpu:
    limit: 1
  memory:
    required: 128000000
  ports:
  - container_port: 8000
    protocol: TCP
  replicas: 1
  workspace: <your-workspace-fqn-here>
```
#### `Dockerfile`
```
FROM pytorch/pytorch:1.12.0-cuda11.3-cudnn8-runtime
WORKDIR /gpu_test
COPY ./requirements.txt /tmp/
RUN pip install -U pip && pip install -U -r /tmp/requirements.txt
COPY . /gpu_test
CMD uvicorn main:app --host 0.0.0.0 --port 8000
```
#### `requirements.txt`
```
servicefoundry>=0.1.67,<0.2.0
fastapi>=0.78.0,<1.0.0
prometheus_client>=0.14.1,<1.0.0
uvicorn>=0.17.0,<1.0.0
```
#### `main.py`
```python
import logging
import os

import torch
from fastapi.responses import HTMLResponse, JSONResponse
from servicefoundry.service import fastapi

app = fastapi.app()

@app.get(path="/test")
def test():
    gpu_available = torch.cuda.is_available()
    compute = None
    device = torch.device('cuda') if gpu_available else torch.device('cpu')
    a = torch.tensor([1, 2, 3], dtype=torch.float32, device=device)
    b = torch.tensor([2, 3, 4], dtype=torch.float32, device=device)
    c = a + b
    compute = c.cpu().numpy().tolist()
    return {"gpu_available": gpu_available, "compute": compute}
```
