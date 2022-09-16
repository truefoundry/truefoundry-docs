# Roadmap

## Model Deployment using Model Servers

Model Deployment using FastAPI is great, but when we are looking to scale, it needs a lot of optimizations and 
can soon become a bottleneck. Then we need to use some of the optimized model frameworks that can give us a much higher 
throughput. There are optimized model inference servers like **TFServe, Triton, Seldon ML Server**. 

Our goal here is to make it very easy to try out different model servers and then benchmark for our own use-cases. 
We are also striving to make it much easier to try out the inference servers without the great learning curve that comes
with each framework. 

## Model Evaluation on Live Traffic

Its still quite hard to roll out models in canary or shadow mode. We want to enable datascientists and ML Engineers to pass a small percentage of traffic through the new model, evaluate if its working similar or better to the currently deployed model and then roll out the new model if things look good.

## Integrations 

## Pipelines Deployment

## Deploy on Managed Infra

## Cost Estimation and Optimization
  
  
