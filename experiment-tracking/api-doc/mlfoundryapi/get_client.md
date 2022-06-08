# create\_run

Get an MLFoundry client object which is used to create and get runs and projects.

|                                                    | Description                                                                                                                                                                                 |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**tracking\_uri**</mark> | (str, optional) URL of hosted MLFoundry server.|
| <mark style="color:blue;">**inference\_store\_uri**</mark> | (str, optional, default=None) URL of Postgress DB where real time inference is logged. If not provided, data is logged to local file store. |
| <mark style="color:blue;">**api\_key**</mark>     | (str, optional, default=None) API key to authenticate transactions.|
| <mark style="color:blue;">**disable_analytics**</mark>     | (bool, optional, default=True) Set to False to turn off analytics and tracking. |

#### Returns

<mark style="color:blue;">**MLFoundry**</mark> object - an MLFoundry object is created with the specified tracking_uri, inference_store_uri and authenticated with the api_key.

#### Example

```python
import mlfoundry as mlf

mlf_api = mlf.get_client()  ## Log everything to local file store
mlf_api = mlf.get_client(tracking_uri="REPLACE_WITH_MLF_SERVER_URI", api_key="REPLACE_WITH_YOUR_API_KEY")
```
