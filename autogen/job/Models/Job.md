# Job
## Properties

| Name | Type | Description | Notes |
|------------ | ------------- | ------------- | -------------|
| **type** | **String** |  | [default to null] |
| **name** | **String** | +usage&#x3D;Name of the job +sort&#x3D;1 | [default to null] |
| **image** | [**Job_image**](Job_image.md) |  | [default to null] |
| **resources** | [**Job_resources**](Job_resources.md) |  | [default to null] |
| **trigger** | [**Job_trigger**](Job_trigger.md) |  | [default to null] |
| **count** | **Integer** | +label&#x3D;Count +usage&#x3D;Number of job instances you want to run. +icon&#x3D;fa-clone +sort&#x3D;5 | [optional] [default to 1] |
| **retries** | **Integer** | +label&#x3D;Retries +usage&#x3D;Specify the maximum number of attempts to retry a job before it is marked as failed. +icon&#x3D;fa-clone | [default to 1] |
| **timeout** | **Integer** | +label&#x3D;Timeout +usage&#x3D;Job timeout in seconds. +icon&#x3D;fa-clone | [optional] [default to 1000] |
| **successful\_jobs\_history\_limit** | **Integer** | +usage&#x3D;Number of successful invocation of this job to keep in history | [optional] [default to 100] |
| **failed\_jobs\_history\_limit** | **Integer** | +usage&#x3D;Number of failed invocation of this job to keep in history | [optional] [default to 100] |
| **restart** | **String** | +usage&#x3D; | [default to Never] |
| **command** | **List** | +label&#x3D;Commands +usage&#x3D;Commands to execute to start the application. Overrides the &#x60;Entrypoint&#x60; command. +icon&#x3D;fa-terminal +sort&#x3D;6 | [optional] [default to null] |
| **env** | [**List**](Env.md) | +label&#x3D;Environment Variables +usage&#x3D;Configure environment variables to be injected in the service. +icon&#x3D;fa-globe +sort&#x3D;7 | [optional] [default to null] |
| **labels** | **Map** | +usage&#x3D;Specify the labels in the workload | [optional] [default to null] |

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)

