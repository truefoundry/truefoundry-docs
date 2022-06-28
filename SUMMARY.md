# Table of contents

* [Introduction](introduction.md)
* [Quick Start](quick-start.md)
* Track Experiments
  * [Overview](experiment-tracking/overview.md)
  * Quick Start
    * [Installation and Setup](experiment-tracking/getting-started/setup.md)
    * [Concepts](experiment-tracking/getting-started/concepts.md)
    * [Add MLFoundry to your code](experiment-tracking/getting-started/add-mlfoundry-to-code.md)
    * [Examples](experiment-tracking/getting-started/examples.md)
  * Log Data
    * [Creating a run](experiment-tracking/log-data/create-run.md) 
    * [Log parameters](experiment-tracking/log-data/log-params.md)
    * [Log Metrics over time](experiment-tracking/log-data/log-metrics.md)
    * [Log Artifacts](experiment-tracking/log-data/log-artifacts.md)
    * [Log Models](experiment-tracking/log-data/log-models.md)
    * [Add Tags](experiment-tracking/log-data/add-tags.md)
    * [Logging and Visualizing System Metrics](experiment-tracking/log-data/system-metrics.md)
    * [Logging images](experiment-tracking/log-data/log-image.md)
    * [Logging Plots](experiment-tracking/log-data/log-plots.md)
    * Log Dataset (Coming Soon)
    * Logging in an existing run (Coming Soon) 
  * Integrations
    * [HuggingFaceTrainer](experiment-tracking/integrations/hf-trainer.md)
  * Compare Runs
    * [Compare Metrics](experiment-tracking/compare-runs/compare-metrics.md)
    * [Oragnizing run-sets using tags](experiment-tracking/compare-runs/compare-with-tags.md)
    * Compare Datasets (Coming Soon)
  * Collaboration with team
    * [Roles and their meanings](experiment-tracking/collaboration/roles.md)
    * [Adding Collaborator](experiment-tracking/collaboration/add-collaborator.md)
    * Making Experiments Public (Coming Soon)
  * [Comparison with MlFlow](experiment-tracking/comparison-mlflow.md)

* Deploy Model
  * [Overview](deployment/README.md)
  * Quick Start
    * [Installation and Setup](deployment/quickstart/install-and-workspace.md)
    * [Concepts](deployment/concepts.md)
    * Examples(TBD)
  * Deploy from Jupyter Notebook
    * [Deploy Code using Notebook](deployment/quickstart/fastapi-quickstart.md)
  * Create a new project with Truefoundry
    * [Deploy REST Service using CLI](deployment/quickstart/fastapi-quickstart.md)
  * Deploy your existing project with Truefoundry(TBD)
  * Environment Variables and Secrets
    * Adding environment variables(TBD)
    * Creating secrets(TBD)
    * [Using secrets in your application(TBU)](deployment/advance_examples/adding-env-vars.md)
  * Deploying Models(TBD)
    * Deploy a simple predict function
    * Deploy model as fastapi service
    * Deploy mlfoundry model in one click
    * Deploy model using TensorFlowServe
    * Deploy model using PytorchServe
    * Deploy model using BentoML
    * Deploy model as a serverless function
    * Deploy multiple models in a single service
    * Deploy multiple models with dynamic loading
    * Deploy pretrained models
    * Deploy models on GPU
    * Not sure of best way to deploy? Read our guide.
  * Deploy From UI(Coming Soon)
  * Deploy your training code(Coming Soon)
  * Deploy pipelines(Coming Soon)
  * Deploy Applications(Coming Soon)
    * JupyterHub
  * Cost Estimation and Optimization(Coming Soon)
    * Sleep services
    * Sleep workspace
  * Collaboration with Team(Coming Soon)
* Demo Your Model (Coming Soon)
  * Create using Streamlit
  * Create using Gradio
  * Deploy on Truefoundry and Share
* Monitoring  (Coming Soon)
  * Introduction
  * Service Health
    * Metrics
    * Logs 
    * Add custom metrics
  * Pipeline Health
  * Model Health
  * Feature Health
  * Data Health
* Model Traffic Shaping(Coming Soon)
  * Canary Rollout
  * Shadow Test your new model
  * A/B Test your new model
* Need something custom? (Coming Soon)
* End to End Examples (TBD)
  * Iris Model
  * Image Segmentation Model / Object Detection Model
  * Question Answering Model
  * Ranking Model
  * Churn Prediction Model
* [Getting Help](getting-help.md)
* [Deploying on your own cloud](deploy-on-own-cloud/getting-started.md)

## Reference
* [Jupyter Notebook](deployment/api-doc/notebook/README.md)
  * [load_predict](deployment/api-doc/notebook/load_predictor.md)
  * [Predictor](deployment/api-doc/notebook/Predictor.md)
  * [sfn_load_predict](deployment/api-doc/notebook/sfn_load_predict.md)
  * [create_service](deployment/api-doc/notebook/create_service.md)
  * [deploy_local](deployment/api-doc/notebook/deploy_local.md)
  * [deploy](deployment/api-doc/notebook/deploy.md)

