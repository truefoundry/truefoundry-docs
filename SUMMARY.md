# Table of contents

* [Introduction](introduction.md)
* [Quick Start](quick-start.md)
* Track Experiments
  * Getting Started
    * [Installation and Setup](experiment-tracking/getting-started/installation-and-setup.md)
    * [Concepts](experiment-tracking/getting-started/concepts.md)
    * [Add mlfoundry to your code](experiment-tracking/getting-started/add-mlfoundry.md)
    * [Examples](experiment-tracking/getting-started/examples.md)
  * Log Data
    * Creating a run
    * Capturing tags and hyperparameters
    * Capturing Metrics over time
    * Logging Artifacts
    * Logging Models
    * Logging Dataset
      * Tabular Dataset
      * Non-tabular Dataset
    * Logging Prediction service
  * Compare Runs
    * Compare metrics
    * Compare datasets
  * Migrating from MLFlow
  * Collaboration with Team
  * API Doc 
* Deploy Model
  * Getting Started
    * Installation and Setup
    * Deploy code using servicefoundry
    * Concepts 
    * Examples
  * Create a new project with truefoundrym
  * Deploy your existing project with truefoundry
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
    * Not sure of best way to deploy? Read our guide.
  * Deploy pipelines
  * Cost Estimation and Optimization
  * Collaboration with Team
* Monitoring
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
* Getting Help
* Deploying on your own cloud




<!-- * Guides
  * Experiment Tracking
    * [Setting up MLFoundry](guides/experiment_tracking/setup.md)
    * [Creating a run](guides/experiment_tracking/run.md)
    * [Capturing tags and hyperparameters](guides/experiment_tracking/tags_and_params.md)
    * [Capturing metrics over time](guides/experiment_tracking/metrics.md)
    * [Logging Artifacts](guides/experiment_tracking/artifacts.md)
    * [Logging Models](guides/experiment_tracking/models.md)
    * [Logging Tabular Datasets](guides/experiment_tracking/tabular_datasets.md)
  * Integrations
    * [HuggingFace Trainer](guides/integrations/hf_trainer.md)
* [Examples](examples.md)

## API Doc
* [Experiment Tracking](api-doc/experiment-tracking/README.md)
  * [MLFoundryAPI](api-doc/experiment-tracking/mlfoundryapi/README.md)
    * [get\_client](api-doc/experiment-tracking/mlfoundryapi/get_client.md)
    * [create\_run](api-doc/experiment-tracking/mlfoundryapi/create_run.md)
    * [get\_run](api-doc/experiment-tracking/mlfoundryapi/get_run.md)
    * [get\_all\_runs](api-doc/experiment-tracking/mlfoundryapi/get_all_runs.md)
    * [get\_all\_projects](api-doc/experiment-tracking/mlfoundryapi/get_all_projects.md)
  * [MLFoundryRun](api-doc/experiment-tracking/mlfoundryrun/README.md)
    * [log\_model](api-doc/experiment-tracking/mlfoundryrun/log_model.md)
    * [log\_dataset](api-doc/experiment-tracking/mlfoundryrun/log_dataset.md)
    * [log\_metrics](api-doc/experiment-tracking/mlfoundryrun/log_metrics.md)
    * [log\_params](api-doc/experiment-tracking/mlfoundryrun/log_params.md)
    * [log\_dataset\_stats](api-doc/experiment-tracking/mlfoundryrun/log_dataset_stats.md)
    * [log\_artifact](api-doc/experiment-tracking/mlfoundryrun/log_artifact.md)
    * [set\_tags](api-doc/experiment-tracking/mlfoundryrun/set_tags.md)
    * [get\_dataset](api-doc/experiment-tracking/mlfoundryrun/get_dataset.md)
    * [get\_metrics](api-doc/experiment-tracking/mlfoundryrun/get_metrics.md)
    * [get\_params](api-doc/experiment-tracking/mlfoundryrun/get_params.md)
    * [get\_model](api-doc/experiment-tracking/mlfoundryrun/get_model.md)
    * [get\_tags](api-doc/experiment-tracking/mlfoundryrun/get_tags.md)
    * [download\_artifact](api-doc/experiment-tracking/mlfoundryrun/download_artifact.md)
  * [MlFlow API](api-doc/experiment-tracking/mlflow-api.md)
  * [Enums](api-doc/experiment-tracking/enums.md)
  * [Get Client](api-doc/experiment-tracking/get_client.md)
 -->
