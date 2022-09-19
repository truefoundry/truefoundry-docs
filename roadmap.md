# Roadmap

## Model Deployment using Model Servers

Model Deployment using FastAPI is great, but when we are looking to scale, it needs a lot of optimizations and 
can soon become a bottleneck. Then we need to use some of the optimized model frameworks that can give us a much higher 
throughput. There are optimized model inference servers like **TFServe, Triton, Seldon ML Server**. 

Our goal here is to make it very easy to try out different model servers and then benchmark for our own use-cases. 
We are also striving to make it much easier to try out the inference servers without the great learning curve that comes
with each framework. 

## Complex Model Deployment Scenarios

Models can be deployed in an ensemble pattern or in a graph pattern. These can usually become very difficult to deploy, manage and maintain. We want to be able to make this process so easy so that we can deploy using just a few python functions. 

## Model Evaluation on Live Traffic

Its still quite hard to roll out models in canary or shadow mode. We want to enable datascientists and ML Engineers to pass a small percentage of traffic through the new model, evaluate if its working similar or better to the currently deployed model and then roll out the new model if things look good.

## Integrations 

We want to build integrations with Cloud Accounts (AWS, GCP and Azure) so that we can automated setting up a lot of the infrastructure needed for MLOps. This will also enable us to setup clusters automatically or deploy on managed services. 

We also want to integrate with CI/CD systems like CodeBuild, Jenkins, etc so that companies can use Truefoundry in their existing workflow. 

## Pipelines Deployment

Being able to run pipelines is an important part of ML workflow. We want to provide datascientists the ability to run a DAG of jobs by simply using Python. 

## Deploy on Cloud Managed Infra
Sometimes, it makes sense to deploy on managed infra provided by AWS, GCP or Azure like Lambda, or Cloud Run or even Sagemaker. In that case, instead of learning every 
deployment system, we want to be able to deploy the code to different systems with minimal changes in the code. This way the code remains portable and can be 
easily migrated from one deployment system to another. 

## Model Monitoring
Monitoring models in production is one of the key factors to deriving impact from ML in a sustainable way. We want to know when the models have drifted away, or their accuracy
has gone down, understand the reasons behind them and then resolve the issues by either fixing a bug or retraining the model. We want to make it very easy for data scientists
to log the model inference data and do advanced analysis on the logged data to figure out data, feature and prediction drifts. 
  
  
