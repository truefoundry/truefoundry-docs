# Table of contents

* [Introduction](introduction.md)
* [Quick Start](quick-start.md)
* Track Experiments
  * [Overview](experiment-tracking/README.md)
  * [Quick Start](experiment-tracking/quick-start.md)
  * Guide
    * [Setting up MLFoundry](experiment-tracking/guides/experiment_tracking/setup.md)
    * [Creating a run](experiment-tracking/guides/experiment_tracking/run.md)
    * [Capturing tags and hyperparameters](experiment-tracking/guides/experiment_tracking/tags_and_params.md)
    * [Capturing metrics over time](experiment-tracking/guides/experiment_tracking/metrics.md)
    * [Logging Artifacts](experiment-tracking/guides/experiment_tracking/artifacts.md)
    * [Logging Models](experiment-tracking/guides/experiment_tracking/models.md)
    * [Logging Tabular Datasets](experiment-tracking/guides/experiment_tracking/tabular_datasets.md)
  * Integrations
    * [HuggingFace Trainer](experiment-tracking/guides/integrations/hf_trainer.md)
  * [Examples](experiment-tracking/examples.md)

  ## API Doc
  * [Experiment Tracking](experiment-tracking/api-doc//README.md)
    * [MLFoundryAPI](experiment-tracking/api-doc//mlfoundryapi/README.md)
      * [get\_client](experiment-tracking/api-doc//mlfoundryapi/get_client.md)
      * [create\_run](experiment-tracking/api-doc//mlfoundryapi/create_run.md)
      * [get\_run](experiment-tracking/api-doc//mlfoundryapi/get_run.md)
      * [get\_all\_runs](experiment-tracking/api-doc//mlfoundryapi/get_all_runs.md)
      * [get\_all\_projects](experiment-tracking/api-doc//mlfoundryapi/get_all_projects.md)
    * [MLFoundryRun](experiment-tracking/api-doc//mlfoundryrun/README.md)
      * [log\_model](experiment-tracking/api-doc//mlfoundryrun/log_model.md)
      * [log\_dataset](experiment-tracking/api-doc//mlfoundryrun/log_dataset.md)
      * [log\_metrics](experiment-tracking/api-doc//mlfoundryrun/log_metrics.md)
      * [log\_params](experiment-tracking/api-doc//mlfoundryrun/log_params.md)
      * [log\_dataset\_stats](experiment-tracking/api-doc//mlfoundryrun/log_dataset_stats.md)
      * [log\_artifact](experiment-tracking/api-doc//mlfoundryrun/log_artifact.md)
      * [set\_tags](experiment-tracking/api-doc//mlfoundryrun/set_tags.md)
      * [get\_dataset](experiment-tracking/api-doc//mlfoundryrun/get_dataset.md)
      * [get\_metrics](experiment-tracking/api-doc//mlfoundryrun/get_metrics.md)
      * [get\_params](experiment-tracking/api-doc//mlfoundryrun/get_params.md)
      * [get\_model](experiment-tracking/api-doc//mlfoundryrun/get_model.md)
      * [get\_tags](experiment-tracking/api-doc//mlfoundryrun/get_tags.md)
      * [download\_artifact](experiment-tracking/api-doc//mlfoundryrun/download_artifact.md)
    * [MlFlow API](experiment-tracking/api-doc//mlflow-api.md)
    * [Enums](experiment-tracking/api-doc//enums.md)
    * [Get Client](experiment-tracking/api-doc//get_client.md)

* Deploy Model [Coming Soon]
  * Getting Started
    * Installation and Setup
    * Deploy code using truefoundry
    * Concepts 
    * Examples
  * Create a new project with truefoundry
  * Deploy your existing project with truefoundry
  * Environment Variables and Secrets
    * Adding environment variables
    * Creating secrets
    * Using secrets in your application
  * Deploying Models
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
  * Deploy from Jupyter Notebook
  * Deploy From UI
  * Deploy your training code
  * Deploy pipelines
  * Deploy Applications
    * JupyterHub
  * Cost Estimation and Optimization
    * Sleep services
    * Sleep workspace
  * Collaboration with Team
* Demo Your Model [Coming Soon]
  * Create using Streamlit
  * Create using Gradio
  * Deploy on Truefoundry and Share
* Monitoring  [Coming Soon]
  * Introduction
  * Service Health
    * Metrics
    * Logs 
    * Add custom metrics
  * Pipeline Health
  * Model Health
  * Feature Health
  * Data Health
* Model Traffic Shaping
  * Canary Rollout
  * Shadow Test your new model
  * A/B Test your new model
* Need something custom? 
* End to End Examples
  * Iris Model
  * Image Segmentation Model / Object Detection Model
  * Question Answering Model
  * Ranking Model
  * Churn Prediction Model
* [Getting Help](getting-help.md)
* [Deploying on your own cloud](deploy-on-own-cloud/getting-started.md)

