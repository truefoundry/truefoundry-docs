# Resources

For all deployments, we need to specify the resource constraints for the application so that it can be deployed accordingly 
on the cluster. The key resources to be specified are:

### CPU

CPU represents compute processing and is specifies as a number. 1 CPU unit is equivalent to 1 physical CPU core, or 1 virtual core, depending on whether the node is a physical host or a virtual machine running inside a physical machine.
We can also specify fractional CPU like `0.2` or even `0.02`. 

For CPU, we need to specify a `cpu_request` and `cpu_limit` param. 

`cpu_request` helps specify the amount of CPU that will
always be reserved for the application. This also means that the minimum cost that I incur for this application is going 
to be the cost of `cpu_request` number of CPUs. If the application always uses CPU lower than the `cpu_request`, you can 
assume it to run in a healthy way. 

`cpu_limit` helps specify the upper limit on cpu usage of the application, beyond which the application will be throttled and not allowed any more CPU usage. This helps safeguard other applications running on the same node since one misbehaving
application cannot interfere and reduce the resources available to the other applications. 

`cpu_limit` has to be always greater than or equal to `cpu_request`.

How do you set `cpu_request` and `cpu_limit` ? 

### Memory

### Disk size (Coming soon)

### GPU (Coming soon)

