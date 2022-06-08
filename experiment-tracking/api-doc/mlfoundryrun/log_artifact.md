# log\_artifact

Log an artifact using mlfoundry run.

| Parameters  | Description                                                                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| local_path | <p>(str) Local path in which the artifact is present. |
| artifact_path | <p>(str, optional) Relative path in which to store the artifact using the run object. This argument can be used to create a folder structure to store your artifacts. |

### Example
```python
mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name=<project-name>)

mlf_run.log_artifact(
    local_path=<local-path-of-artifact>
    artifact_path="artifacts"
)
```
