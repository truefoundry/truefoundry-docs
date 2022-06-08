# Overview

## What is MLFoundry?

MLFoundry is **world's first integrated experimented tracking & inference monitoring solution**. Our client-side library allows you to log your experiments, models, metrics, data, features & predictions. Our interactive & intuitive dashboards allows you to track your Machine Learning models during develoment (experiment tracking) and post deployment (inference monitoring).

* **Experiment Tracking:** Helps you to create reproducible Machine Learning experiments and collaborate with your team by tracking your code, artefacts, hyperparameters & metrics all in one place. This is built on top of [MLFlow](https://www.mlflow.org) where we solve for a lot of issues like Role Based Access Control, first class support for non-scalar metrics & full data observability. This is already live- get started [here](quick-start.md).
* **Inference Monitoring:** Helps you to track your model and feature health during inference. Supports model input/ output monitoring, feature drift and distribution tracking. Our dashboards presents a unique hierarchical view of the system to perform root cause analysis where you can see metrics at the service level and debug it down to a row level. MLFlow APIs are extended to support for logging model predictions. This is coming soon, if you are interested, email us at nikunj@truefoundry.com.

You can use the two parts above as separate standalone modules if one is less relevant in your context.

## Why build an integrated Experiment Tracking with Inference Monitoring?

Machine Learning Inference monitoring makes sense only in the context of training metrics-
* From a model input standpoint i.e. Feature & Label Distributions. E.g. Feature drift relative to training dataset is one of the most common indicators of model performance.
* From a model output standpoint i.e. Performance Metrics. E.g. An inference accuracy of 0.845 is fine if training accuracy was 0.84 but is a disaster if training accuracy was 0.96.

In this context, there are two main reasons why these two solutions go together-

* During experiment tracking, we are already logging all relevant model artifacts, model metrics and training data distributions. If you use a completely different solution for model monitoring, all this information needs to be provided again.
* Developers need to build different mental models for projects, runs, model versions which are relevant for both experiment tracking and monitoring. This also involves familiarizing themselves with multiple frameworks and a lot more boiler-plate code.
