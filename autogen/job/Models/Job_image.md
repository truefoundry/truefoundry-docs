# Job_image
## Properties

| Name | Type | Description | Notes |
|------------ | ------------- | ------------- | -------------|
| **type** | **String** |  | [default to null] |
| **docker\_registry** | **String** | +usage&#x3D;FQN of the container registry. If you can&#39;t find your registry here, add it through the [Settings](/settings?tab&#x3D;registry) page | [optional] [default to null] |
| **image\_uri** | **String** | +label&#x3D;Image URI +usage&#x3D;The image URI. Specify the name of the image and the tag. If the image is in Dockerhub, you can skip registry-url (for e.g. &#x60;tensorflow/tensorflow&#x60;). You can use an image from a private registry using Advanced fields +placeholder&#x3D;registry-url/account/image:version (e.g. docker.io/tensorflow/tensorflow) | [default to null] |
| **build\_source** | [**Build_build_source**](Build_build_source.md) |  | [default to null] |
| **build\_spec** | [**Build_build_spec**](Build_build_spec.md) |  | [default to null] |

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)

