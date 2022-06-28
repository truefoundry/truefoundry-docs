# Concepts

<mark style="color:blue;">**Workspace:**</mark> The workspace is where you deploy your ML services. Each workspace maps directly to a kubernetes namespace and allocated a fixed amount of resources in terms of cpu and memory.

<mark style="color:blue;">**Service:**</mark> The service in a workspace can be:

1. Web services which serve your inference as a REST endpoint.
2. Web application which provide an interactive HTML based UI.

<mark style="color:blue;">**Pods:**</mark> The pod in a service is an actual physical host which is running your service.

<mark style="color:blue;">**Deployments:**</mark> The deployment refers to the process of deploying new code or configuration change in your new service.

<mark style="color:blue;">**Environment:**</mark> The environment refers to the key value pairs which are set as environment variables in all the pods of the services.

<mark style="color:blue;">**Grafana:**</mark> The grafana is an analytics dashboard where you can access metrics, logs and alarms of your service.

<mark style="color:blue;">**Secret Group:**</mark> The Secret group is where you can create your secret key value.

<mark style="color:blue;">**Secret:**</mark> The Secret is pair of key and value in a secret group.

> > > > > > > Stashed changes
