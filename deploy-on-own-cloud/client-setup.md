# Using clients with the installed Truefoundry services

To configure clients to use the installed truefoundry setup, do ensure that you have
the host for the installation on hand. This is host configured in the installation section
of the truefoundry-frontend-app.

For the examples mentioned below, the installation is assumed to be at `https://truefoundry.organization.com`

## Setup Mlfoundry client for Experiment tracking and ML metadata store

To use the `mlfoundry` client with your installation of truefoundry,

```python
# This is the url where your truefoundry installation can be reached
base_url = "https://truefoundry.organization.com" 

import mlfoundry as mlf
mlf.login(base_url)
client = mlf.get_client(base_url)
```

If you wish to use an already generated api_key instead use :

```python
client = mlf.get_client(base_url, api_key="...")
```

## Setup Servicefoundry Clients for ML Training and Model deployment

### Setup CLI

The servicefoundry cli can configured to point to the newly deployed truefoundry installation, with the following command.

```bash
# This is the url where your truefoundry installation can be reached
CONTROL_PLANE_URL=https://truefoundry.organization.com

sfy use server $CONTROL_PLANE_URL
```

### Setup client use in code

Servicefoundry can also be used in your python code.

```python
import servicefoundry.core as sfy

# This is the url where your truefoundry installation can be reached
control_plane_url = "https://truefoundry.organization.com" 

sfy.use_server(control_plane_url)

# Interactive login
sfy.login()
```

If you want to use an api_key to login, instead use :

```python
sfy.login(api_key="...")
```
