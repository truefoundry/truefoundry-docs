# Deploy a streamlit app using servicefoundry

In this guide, we will deploy a [streamlit app](https://docs.streamlit.io/library/get-started/create-an-app) using servicefoundry.

Before we begin,
1. You need to have the `servicefoundry`
library installed and login using the `servicefoundry login` command. If you do not have the library installed follow [the instructions here](quickstart/install-and-workspace.md).

2. Go to [the workspace page](https://app.truefoundry.com/workspace) and create a workspace. Keep the workspace _FQN_ handy. If you already have a workspace you can use that.

> **_NOTE:_** A workspace is a resource (CPU, Memory) bound environment where we deploy jobs, services.

### Writing our streamlit app

**File Structure:**

```
.
├── main.py
└── requirements.txt
```

**`main.py`**
```python
# https://docs.streamlit.io/library/get-started/create-an-app
import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = (
    "https://s3-us-west-2.amazonaws.com/"
    "streamlit-demo-data/uber-raw-data-sep14.csv.gz"
)


@st.cache
def load_data(nrows):
    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data


data_load_state = st.text("Loading data...")
data = load_data(10000)
data_load_state.text("Done! (using st.cache)")

if st.checkbox("Show raw data"):
    st.subheader("Raw data")
    st.write(data)

st.subheader("Number of pickups by hour")
hist_values = np.histogram(data[DATE_COLUMN].dt.hour, bins=24, range=(0, 24))[0]
st.bar_chart(hist_values)

# Some number in the range 0-23
hour_to_filter = st.slider("hour", 0, 23, 17)
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]

st.subheader("Map of all pickups at %s:00" % hour_to_filter)
st.map(filtered_data)
```

**`requirements.txt`**
```
streamlit==1.12.0
pandas==1.4.3
```

> **_NOTE:_** In the above code, we are **downloading** the data and caching it when the app loads for the first time. You can choose to keep the data file `uber-raw-data-sep14.csv.gz` in the same directory and read from there.

### Deploying the streamlit app

We will deploy the streamlit app we wrote in the above section. You can either deploy using the python APIs or you can deploy using a YAML file and the `servicefoundry deploy` command.

{% tabs %}
{% tab title="Deploying using python API" %}

* Here we will use the `Service` class from servicefoundry library to define and deploy the service.

* We need to expose the port `8501` as that is the default port used by streamlit.

* We are also using the `PythonBuild` class to define that we need a python environement. Learn more about our [build process here](../concepts/build.md).

**File Structure:**

```
.
├── main.py
├── requirements.txt
└── deploy.py
```

**`deploy.py`**
```python
# Replace `YOUR_WORKSPACE_FQN`
# with the actual value.
import logging

from servicefoundry import Build, PythonBuild, Service

logging.basicConfig(level=logging.INFO)
service = Service(
    name="streamlit",
    image=Build(
        build_spec=PythonBuild(
            command="streamlit run main.py",
        ),
    ),
    ports=[{"port": 8501}],
)
deployment = service.deploy(workspace_fqn="YOUR_WORKSPACE_FQN")
```

You can deploy the app using, 
```shell
python deploy.py
```

{% endtab %}
{% tab title="Deploying using YAML definition file and CLI command" %} 

* The `type: service` indicates that we are defining a service component.

* Here, the `build_spec: type: tfy-python-buildpack` indicates that we need an python environment for this service. Learn more about our [build process here](../concepts/build.md).

* We need to expose the port `8501` as that is the default port used by streamlit.

**File Structure:**

```
.
├── main.py
├── requirements.txt
└── servicefoundry.yaml
```

**`servicefoundry.yaml`**
```yaml
name: streamlit
components:
  - name: streamlit
    type: service
    image:
      type: build
      build_source:
        type: local
      build_spec:
        type: tfy-python-buildpack
        command: streamlit run main.py
    ports:
      - port: 8501
```
You can deploy the training job using the command below,

```shell
servicefoundry deploy --workspace-fqn YOUR_WORKSPACE_FQN --wait
```
{% endtab %}
{% endtabs %}