# Deploy Inference from Notebook



Servicefoundry comes with a few handy APIs that lets you easily convert your Python code in IPython notebooks (like [Jupyter](https://jupyter.org/), [Google Colaboratory](https://research.google.com/colaboratory/faq.html)) to fully fledged applications.

In this guide, we cover:

1. Generating an API service from Python code
2. Generating a interactive UI application from Python code
3. Deploying a [Gradio](https://gradio.app/) app


You can follow along this tutorial on Google Colaboratory:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/truefoundry/truefoundry-examples/blob/main/notebook-deployment/deployment.ipynb)


## Generating an API service

Inside your IPython notebook:
```python
import servicefoundry.core as sfy
```

Login to TrueFoundry on the notebook:
```python
sfy.login()
```

Write Python functions to a file:
```python
%%writefile quick_math.py

def sum(a: int, b: int, c: int, d: int):
    return a + b + c + d

def product(a: int, b: float):
    return a * b
```

The `servicefoundry` library can automatically gather requirements for your Python program:

```python
requirements = sfy.gather_requirements("quick_math.py")
print(requirements)
```

To generate an API service from the your Python program:
```python
auto_service = sfy.Service("quick_math.py", requirements, sfy.Parameters(
    name="auto-service",
    workspace="<your-workspace-fqn>"
))
```

Replace `<workspace-fqn>` with the FQN of one of your own workspace. You can fetch it from the [dashboard](https://app.truefoundry.com/workspace).

To deploy your service on TrueFoundry servers:
```python
auto_service.deploy()
```

## Generating a webapp
To generate an interactive webapp, instead of `sfy.Service`, use `sfy.Webapp`.
```python
auto_webapp = sfy.Webapp("quick_math.py", requirements, sfy.Parameters(
    name="auto-webapp",
    workspace="<your-workspace-fqn>"
))
```

To deploy your webapp on TrueFoundry servers:
```python
auto_webapp.deploy()
```

## Deploying a Gradio application

Write the Gradio interface object and assign it to a variable named `app` in your notebook. Save it to a file:

```python
%%writefile hello.py

import gradio as gr

def greet(name):
    return "Hello " + name + "!"

app = gr.Interface(fn=greet, inputs="text", outputs="text")
```

Then use `sfy.Gradio` to deploy it to TrueFoundry:

```python

requirements = sfy.gather_requirements("hello.py")
print(requirements)
```

```python
auto_gradio = sfy.Gradio("hello.py", requirements, sfy.Parameters(
    name="auto-gradio",
    workspace="<your-workspace-fqn>"
))
```

To deploy your webapp on TrueFoundry servers:
```python
auto_gradio.deploy()
```