# Installation

To install Truefoundry in your own cloud, please make sure you have the following things ready as 
mentioned in the [requirements](./requirements.md) page. 

1. Kubernetes Cluster
2. S3 Buckets
   1. Bucket Name and credentials to access them. 
3. Postgres DBs
   1. Host, Port, Username, Password and DB name. 
4. Information from Truefoundry Team
   1. Tenant ID
   2. Password of the initial user
   3. Image Pull Secret to pull the docker images

This guide will walk you through the process of setting up Truefoundry. 

## Install Truefoundry Helm-Chart

Make sure that [helm](https://helm.sh/docs/intro/install/) is installed.

### Add Truefoundry Helm Repo

```
helm repo add truefoundry https://truefoundry.github.io/charts
helm repo update
helm repo list
```

### Install Truefoundry helm chart

Create a values file (values.yaml) as shown below. Fill up the values as provided by Truefoundry team
and what is relevant for your cluster. 

TODO: Fix the values file below

```
global:
  imagePullCredentials: <to_be_provided_by_truefoundry>
  authTenantId: <to_be_provided_by_truefoundry>

  mlfoundry_enabled: true / false (If you want experiment tracking / ml metadata store)
  servicefoundry_enabled: true / false (If you want model deployment)

  controlPlaneHost: example.organization.com (Hostname for the ingress)

truefoundry-frontend-app:
  replicaCount: 1
  # You can choose to configure generic ingress or istio virtual service
  ingress:
    enabled: false
    annotations: {}
    labels: {}
    ingressClassName: istio
    tls: []
    hosts: []
  istio:
    virtualservice:
      enabled: false
      annotations: {}
      gateways: []
      hosts: []
  # These details will be autofilled
  imagePullCredentials: "{{ .Values.global.imagePullCredentials }}"
  env:
    VITE_SERVICEFOUNDRY_SERVER_ENABLED: "{{ .Values.global.servicefoundry_enabled }}"
    VITE_MLFOUNDRY_SERVER_ENABLED: "{{ .Values.global.mlfoundry_enabled }}"
    VITE_TENANT_ID: "{{ .Values.global.authTenantId }}"
    VITE_PUSHER_KEY: "{{ .Values.global.PUSHER_KEY }}"
    VITE_PUSHER_CLUSTER: "{{ .Values.global.PUSHER_CLUSTER }}"
    SERVICEFOUNDRY_SERVER_URL: 'http://{{ .Release.Name }}-servicefoundry-server.{{ .Release.Namespace }}.svc.cluster.local:3000'
    MLFOUNDRY_SERVER_URL: 'http://{{ .Release.Name }}-mlfoundry-server.{{ .Release.Namespace }}.svc.cluster.local:5000'

#############################
# Settings specific to mlfoundry.
mlfoundry-server:
  enabled: true
  replicaCount: 1
  imagePullCredentials: "{{ .Values.global.imagePullCredentials }}"
  env:
    # Database config mandatory
    DB_USERNAME: ''
    DB_PASSWORD: ''
    DB_NAME: ''
    DB_HOST: ''
    DB_PORT: 5432
    # S3 bucket name mandatory
    ARTIFACT_ROOT: "s3://<mlfoundry_bucket_name>"
    # No change needed here
    AUTH_TENANT_ID: "{{ .Values.global.authTenantId }}"
  serviceAccount:
    create: true
    annotations:
      # Provide permission to s3 using role_arn or anything compatible
      eks.amazonaws.com/role-arn: <role_arn>

#######################
# Settings specific to servicefoundry.
servicefoundry-server:
  enabled: true
  replicaCount: 1
  imagePullCredentials: "{{ .Values.global.imagePullCredentials }}"
  serviceAccount:
    create: true
    annotations:
      eks.amazonaws.com/role-arn: "{{ .Values.IAMRoleArn }}"
  env:
    S3_BUCKET_NAME: <s3_bucket_name>
    # API KEY recieved from truefoundry for servicefoundry
    SVC_FOUNDRY_SERVICE_API_KEY: <API_KEY>
    # Database config mandatory
    DB_USERNAME: ''
    DB_PASSWORD: ''
    DB_NAME: ''
    DB_HOST: ''
    DB_PORT: 5432
    # These fill be auto-filled
    AUTH_TENANT_ID: "{{ .Values.global.authTenantId }}"
    CONTROL_PLANE_URL: 'https://{{ .Values.global.controlPlaneHost }}'
    IMAGE_PULL_CREDENTIALS: "{{ .Values.global.imagePullCredentials }}"
    PUSHER_APP_ID: "{{ .Values.global.PUSHER_APP_ID }}"
    PUSHER_KEY: "{{ .Values.global.PUSHER_KEY }}"
    PUSHER_SECRET: "{{ .Values.global.PUSHER_SECRET }}"
    PUSHER_CLUSTER: "{{ .Values.global.PUSHER_CLUSTER }}"
    WORKSPACE_PER_USER_WITHOUT_PAYMENT: '10'
```

Install the helm chart with this values file:

```
helm upgrade --install truefoundry truefoundry/truefoundry -f values.yaml
```





