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
    * [Concepts](deployment/concepts.md)
    * [Deploy a service](deployment/quickstart/fastapi-quickstart.md)
    * [More examples](deployment/quickstart/more-examples.md)
  * Deploy from Jupyter Notebook
    * [Deploy Code using Notebook](deployment/quickstart/notebook-quickstart.md)
  * Create a new project with Truefoundry
    * [Deploy REST Service using CLI](deployment/new-project.md)
  * Environment Variables and Secrets
    * [Adding environment variables](deployment/advance_examples/adding-env-vars.md)
    * [Using secrets in your application](deployment/advance_examples/secret-env-vars.md)
  * [Adding GPUs to your service](deployment/advance_examples/adding-gpus.md)
  * [Monitoring your services](./deployment/monitoring.md)
  * [Cost Estimation](./deployment/costing/cost-estimation.md)
  * [Collaboration with team](deployment/collab.md)
  * [CI/CD](./deployment/advance_examples/ci-pipeline-integration.md)
  * [servicefoundry.yaml Reference](deployment/servicefoundry.yaml.md)
* [Getting Help](getting-help.md)
* [Deploying on your own cloud](deploy-on-own-cloud/getting-started.md)
* Coming Soon
  * Deploy Model
    * Deploy your existing project with Truefoundry
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
    * Deploy on Truefoundry and Share
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
  * [sfy create](deployment/reference/cli/sfy-create/README.md)
    * [sfy create workspace](deployment/reference/cli/sfy-create/sfy-create-workspace.md)
  * [sfy delete](deployment/reference/cli/sfy-delete/README.md)
    * [sfy delete service](deployment/reference/cli/sfy-delete/sfy-delete-service.md)
    * [sfy delete workspace](deployment/reference/cli/sfy-delete/sfy-delete-workspace.md)
  * [sfy deploy](deployment/reference/cli/sfy-deploy/README.md)
    * [sfy deploy function](deployment/reference/cli/sfy-deploy/sfy-deploy-function.md)
    * [sfy deploy webapp](deployment/reference/cli/sfy-deploy/sfy-deploy-webapp.md)
  * [sfy get](deployment/reference/cli/sfy-get/README.md)
    * [sfy get config](deployment/reference/cli/sfy-get/sfy-get-config.md)
    * [sfy get deployment](deployment/reference/cli/sfy-get/sfy-get-deployment.md)
    * [sfy get service](deployment/reference/cli/sfy-get/sfy-get-service.md)
    * [sfy get workspace](deployment/reference/cli/sfy-get/sfy-get-workspace.md)
  * [sfy list](deployment/reference/cli/sfy-list/README.md)
    * [sfy list deployment](deployment/reference/cli/sfy-list/sfy-list-deployment.md)
    * [sfy list service](deployment/reference/cli/sfy-list/sfy-list-service.md)
    * [sfy list workspace](deployment/reference/cli/sfy-list/sfy-list-workspace.md)
  * [sfy use](deployment/reference/cli/sfy-use/README.md)
    * [sfy use cluster](deployment/reference/cli/sfy-use/sfy-use-cluster.md)
    * [sfy use server](deployment/reference/cli/sfy-use/sfy-use-server.md)
    * [sfy use workspace](deployment/reference/cli/sfy-use/sfy-use-workspace.md)
  * [sfy build](deployment/reference/cli/sfy-build.md)
  * [sfy init](deployment/reference/cli/sfy-init.md)
  * [sfy login](deployment/reference/cli/sfy-login.md)
  * [sfy logout](deployment/reference/cli/sfy-logout.md)
