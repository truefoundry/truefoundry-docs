# Bootstrap Servicefoundry

Servicefoundry enables data scientists and MLEs to deploy workloads (any application / deployment / job) to 
any Kubernetes cluster. For this we need to tell ServiceFoundry which cluster to use. 

ServiceFoundry itself serves as the ControlPlane which can coordinate deployments across multiple clusters. The workload
clusters can be residing in any other region or in multiple cloud providers. 

![ServiceFoundry Architecture](../assets/servicefoundry-architecture.png)

We can always use the same physical cluster as the control plane and the tfy-workload cluster so that servicefoundry will deploy the 
workloads on the same cluster where it is deployed.

## Adding local cluster as workload cluster

We will now add the same physical cluster as the workload cluster also. This will be the most easiest flow to get started. If you want to add
another cluster as the workload cluster, please follow the next section. 

Once you login, please go to the clusters tab:

![Clusters Initial View](../assets/cluster-start.png)

ServiceFoundry by default uses the current cluster - but we need to bootstrap it. To do that, please click on the `Connect` button.

This will then show the instruction to install the tfy-workload helm chart.

### Install tfy-workload helm chart

To do this, we need to do the following:

Create a values file (tfy-workload.yaml) as shown below. Fill up the values as provided by Truefoundry team
and what is relevant for your cluster. You can see the complete values file [here](https://github.com/truefoundry/charts/blob/main/charts/tfy-workload/values.yaml)

```
global:
  # The below are installation secrets that will be provided as is.
  # These are customised for your account. Please use the provided values as
  # is.
  imagePullCredentials: '<to be provided by Truefoundry Team>'

  # Auth token for your workload cluster
  sfyAgentToken: ''<Copy from the token provided in UI>''

  # Host for the control plane
  controlPlaneHost: '<Url for the control plane dashboard>'

  # Configure the below with the url you wish to set up the workload on.
  # NOTE: This host should be accessible to control-plane
  workloadHost: ''

  # Base domain url that will be used to map your services against this cluster
  baseDomainUrl: '<base domain attached to this cluster>'

  # Monitoring: we can provision grafana, loki, prometheous stacks within your cluster
  # 1. disabled : No monitoring components will be setup by truefoundry
  # 2. external: provide externalGrafanaUrl to redirect users from dashboard
  # Note: Incase of local, we map grafana to grafana.<baseDomainUrl>
  monitoring: disabled | external

  # This is mandatory incase of monitoring: external
  externalGrafanaUrl: ''

################################
# Below configuration is option
################################

sfy-proxy:
  replicaCount: 2
  # You can choose to configure any one for dashboard ingress
  ingress:
    enabled: false
    annotations: {}
    labels: {}
    ingressClassName: istio
    tls: []
    hosts: []
    # hosts:
    #   - <workloadHost>
  istio:
    virtualservice:
      enabled: false
      annotations: {}
      gateways: []
      hosts: []
      # hosts:
      #   - <workloadHost>
  serviceAccount:
    annotations: {}

sfy-agent:
  serviceAccount:
    annotations: {}

sfy-operators:
  serviceAccount:
    annotations: {}
```

Install the helm chart with this values file:

```
helm upgrade --install tfy-workload truefoundry/tfy-workload -f tfy-workload.yaml --wait
```

The above command can take a few seconds since it will wait for all the pods to come up. Once its done, check
the final staus using the following command:

// TODO: Add kubectl command to observe status

Once the pods are up and if everything is correct, you should see the status of the cluster as Connected on the UI. You might need to refresh the page to see the effect. Once its connected, you are all set to deploy workloads on the cluster.

// TODO: Add connect cluster screenshot
![Connected Cluster](../assets/connected-cluster.png)


### Adding another cluster as workload cluster

The workflow to add another cluster is quite similar to the flow above. In this case, you need to click on `New Cluster` and provide the following inputs. 

1. Name: This can be any string that helps yuo identify the cluster later (maybe something like example-org-aws-production)
2. Region: This specifies the region in which the cluster is present. It can be any string - but we recommend you to put the region as the region string of the cloud provider you are using. 

Once you have added the cluster, you will need to install the tfy-workload helm chart in the target cluster using the same instructions as done for local cluster. 


