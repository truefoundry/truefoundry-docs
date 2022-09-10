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

We will need to have a workspace to get started with deployment. This will either be provisioned by your Devops / Infra teams or you can create one. You can read about Cluster and Workspace [here](../concepts/workspace.md).



