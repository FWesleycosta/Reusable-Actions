# Reusable-Terraform : Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2024-08-22

### Added
- Added `build-push-scan.yml` workflow for building, pushing, and scanning Docker images.
- Added `ecr-repository-management.yml` workflow for managing ECR repositories using Terraform.
- Added `terraform-deploy.yml` workflow for deploying lambda function using Terraform.
- Added `trivy.yml` workflow for scanning Docker images for vulnerabilities using Trivy.
- Added `sonarqube.yml` workflow for scanning code quality using SonarQube.
- Added `create-pull-request.yml` workflow for creating a pull request using the GitHub API.

### Changed
- None

### Removed
- None

