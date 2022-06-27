# Deploy Inference Fom Notebook

To use deployment API, import `servicefoundry.core`.

```python
import servicefoundry.core as sfy
```

To login.
```python
sfy.login()
```

To load your predict function and create a predictor.
```python
%%writefile predict.py

def predict(a: int, b: int, c: int, d: int):
    return a + b + c + d

def predict1(a: int, b: float):
    return a + b

def predict2(a: str):
    return f"Hello {a}"
```

To automatically gather requirements
```python
requirements = sfy.gather_requirements("predict.py")
print(requirements)
```

To create a service from predictor and write the generated
files in your current directory.
```python
auto_service = sfy.Service("predict.py", requirements, sfy.Parameters(
    name="auto-service",
    workspace="workspace-fqn(copy from website)"
))
```

To deploy your service on servicefoundry server.
```python
auto_service.deploy()
```

To create a webapp from predictor and write the generated
files in your current directory.
```python
auto_webapp = sfy.Webapp("predict.py", requirements, sfy.Parameters(
    name="auto-webapp",
    workspace="workspace-fqn(copy from website)"
))
```

To deploy your service on servicefoundry server.
```python
auto_webapp.deploy()
```
