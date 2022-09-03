# Resources
## Properties

| Name | Type | Description | Notes |
|------------ | ------------- | ------------- | -------------|
| **cpu\_request** | **String** | +label&#x3D;CPU Request +sort&#x3D;1 +usage&#x3D;Requested CPU which determines the minimum cost incurred. The CPU usage can exceed the requested amount, but not the value specified in the limit. 1 CPU means 1 CPU core. Fractional CPU can be requested like &#x60;0.5&#x60; or you can express it in milli units like &#x60;50m&#x60; which is equal to 0.05 units. You can read more about it [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) | [default to 200m] |
| **cpu\_limit** | **String** | +label&#x3D;CPU Limit +usage&#x3D;CPU limit beyond which the usage cannot be exceeded. 1 CPU means 1 CPU core. Fractional CPU can be requested like &#x60;0.5&#x60; or you can express it in milli units like &#x60;50m&#x60; which is equal to 0.05 units. You can read more about it [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) +sort&#x3D;2 | [optional] [default to 200m] |
| **memory\_request** | **String** | +label&#x3D;Memory Request +usage&#x3D;Requested memory which determines the minimum cost incurred. The unit of memory is in bytes. We can specify the plain integer or using a quantity suffix like k, M, G (400k is 400 kilobytes, 1M is 1 Megabyte and 2G is 2 gigabytes.) You can read more about it [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) +sort&#x3D;3 | [default to 256Mi] |
| **memory\_limit** | **String** | +label&#x3D;Memory Limit +usage&#x3D;Memory limit after which the application will be killed with an OOM error. The unit of memory is in bytes. We can specify the plain integer or using a quantity suffix like k, M, G (400k is 400 kilobytes, 1M is 1 Megabyte and 2G is 2 gigabytes.) You can read more about it [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) +sort&#x3D;4 | [optional] [default to 256Mi] |
| **gpu\_limit** | **String** | +label&#x3D;GPU Limit +usage&#x3D;GPU limit for the component. It can be a plain integer. +sort&#x3D;5 | [optional] [default to 0] |
| **gpu\_vendor** | **String** | +label&#x3D;GPU Vendor +usage&#x3D;GPU vendor. Defaults to \&quot;nvidia\&quot; +sort&#x3D;6 | [optional] [default to nvidia] |

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)

