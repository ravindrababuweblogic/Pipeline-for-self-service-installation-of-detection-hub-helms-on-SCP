# Detection Hub – Self-Service Helm Installation Pipeline (SCP)

This repository contains the Azure DevOps pipeline package for self-service installation of Detection Hub Helm charts on SCP (Sovereign Cloud Platform) AKS clusters.

## Repository Structure

```
.
├── azure-pipelines.yml          # Main Azure DevOps pipeline (multi-stage deploy with Teams notifications)
├── templates/
│   ├── parameters.yml           # Pipeline input parameters definition
│   ├── variables.yml            # Shared pipeline variables
│   ├── prechecks.yml            # Pre-deployment Kubernetes validation checks
│   ├── helm-deploy.yml          # Helm upgrade/install step
│   ├── postchecks.yml           # Post-deployment verification checks
│   └── teams-notification.yml  # Microsoft Teams notification step
└── env/
    ├── dev.yml                  # Dev environment configuration
    ├── test.yml                 # Test environment configuration
    ├── stage.yml                # Staging environment configuration
    └── prod.yml                 # Production environment configuration
```

## Quick Start

1. Import this repository into Azure DevOps.
2. Create a pipeline using `azure-pipelines.yml` as the pipeline definition file.
3. Set up the required Azure DevOps service connection and variable group per environment:
   - `AZ_SUB_CONN` – Azure subscription service connection name
   - `AKS_NAME` – AKS cluster name
   - `AKS_RG` – AKS resource group
   - `ACR_NAME` – Azure Container Registry name (e.g., `symphonyfs`)
   - `dethub-teams-webhook` – Microsoft Teams incoming webhook URL (stored as a secret)
4. Run the pipeline, selecting:
   - **environment** – `dev` / `test` / `stage` / `prod`
   - **namespace** – Kubernetes namespace (e.g., `dev-dethub`)
   - **releaseName** – Helm release name (e.g., `dethub`)
   - **chartVersion** – Helm chart version (e.g., `0.1.0`)

## Pipeline Stages

| Stage  | Description |
|--------|-------------|
| Plan   | Resolves and publishes input parameters as an artifact |
| Deploy | Logs in to Azure + AKS, runs pre-checks, executes `helm upgrade --install`, runs post-checks, and sends a Teams notification |

## Helm Command Equivalent

```bash
helm upgrade --install -n <namespace> <releaseName> symphonyfs/detectionhub --version <chartVersion>
```

Example used in `aks-scp-dev-uks-st-scratch`:
```bash
helm upgrade --install -n dev-dethub dethub symphonyfs/detectionhub --version 0.1.0
```

## Requirements

- Azure DevOps with YAML pipelines enabled
- AKS cluster with Helm 3.12+ and `kubectl` access
- Azure Container Registry (`symphonyfs`) hosting the `detectionhub` Helm chart
- Microsoft Teams incoming webhook for deployment notifications

## Source

This package was copied from the `extra_copy/Latest` folder of the
[Pipeline-for-self-service-installation-of-detection-hub-helms-on-SCP](https://github.com/ravindrababuweblogic/Pipeline-for-self-service-installation-of-detection-hub-helms-on-SCP)
repository.
