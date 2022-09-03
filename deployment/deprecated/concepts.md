# Concepts

![Deployment Concepts](/assets/deployment-concepts.png)

**Cluster**: The term cluster refers to a [Kubernetes cluster](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/). TrueFoundry deploys all services to Kubernetes clusters.

**Workspace**: A workspace in TrueFoundry is a collection of services. A user will have the same set of permissions on all services inside a workspace. The workspace also maps to namespace in the Kubernetes cluster. 

**Service**: Service refers to the application/job that you are deploying. For example, your ML model can be deployed as a:

* A web serivice that serves your model inference as REST APIs.
* A web application which provides an interactive UI to test your model predictions.

**Deployment**: A deployment is a certain set of code and configuration of the service. The latest deployment will usually be the active deployment of a service that is running on the cluster.

**Pod**: A pod is an actual physical host which is running your service.

**Environment**: Environment refers to the set of key value pairs that are made available as environment variables in all the pods of a service.

**Secret:**: A TrueFoundry secret is a key value pair where the value is some sensitive text. You can create secrets and [inject them into the environment of a deployment](./advance_examples/secret-env-vars.md) for use with the service.

**Secret Group**: Secret group referes to a collection of secrets.


