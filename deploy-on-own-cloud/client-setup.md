# Using clients with the installed Truefoundry services

### Setup Mlfoundry client for Experiment tracking and ML metadata store

To use the `mlfoundry` client with your installation of truefoundry,

```
// This is the url where the truefoundry installation can be reached
base_url = "https://truefoundry.organization.com" 

import mlfoundry as mlf
mlf.login(base_url)
client = mlf.get_client(base_url)
```

If you wish to use an already generated api_key instead use :

```
client = mlf.get_client(base_url), api_key="...")
```

### Setup Servicefoundry Cli for ML Training and Model deployment

The servicefoundry cli can configured to point to the newly deployed truefoundry installation, with the following command.

```
// This is the url where the truefoundry installation can be reached
CONTROL_PLANE_URL=https://truefoundry.organization.com

sfy user server $CONTROL_PLANE_URL
```
