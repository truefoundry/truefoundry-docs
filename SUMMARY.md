# Table of contents

* [Introduction](introduction.md)
<!--* [Quick Start](quick-start.md)-->
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

* Deploy
  * [Overview](deployment/overview.md)
  * Quick Start
    * [Installation and setup](deployment/quickstart/install-and-workspace.md)
    * [Train and deploy model](deployment/quickstart/train-and-deploy-model.md)
    <!--* [More examples](deployment/quickstart/more-examples.md) -->
  * Concepts
    * [Cluster and Workspace](deployment/concepts/workspace.md)
    * [Build](deployment/concepts/build.md)
    <!-- * [Command](deployment/concepts/command.md) -->
    * [Environment Variables](deployment/concepts/env-variables.md)
    * [Secrets](deployment/concepts/secrets.md)
    * [Resources](deployment/concepts/resources.md)
    * [Ignoring files - `.sfyignore`](deployment/concepts/syfignore.md)
  * Deploy a job
    * [Job and usecases](deployment/job/definition.md)
    * [Deploy a Job](deployment/job/deploy.md)
    * [Deploy a CronJob](deployment/job/cron-job.md)
    * [Deploy a Job from Dockerfile / Image](deployment/job/docker.md)
    * [Advanced Options](deployment/job/advanced.md)
    * [Monitor Jobs](deployment/job/monitoring.md)
  * Deploy a service
    * [Service and usecases](deployment/service/definition.md)
    * [Deploy a Service](deployment/service/deploy.md)
    * [Deploy FastAPI Service](deployment/service/fastapi.md)
    * [Deploy Streamlit Service](deployment/service/streamlit.md)
    * [Deploy Gradio Service](deployment/service/gradio.md)
    * [Deploy Dockerized Service](deployment/service/docker.md)
    * [Monitor a Service](deployment/service/monitoring.md)
    * [Advanced Options](deployment/service/advanced.md)
  * Deploy function and class
    * [Deploy function](deployment/function/function_deploy.md)
    * [Deploy instance of a class](deployment/function/class_deploy.md)
    * [Advanced Options](deployment/function/advanced.md)
  * [Servicefoundry CLI](deployment/reference/cli/README.md)
    * [sfy list](deployment/reference/cli/sfy-list/README.md)
      * [sfy list deployment](deployment/reference/cli/sfy-list/sfy-list-deployment.md)
    * [sfy use](deployment/reference/cli/sfy-use/README.md)
      * [sfy use server](deployment/reference/cli/sfy-use/sfy-use-server.md)
    * [sfy build](deployment/reference/cli/sfy-build.md)
    * [sfy deploy](deployment/reference/cli/sfy-deploy.md)
    * [sfy login](deployment/reference/cli/sfy-login.md)
    * [sfy logout](deployment/reference/cli/sfy-logout.md)
    * [sfy logs](deployment/reference/cli/sfy-logs.md)
    <!-- * [Monitoring your services](./deployment/monitoring.md) -->
    <!-- * [Cost Estimation](./deployment/costing/cost-estimation.md) -->
    <!-- * [Collaboration with team](deployment/collab.md) -->
    <!-- * [CI/CD](./deployment/advance_examples/ci-pipeline-integration.md) -->

* Deploying on your own cloud
  * [Overview](deploy-on-own-cloud/overview.md)
  * [Requirements](deploy-on-own-cloud/requirements.md)
  * [Local Installation](deploy-on-own-cloud/local-installation.md)
  * [Production Installation](deploy-on-own-cloud/production-installation.md)
  * [Bootstrap Servicefoundry](deploy-on-own-cloud/servicefoundry-bootstrap.md)
  * [Client Setup](deploy-on-own-cloud/client-setup.md)
  * [Adding Users](deploy-on-own-cloud/add-users.md)
  * [Adding Git Repositories](deploy-on-own-cloud/add-version-control-integration.md)
* Monitor your Models
  * [Quick Start](monitoring/introduction.md)
  * [Concepts](monitoring/concepts.md)
  * [Defining the schema](monitoring/schema.md)
  * [Logging Predictions and Actuals](monitoring/data-logging.md)
  * [Visualizing the Logged Data](monitoring/data-visualizing.md)
  * [Fetching Inference Data](monitoring/get-inference-data.md)
  * [Alerts and Notifications (Coming Soon)]
* [Getting Help](getting-help.md)
* [Roadmap](roadmap.md)
<!-- * [Truefoundry vs ...](products.md) -->

