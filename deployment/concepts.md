# Concepts

![Concepts](/assets/deployment-concepts.png)

**Cluster**: The term cluster refers to a Kubernetes cluster. TrueFoundry deploys all services to Kubernetes clusters.

**Workspace**: A Workspace in TrueFoundry is a collection of services. A user will have the same set of permissions on all services inside a workspace. The workspace also maps to namespace in the Kubernetes cluster. 

**Service**: Service refers to the application/job that you are deploying. For example, your ML model service could be a:

* A web serivice that serves your model inference as REST APIs.
* A web application which provides an interactive UI to test your model predictions.

**Deployment**: A deployment is a certain set of code and configuration of the service that is deployed to the Workspace. The latest set of code and configuration that was deployed will be the active deployment for a service.

**Pod**: A pod in a service is an actual physical host which is running your service.

**Environment**: Environment refers to the set of key value pairs that are made available as environment variables in all the pods of a service.

**Secret:**: A secret is a key value pair where the value is sensitive text. Secrets can be injected into the environment of a deployment for use with the service

**Secret Group**: Secret group referes to a collection of secrets.


