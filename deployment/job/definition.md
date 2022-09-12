# Job

A job basically executes the code once and if it completes successfully, the job is marked as `Succeeded`. If it fails, the job can be configured to retry a certain number of times. If the code doesn't end even after the configured number of retries, the job is marked as `Failed`. The compute and memory resources are released once the job is completed and hence we don't incur any cost once the job completes.

Jobs can be triggered in multiple ways:

1. **Manual**: This is good for ad hoc use cases and can be triggered manually. An example can be a model training job which can be run when needed. 
2. **Schedule**: A job can be triggered on a schedule like daily, weekly, or at 9AM every Monday. An example of this can be a batch inference job running every morning at 8 AM on the previous day's incoming data. 

The code below shows an example job that trains a scikit learn model on iris dataset.

```
.
├── train.py
└── requirements.txt
```

**`requirements.txt`**
```
pandas==1.4.4
numpy==1.22.4
scikit-learn==1.1.2
```

**`train.py`**
```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC
from sklearn.metrics import classification_report

X, y = load_iris(as_frame=True, return_X_y=True)
X = X.rename(columns={
        "sepal length (cm)": "sepal_length",
        "sepal width (cm)": "sepal_width",
        "petal length (cm)": "petal_length",
        "petal width (cm)": "petal_width",
})

# NOTE:- You can pass these configurations via command line
# arguments, config file, environment variables.
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
pipe = Pipeline([("scaler", StandardScaler()), ("svc", SVC())])
pipe.fit(X_train, y_train)
print(classification_report(y_true=y_test, y_pred=pipe.predict(X_test)))
```
If we do `python train.py`, the code will run and then end. In the [next section](./deploy.md), we will see how we deploy this job to run in the cloud so that we can use more resources than what we have locally in our machines and also let it run for a much longer time without interruption. 



