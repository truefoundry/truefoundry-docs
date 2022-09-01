# Table of contents

* [Introduction](introduction.md)
* [Quick Start](quick-start.md)
* Track Experiments
  * [Overview](experiment-tracking/overview.md)
  * Quick Start
    * [Installation and setup](experiment-tracking/getting-started/setup.md)
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
    * [Log Dataset](experiment-tracking/log-data/log-dataset.md)
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
    * [Installation and setup](deployment/quickstart/install-and-workspace.md)
    * [More examples](deployment/quickstart/more-examples.md)
  * Environment Variables and Secrets
    * [Adding environment variables](deployment/advance_examples/adding-env-vars.md)
    * [Using secrets in your application](deployment/advance_examples/secret-env-vars.md)
  * [Train and deploy model](deployment/train_and_deploy_model.md)
  * [Deploy a streamlit app](deployment/streamlit.md)
  * [Deploy a Dockerized service](/deployment/docker.md)
  * [Adding GPUs to your service](deployment/advance_examples/adding-gpus.md)
  * [Monitoring your services](./deployment/monitoring.md)
  * [Cost Estimation](./deployment/costing/cost-estimation.md)
  * [Collaboration with team](deployment/collab.md)
  * [CI/CD](./deployment/advance_examples/ci-pipeline-integration.md)
* [Getting Help](getting-help.md)
* Deploying on your own cloud
  * [Overview](deploy-on-own-cloud/overview.md)
  * [Requirements](deploy-on-own-cloud/requirements.md)
  * [Local Installation](deploy-on-own-cloud/local-installation.md)
  * [Production Installation](deploy-on-own-cloud/production-installation.md)
  * [Bootstrap Servicefoundry](deploy-on-own-cloud/servicefoundry-bootstrap.md)
  * [Client Setup](deploy-on-own-cloud/client-setup.md)
  * [Adding Users](deploy-on-own-cloud/add-users.md)
* Coming Soon
  * Deploy Model
    * Deploy your existing project with TrueFoundry
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
    * Deploy From UI
    * Deploy your training code
    * Deploy pipelines
    * Deploy Applications
      * JupyterHub
    * Cost Optimization
      * Sleep services
      * Sleep workspace
  * Demo Your Model
    * Create using Streamlit
    * Create using Gradio
    * Deploy on TrueFoundry and Share
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
  * End to End Examples
    * Iris Model
    * Image Segmentation Model / Object Detection Model
    * Question Answering Model
    * Ranking Model
    * Churn Prediction Model
  * Model Traffic Shaping
    * Canary Rollout
    * Shadow Test your new model
    * A/B Test your new model
  * Need something custom?

## Reference
* [Servicefoundry CLI](deployment/reference/cli/README.md)
  * [sfy deploy](deployment/reference/cli/sfy-deploy/README.md)
  * [sfy login](deployment/reference/cli/sfy-login.md)
  * [sfy logout](deployment/reference/cli/sfy-logout.md)