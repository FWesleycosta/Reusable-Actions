# Reusable GitHub Actions Workflows

[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)
[![Terraform](https://img.shields.io/badge/Terraform-v1.7.5+-623CE4?logo=terraform)](https://img.shields.io/badge/Terraform-v1.0.0+-623CE4?logo=terraform)
[![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-v2+-2088FF?logo=github)](https://img.shields.io/badge/GitHub%20Actions-v2+-2088FF?logo=github)
[![Trivy](https://img.shields.io/badge/Trivy-v0.19.2+-35495E?logo=trivy)](https://img.shields.io/badge/Trivy-v0.19.2+-35495E?logo=trivy)

This repository contains a collection of reusable GitHub Actions workflows for automating CI/CD processes with Docker, ECR, Terraform, and security scanning with Trivy.

## Workflows

### Build and Push

File: `build-push-scan.yml`

This workflow is responsible for building, pushing, and scanning a Docker image. It checks if the ECR repository exists, logs in, builds the image, pushes it to ECR, and saves the image name as an artifact.

### ECR Repository Management

File: `create-repository-elastic-container-registry.yml`

This workflow manages ECR repositories using Terraform. It initializes Terraform, configures AWS credentials, plans, and applies changes to the ECR repository.

### Terraform Deploy

File: `terraform-deploy.yml`

This workflow is used for deploying infrastructure using Terraform. It downloads the Docker image name, configures AWS credentials, initializes and applies the Terraform plan, and sends notifications to Slack.

### Trivy Scan

File: `trivy.yml`

This workflow scans Docker images for vulnerabilities using Trivy. It downloads the Docker image name, configures AWS credentials, logs into ECR, and runs the scan with Trivy.

### SonarQube Scan

File: `sonarqube.yml`

This workflow scans code quality using SonarQube. It configures the SonarQube scanner, runs the scan, and sends the results to SonarQube.

## Usage

To use the workflows, add the following code to your main workflow:

```yaml
jobs:
  build-push-scan:
    uses: FWesleycosta/Reusable-Actions/.github/workflows/build-push-scan.yml@v1.0.0
    secrets:
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_REGION: ${{ secrets.AWS_REGION }}
      ECR_REPOSITORY: ${{ secrets.ECR_REPOSITORY }}

  create-repository-elastic-container-registry:
    uses: FWesleycosta/Reusable-Actions/.github/workflows/ecr-repository-management.yml@v1.0.0
    secrets:
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_REGION: ${{ secrets.AWS_REGION }}
      ECR_REPOSITORY: ${{ secrets.ECR_REPOSITORY }}
      WORKING_DIRECTORY: ${{ secrets.WORKING_DIRECTORY }}
      DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
      TF_BACKEND_KEY: ${{ secrets.TF_BACKEND_KEY }}
      TF_BACKEND_REGION: ${{ secrets.TF_BACKEND_REGION }}
      TF_BACKEND_BUCKET: ${{ secrets.TF_BACKEND_BUCKET }}

  terraform-deploy:
    uses: FWesleycosta/Reusable-Actions/.github/workflows/terraform-deploy.yml@v1.0.0
    with:
      environment: 'Development' <- Change to 'Production' for production
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID_PRD }}
      AWS_REGION: ${{ secrets.AWS_REGION }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY_PRD }}
      ECR_REPOSITORY: ${{ secrets.ECR_REPOSITORY }}
      DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
      WORKING_DIRECTORY: ${{ secrets.WORKING_DIRECTORY }}
      TF_BACKEND_KEY: ${{ secrets.TF_BACKEND_KEY }}
      TF_BACKEND_REGION: ${{ secrets.TF_BACKEND_REGION }}
      TF_BACKEND_BUCKET: ${{ secrets.PROD_TF_BACKEND_BUCKET }}
      SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK_TERRAFORM }}

  trivy-scan:
    uses: FWesleycosta/Reusable-Actions/.github/workflows/trivy.yml@v1.0.0
    secrets:
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_REGION: ${{ secrets.AWS_REGION }}

  sonarqube-scan:
    uses: FWesleycosta/Reusable-Actions/.github/workflows/sonarqube.yml@v1.0.0
    secrets:
      sonarqube_token: ${{ secrets.SONARQUBE_TOKEN }}
      sonarqube_url: ${{ secrets.SONARQUBE_URL }}
      sonarqube_project_key: ${{ secrets.SONARQUBE_PROJECT_KEY }}

