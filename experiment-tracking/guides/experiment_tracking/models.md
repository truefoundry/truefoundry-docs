# Logging Models

You can automatically serialize and save model objects as artifacts using the `log_model` method.

This is an example of serializing and storing an `sklearn` model.

```python
import mlfoundry
import numpy as np
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC

client = mlfoundry.get_client()
run = client.create_run(project_name="my-first-project")
X = np.array([[-1, -1], [-2, -1], [1, 1], [2, 1]])
y = np.array([1, 1, 2, 2])
clf = make_pipeline(StandardScaler(), SVC(gamma='auto'))
clf.fit(X, y)

run.log_model(model=clf, framework="sklearn")

run.end()
```

You can find framework specific details [here](../../api-doc/experiment-tracking/mlfoundryrun/log_model.md).

#### What are the frameworks supported by the `log_model` method?

This method supports, "sklearn", "tensorflow", "pytorch", "keras", "xgboost", "lightgbm", "fastai", "h2o", "spacy", "statsmodels", "gluon", "paddle".

#### In a run how do I save multiple models?

Right now, you can only save one model per run using this method. We are working on step-wise model logging.

#### How can I download or load a model for a run?

You can use the `download_model` function to download a logged model.

```python
import mlfoundry

client = mlfoundry.get_client()
run = client.get_run("run-id-of-the-run")

run.download_model(dest_path="./")
```

Use the `get_model` function to deserialize and load the logged model.


```python
import mlfoundry

client = mlfoundry.get_client()
run = client.get_run("run-id-of-the-run")

model = run.get_model()
```
