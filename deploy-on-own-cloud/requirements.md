# Requirements

We recommend the following requirements for installation of Truefoundry. Currently, we only have the instructions for AWS
with support for other clouds coming soon.

### Kubernetes Cluster:

Kubernetes version should be greater than v1.20+. If you don't have an existing Kubernetes cluster, you can spin up an 
EKS cluster. We also need an ingress controller to be installed on the cluster. Any Ingress Controller like NGinx works, 
but to reap all the features of Truefoundry platform, we recommend Istio.

### S3 Buckets:

MlFoundry needs one S3 bucket and ServiceFoundry needs one S3 bucket. So if you are installing both, we need 2 S3 buckets to be provisioned.

### Postgres Database:

We need one Postgres database each for MLFoundry, ServiceFoundry and ML-Monitoring. You can provision one RDS on AWS and create three databases on it with the name mlfoundry, svcfoundry and ml-monitoring or provide different databases. 

We also provide a [Terraform module](https://github.com/truefoundry/tfy-terraform) to provision the DB and S3 buckets on AWS which can be found at https://github.com/truefoundry/tfy-terraform

### Tenant ID 

You will need a tenantId to install Truefoundry. You will be getting this from the Truefoundry team prior to the installation. You will also get the password for the initial admin user of the platform. 


## Networking Configuration
1. Truefoundry services should have access to dockerhub registry - the credentials for which will be provided by Truefoundry.

2. The clients (mlfoundry and servicefoundry) and all other services should have egress access to auth.production.truefoundry.com