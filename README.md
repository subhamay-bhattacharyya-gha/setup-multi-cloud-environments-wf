# Setup Multi Cloud Environments – GitHub Reusable Workflow

![Release](https://github.com/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf/actions/workflows/release.yaml/badge.svg)&nbsp;![Commit Activity](https://img.shields.io/github/commit-activity/t/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![Last Commit](https://img.shields.io/github/last-commit/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![Release Date](https://img.shields.io/github/release-date/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![Repo Size](https://img.shields.io/github/repo-size/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![File Count](https://img.shields.io/github/directory-file-count/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![Open Issues](https://img.shields.io/github/issues/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![Top Language](https://img.shields.io/github/languages/top/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![Monthly Commit Activity](https://img.shields.io/github/commit-activity/m/subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf)&nbsp;![Custom Endpoint](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/bsubhamay/3fbd103e24a78453bdf7f8c03cc33a7c/raw/setup-multi-cloud-environments-wf.json?)

## 📝 About the Reusable Workflow

This reusable GitHub Action sets up multi-cloud environments (AWS, GCP, and Azure) by creating GitHub environments with required variables for OIDC authentication and Terraform state management.

## 🛠️ Usage

Call this workflow from another workflow using `workflow_call`.

---

## 🚀 Example: Using This Reusable Workflow

```yaml
name: Setup Multi-Cloud Environments
run-name: Setup Multi-Cloud Environments by ${{ github.actor }}

on:
  workflow_dispatch:
    inputs:
      ci-environment:
        description: "CI environment name"
        required: true
        default: "ci"
      aws-region:
        description: "AWS region where services will be deployed"
        required: true
        default: "us-east-1"
      gcp-region:
        description: "GCP region where services will be deployed"
        required: true
        default: "us-central1"
      azure-region:
        description: "Azure region where services will be deployed"
        required: true
        default: "eastus"

jobs:
  setup:
    name: Setup Multi-Cloud Environments
    uses: subhamay-bhattacharyya-gha/setup-multi-cloud-environments-wf/.github/workflows/setup-environments.yaml@main
    with:
      ci-environment: ${{ github.event.inputs.ci-environment }}
      aws-region: ${{ github.event.inputs.aws-region }}
      gcp-region: ${{ github.event.inputs.gcp-region }}
      azure-region: ${{ github.event.inputs.azure-region }}
    secrets:
      GH_PAT: ${{ secrets.GH_PAT }}
```

## Inputs

| Name              | Description                                        | Required |
|-------------------|----------------------------------------------------|----------|
| `ci-environment`  | CI environment name                                | ✅       |
| `aws-region`      | AWS region where services will be deployed         | ✅       |
| `gcp-region`      | GCP region where services will be deployed         | ✅       |
| `azure-region`    | Azure region where services will be deployed       | ✅       |

## Secrets

| Name     | Description                                      | Required |
|----------|--------------------------------------------------|----------|
| `GH_PAT` | GitHub Personal Access Token with `repo` `workflow` and `admin:repo_hook` scopes   | ✅       |

## Permissions Required

This workflow requires the following permissions:

```yaml
permissions:
  contents: write
  id-token: write
  actions: write
  deployments: write
```

## What It Does

The workflow creates the following GitHub environments:

1. **CI Environment**: `<ci-environment>`
2. **AWS Environment**: `<ORG_SHORT_NAME>-aws-<aws-region>`
3. **GCP Environment**: `<ORG_SHORT_NAME>-gcp-<gcp-region>`
4. **Azure Environment**: `<ORG_SHORT_NAME>-azure-<azure-region>`

### Variables Created Per Environment

**AWS Environment** (`<ORG_SHORT_NAME>-aws-<aws-region>`):
- `AWS_REGION` - AWS region
- `AWS_ROLE_ARN` - IAM role ARN for OIDC authentication
- `S3_KMS_KEY_ALIAS` - KMS key alias for S3 encryption
- `AWS_TF_STATE_BUCKET_NAME` - S3 bucket name for Terraform state

**GCP Environment** (`<ORG_SHORT_NAME>-gcp-<gcp-region>`):
- `GCP_REGION` - GCP region
- `GCP_ROLE_ARN` - Role ARN for authentication
- `GCP_KMS_KEY_ALIAS` - KMS key alias for encryption
- `GCP_TF_STATE_BUCKET_NAME` - Bucket name for Terraform state

**Azure Environment** (`<ORG_SHORT_NAME>-azure-<azure-region>`):
- `AZURE_REGION` - Azure region
- `AZURE_ROLE_ARN` - Role ARN for authentication
- `AZURE_KMS_KEY_ALIAS` - KMS key alias for encryption
- `AZURE_TF_STATE_BUCKET_NAME` - Bucket name for Terraform state

## Requirements

Make sure your repository has the following variables defined:

### Required Repository Variables

- `ORG_SHORT_NAME`: Organization short name used to construct environment names

### AWS Variables

- `AWS_ACCOUNTS`: JSON object with AWS Account ID. Example:

  ```json
  {
    "aws": "111122223333"
  }
  ```

- `AWS_OIDC_ROLE`: Name of the IAM role to assume via OIDC
- `S3_KMS_KEY_ALIAS`: KMS key alias for S3 encryption
- `AWS_TF_STATE_BUCKET_NAME`: S3 bucket name for Terraform state

### GCP Variables

- `GCP_PROJECTS`: JSON object with GCP Project ID. Example:

  ```json
  {
    "gcp": "my-prod-project-123456"
  }
  ```

- `GCP_ROLE_ARN`: Role ARN for GCP authentication
- `GCP_KMS_KEY_ALIAS`: KMS key alias for encryption
- `GCP_TF_STATE_BUCKET_NAME`: Bucket name for Terraform state

### Azure Variables

- `AZURE_SUBSCRIPTIONS`: JSON object with Azure Subscription ID. Example:

  ```json
  {
    "azure": "12345678-1234-1234-1234-123456789012"
  }
  ```

- `AZURE_ROLE_ARN`: Role ARN for Azure authentication
- `AZURE_KMS_KEY_ALIAS`: KMS key alias for encryption
- `AZURE_TF_STATE_BUCKET_NAME`: Bucket name for Terraform state

---

## License

 MIT
