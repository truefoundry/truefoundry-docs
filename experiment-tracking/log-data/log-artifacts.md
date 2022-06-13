### Logging artifacts

An artifacts is a local file or a directory. We can log artifacts to remote storage and retrieve later. You can log your data files, serialized optimizer configurations, tokenizer and its metadata as artifact. The remote artifact storage is similar to a file-system dedicated for each run.

While logging artifact, optionaly you can pass an directory path through `artifact_path` argument. If passed, then the artifact will be stored in the passed directory on the remote storage. Otherwise it is stored at the root path.

```python
import os
import mlfoundry

with open("artifact.txt", "w") as f:
    f.write("hello-world")

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project")

run.log_artifact(local_path="artifact.txt", artifact_path="my-artifacts")

run.end()
```

This is how it looks like on the dashboard.

![Artifact Browser](/assets/guide_exp_artifacts.png)

#### Are artifacts versioned?

No. The process of artifacts logging is similar to the behavior of a `cp SOURCE DESTINATION` command on a Linux system.

#### How can I download the logged artifacts?

You can download the aritfacts to your local filesystem using the `download_artifact` method.
Using the `path` argument, you can pass the remote directory path that you want to download.


```python
import os
import mlfoundry

with open("artifact.txt", "w") as f:
    f.write("hello-world")

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project")

run.log_artifact(local_path="artifact.txt", artifact_path="my-artifacts")

run_id = run.run_id
run.end()

run = client.get_run(run_id)

local_path = run.download_artifact(path="my-artifacts")
print(f"Artifacts: {os.listdir(local_path)}")
```