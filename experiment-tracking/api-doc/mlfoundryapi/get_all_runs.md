# get\_all\_runs

Given a project\_name, get\_all\_runs returns all the run's associated with it. A Dataframe containing all the runs is returned.

| Parameters                                         | Description                                                                         |
| -------------------------------------------------- | ----------------------------------------------------------------------------------- |
| <mark style="color:blue;">**project\_name**</mark> | (str) name of the project, every run's id and name inside this project is returned. |


### Returns:
(pd.DatFrame) - A dataframe with run\_id and run\_name as columns.
### Example:
```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
mlf_run_1 = mlf_api.create_run(project_name=<project-name>, run_name = "sklearn_model")
mlf_run_2 = mlf_api.create_run(project_name=<project-name>, run_name = "")

## DO some logging with the run objects

mlf_api.get_all_runs(project_name=<project-name>)
```
