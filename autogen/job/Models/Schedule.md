# Schedule
## Properties

| Name | Type | Description | Notes |
|------------ | ------------- | ------------- | -------------|
| **type** | **String** |  | [default to null] |
| **schedule** | **String** | +usage&#x3D;Specify the schedule for this job to be run periodically in cron format. | [default to null] |
| **concurrency\_policy** | **String** | +usage&#x3D;Choose whether to allow this job to run while another instance of the job is running, or to replace the currently running instance. Allow will enable multiple instances of this job to run. Forbid will keep the current instance of the job running and stop a new instance from being run. Replace will terminate any currently running instance of the job and start a new one. | [optional] [default to Allow] |

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)

