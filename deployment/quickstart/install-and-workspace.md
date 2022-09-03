# Quick Start

## Signup on TrueFoundry

Sign up on [TrueFoundry](https://app.truefoundry.com/signup) using email, Google account or Github account.

## Install ServiceFoundry client library

On your machine, run (preferably within a virtual environment or conda environment):
```
pip install servicefoundry
```

## Login from CLI

From the CLI, run the following and follow the link to authenticate:

```
servicefoundry login
```

Output:
```commandline
Login Code: XXXXXX

Waiting for authentication. Go to the following url to complete the authentication:
https://app.truefoundry.com/authorize/device?userCode=XXXXXX

Login Successful!

Session file stored at /Users/user/.truefoundry
You are logged in as 'user' with email 'user@xxx.xxx'
```

## Create a workspace

Workspaces are groups of applications that can be somewhat grouped together - either they are handled by the same team, 
or belong to an environment like dev, staging, production. Each workspace can have multiple applications. 

Workspace also serves as the minimum unit of permission and access control which means that we can add users to a workspace and they will
have access to everything inside the workspace. 

> In the Kubernetes world, workspace maps to one namespace in the Kubernetes cluster. 

In many cases, the devops / infra team might already assign a workspace to you. In that case just copy the FQN column value from the Workspaces page. 



