# Pipeline-for-self-service-installation-of-detection-hub-helms-on-SCP
Detailed Description of task required: Create new pipeline for self-service installation of detectionhub helms. Command used for deployment in aks-scp-dev-uks-st-scratch cluster was: helm upgrade --install -n dev-dethub dethub symphonyfs/detectionhub --version 0.1.0  Required to support customer: Internal  


# Detection Hub – Self‑Service Pipeline Package

This package contains:

- `azure-pipelines-dethub.yml` – Azure DevOps YAML for multi-stage deploy with Teams notifications.
- `DetectionHub_SelfService_Pipeline_Documentation.docx` – Confluence-ready documentation.
- `architecture_diagram.png` – High-level architecture diagram.
- `runbook_detectionhub.txt` – On-call / operational runbook.
- `env_matrix.csv` – Environment and configuration placeholders.

## Quick Start
1. Create/update Azure DevOps service connection and set variables in a variable group per environment.
2. Provide AKS Resource Group, Key Vault name, Teams Webhook URL (secret: `dethub-teams-webhook`).
3. Confirm Helm chart path (`symphonyfs/detectionhub`) and ACR name (`symphonyfs`).
4. Run pipeline with defaults: env=dev, namespace=dev-dethub, releaseName=dethub, chartVersion=0.1.0.
