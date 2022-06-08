# get\_all\_projects

Returns name of all projects that has been created.

### Returns:

(List) - List of project names created already.

### Example
```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
mlf_projects = mlf_api.get_all_projects()
```
