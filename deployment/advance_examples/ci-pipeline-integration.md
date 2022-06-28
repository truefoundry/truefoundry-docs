# Integrate servicefoundry in a CI/CD pipeline

## Introduction
Servicefoundry can be seamlessly integrated with any CI/CD pipeline solution like Github Actions, Jenkins etc. An example leveraging github actions is provided below.

## Before you start
Make sure the following steps have been completed before moving ahead:
- [Install servicefoundry](https://docs.truefoundry.com/servicefoundry/quick-start#install-servicefoundry-client-library)
- [Create an application and deploy it on servicefoundry](https://docs.truefoundry.com/servicefoundry/quick-start-templates)
- [Push the codebase to github](https://docs.github.com/en/get-started/quickstart/hello-world)

## Steps
- Get the api key for your user from [here](https://app.devtest.truefoundry.tech/settings). Create one if one has not been provisioned yet.
- Add the api key to the github repo as a secret with key `SERVICE_FOUNDRY_API_KEY`. This will be used by servicefoundry cli to authenticate to the server.
- Inside the codebase directory, create a folder for github actions
  ```bash
  mkdir -p .github/workflows
  ```
- Create a file called `sfy-deploy.yaml` in this directory and paste the below content -
  ```bash
  name: sfy deploy
  on:
    push:
      branches:
      - main
  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
      - run: echo "Deploying ${{ github.ref }} to servicefoundry"
      - name: Install sfy
        run: pip install servicefoundry
      - name: Check out repository code
        uses: actions/checkout@v3
      - name: Deploy
        run: sfy deploy
        env:
          SERVICE_FOUNDRY_API_KEY: ${{ secrets.SERVICE_FOUNDRY_API_KEY }}
  ```
- Commit this file to `main`. This will trigger the pipeline and deploy the code on `main` to servicefoundry
