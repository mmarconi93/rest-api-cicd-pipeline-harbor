# RESTful API CI/CD Pipelines

This document describes how to setup and use our Continuous Integration (CI) and Continuous Deployment (CD) pipelines for a RESTful API using GitHub Actions.

## Prerequisites

To get started, you will need:

1. A GitHub repository with your RESTful API code.
2. A Dockerfile in your repository root to build your RESTful API as a Docker image.
3. Access to a [Harbor](https://goharbor.io/) Docker registry.
4. A deployment server to which you have SSH access and Docker installed.

## Set Up Secrets

For security reasons, we use GitHub Secrets to store sensitive information. You will need to set up the following secrets in your GitHub repository:

1. `HARBOR_REGISTRY_URL`: The URL of your Harbor registry.
2. `HARBOR_USERNAME`: Your username for the Harbor registry.
3. `HARBOR_PASSWORD`: Your password for the Harbor registry.
4. `DEPLOY_SSH_KEY`: A private SSH key that has access to your deployment server. The corresponding public key must be added to the `authorized_keys` file on the server.
5. `DEPLOY_USER`: The username to use when connecting to the deployment server via SSH.
6. `DEPLOY_HOST`: The hostname or IP address of your deployment server.

To add a secret:

1. Go to your GitHub repository and click on the `Settings` tab.
2. Click on `Secrets` in the left sidebar.
3. Click on `New repository secret`.
4. Enter the name of the secret and its value, then click `Add secret`.

## CI Pipeline

Our CI pipeline is defined in the `.github/workflows/ci.yml` file in the root of your repository. This pipeline will automatically build a Docker image of your RESTful API and push it to your Harbor registry whenever changes are pushed to the `main` branch of your repository.

You will need to replace `REPO_NAME` and `IMAGE_NAME` in the file with the name of your Harbor repository and Docker image respectively.

## CD Pipeline

Our CD pipeline is defined in the `.github/workflows/cd.yml` file in the root of your repository. This pipeline will automatically pull the latest Docker image of your RESTful API from your Harbor registry and deploy it to your server whenever the CI pipeline has completed successfully.

You will need to replace `REPO_NAME` and `IMAGE_NAME` in the file with the name of your Harbor repository and Docker image respectively.

The pipeline assumes that your RESTful API will be accessible on port 80 on your server. If this is not the case, adjust the `-p` flag in the `docker run` command accordingly.

## Conclusion

With the provided setup, you should have a fully working CI/CD pipeline for your RESTful API. Whenever you push changes to the `main` branch of your repository, those changes will automatically be deployed to your server. Make sure to keep your GitHub Secrets updated if you change your Harbor credentials or deployment server.

