# Bootstrap Servicefoundry

Servicefoundry enables data scientists and MLEs to deploy workloads (any application / deployment / job) to 
any Kubernetes cluster. For this we need to tell ServiceFoundry which cluster to use. 

ServiceFoundry itself serves as the ControlPlane which can coordinate deployments across multiple clusters. The workload
clusters can be residing in any other region or in multiple cloud providers. 

![ServiceFoundry Architecture](../assets/servicefoundry-architecture.png)

