# Quick Start

## Signup on TrueFoundry

1. Signup on [TrueFoundry](https://app.truefoundry.com/signup).


## Install ServiceFoundry Client Library

On your machine (preferably, within a virtual environment or conda environment) -
```
# Install via pip
pip install servicefoundry
```

## Login from your cli

```
sfy login
```

output
```commandline
Login Code: XXXXXX
Waiting for authentication. Go to the following url to complete the authentication:
https://app.truefoundry.com/authorize/device?userCode=XXXXXX
Login Successful!
Session file stored at /Users/user/.truefoundry
You are logged in as 'user' with email 'user@xxx.xxx'
You can now initiate a new service using sfy init
```

## Create a workspace

```
sfy create workspace <workspace-name>
```

output
```commandline
Waiting for the task to start...
[Start] Creating Workspace...
[Done] Workspace Created Successfully
[Start] Deploying Grafana to enable workspace level monitoring
[Done] Grafana Deployed Successfully at https://grafana-<workspace-name>.tfy-dub-euwe1-production.production.truefoundry.cloud

[Done] Space created successfully. You can track your services at https://app.truefoundry.com/servicefoundry

                                                           Workspace
                  ╷
  key             │ value
╶─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────╴
  createdAt       │ 02 Jun 2022 16:51:58 IST
  updatedAt       │ 02 Jun 2022 16:51:58 IST
  id              │ xxxxxxxxxxxxxxxxxxxxxxxxxx
  fqn             │ v1:tfy-dub-euwe1-production:<workspace-name>
  name            │ <workspace-name>
  clusterId       │ tfy-dub-euwe1-production
  createdBy       │ cloud
  status          │ CREATE_SPACE_REQUESTED
  workspaceTier   │ {'id': 'MEDIUM', 'resources': {'price': {'max': 56, 'min': 10}, 'limits': {'cpu': '2', 'memory':
                  │ '4000000000'}, 'services': 10}, 'displayName': 'MEDIUM'}
  grafanaEndpoint │ grafana-<workspace-name>.tfy-dub-euwe1-production.production.truefoundry.cloud
  metadata        │ {'grafanaEndpoint': 'grafana-<workspace-name>.tfy-dub-euwe1-production.production.truefoundry.cloud'}
```

## List workspace

You can go to https://app.truefoundry.com/workspace

You can also use below cli command to list your workspace
```commandline
sfy list workspace
```

output
```commandline
Using cluster 'tfy-dub-euwe1-production'
                                                 Workspaces
              ╷                                               ╷                        ╷
  name        │ fqn                                           │ status                 │ createdAt
╶─────────────┼───────────────────────────────────────────────┼────────────────────────┼──────────────────────────╴
  badal-test  │ v1:tfy-dub-euwe1-production:<workspace-name>  │ CREATE_SPACE_SUCCEEDED │ 02 Jun 2022 16:51:58 IST
```
