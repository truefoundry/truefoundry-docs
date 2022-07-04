# Create a Service using Servicefoundry Templates

In this quickstart, we will deploy a FastAPI service to Servicefoundry by using init command to bootstrap the project. Before moving ahead, make sure you have installed the library and created a workspace. If not, see how [here](./install-and-workspace.md).

## Create a new project
Start a new project using one of the many templates that Servicefoundry provides. On your terminal, enter the following:

```
servicefoundry init --from-template
```

This command will ask you a bunch of questions to get you set up with the project.

First, you will be asked to choose from a list of templates supported by servicefoundry. For this example, we will choose `fastapi` - a simple webapp using the [FastAPI framework](https://fastapi.tiangolo.com).

```
? Choose a template (Use arrow keys)
 » fastapi - A basic template for using FastAPI framework
   streamlit - A basic template for using Streamlit framework
   tensorflow-serving - Easily deploy TensorFlow saved models using this template
```

Provide a name for your service (or stay with the default). Choose a Python version from the dropdown. Let’s go with `python:3.10`.

```
? Name your FastAPI service: my-fast-api-service
? Choose a Python version for your FastAPI service: python:3.10
```

After that, you will be prompted to choose a workspace. A workspace in Servicefoundry is simply a collection of services that have the same permissions on them.

You can choose the workspace which you had created during [setup](./install-and-workspace.md).

```
? Please provide a name for your workspace: <workspace-name>
   <workspace-name>
   Create a new workspace
```

If you to tweak more parameters press `y` or press `n`

```
? Would you like to change additional parameter? (y/N) No
```

## Project Content

Once you are done with the above steps, Servicefoundry will create the new project for you. To view your project files, change to the project directory. If you chose the default name for your service:

```
cd my-fast-api-service
```

Let’s look at each of the files in the directory:

`main.py` - this file will contain the FastAPI code. This is where the business logic of your service resides and you can edit it as required.

`requirements.txt` - the Python requirements file

`servicefoundry.yaml` - this is where the Servicefoundry config resides. If you want to change any of the values you entered during the init step, you can edit this file.

## Deploy the service
To deploy the service, simply run:

```
sfy deploy
```

This will deploy the service to Servicefoundry. From the logs, you can get the link to your deployed service. You can also find your service from the Servicefoundry dashboard.

Go to the link and try out the FastAPI endpoints to make sure it's working as expected! You can find the docs for your service at `<service-link>/docs`

## Add features to your service
Now you can proceed to further develop your FastAPI service. The template sets you up with a FastAPI project that can add and subtract two numbers. Let's add an endpoint to multiply in `main.py`:

```python
@app.get(path="/multiply")
def multiply(a: int, b: int):
    return a * b
```

You can deploy the service again by running `sfy deploy` and the `/multiply` endpoint will become available.
