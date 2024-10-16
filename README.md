# CI/CD Template with GitHub Actions for deepset Cloud

This repository serves as a sample template example to showcase how you can implement CI/CD with GitHub Actions by leveraging the deepset Cloud REST APIs. It demonstrates how to work with two separate workspaces: **dev** and **prod**.

The purpose of this template is to illustrate best practices and provide a starting point for setting up automated pipeline management and deployment in deepset Cloud environments. While it's designed to be functional, you may need to adapt and extend it to fit your specific use case and requirements.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Repository Structure](#repository-structure)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
  - [Branching Strategy](#branching-strategy)
  - [Adding or Updating Pipelines](#adding-or-updating-pipelines)
  - [Triggering Deployments](#triggering-deployments)
  - [Rollback Procedures](#rollback-procedures)
- [Extending the Template](#extending-the-template)

---

## Prerequisites

Before using this template, ensure you have the following:

- A **deepset Cloud** account with access to your **dev** and **prod** workspaces.
- A **GitHub** account with permissions to create repositories and set up GitHub Actions.
- Basic understanding of **Git** and GitHub workflows.

## Repository Structure

```plaintext
your-repo/
├── .github/
│   └── workflows/
│       ├── deploy-dev.yml
│       └── deploy-prod.yml
├── pipelines/
│   ├── pipeline1/
│   │   ├── indexing.yaml
│   │   └── query.yaml
│   └── pipeline2/
│       ├── indexing.yaml
│       └── query.yaml
└── README.md
```

- **`.github/workflows/`**: Contains GitHub Actions workflow files for CI/CD.
- **`pipelines/`**: Stores your pipeline configuration files.
- **`README.md`**: Provides instructions and information about the repository.

## Setup Instructions

1. **Clone the Repository locally**:

   ```bash
   git clone https://github.com/deepset-ai/deepset-cloud-github-action-template
   cd your-repo
   ```

2. **Set Up GitHub Secrets**:

   - Navigate to your GitHub repository.
   - Go to **Settings** > **Secrets and variables** > **Actions**.
   - Click **"New repository secret"** and add the following secret:

     - `DEEPSET_CLOUD_API_KEY`: Your deepset Cloud API key.

     **Note**: The same API key is used for both dev and prod environments.

3. **Configure Workspace Names**:

   - Update the workspace names in the workflow files:

     - In `.github/workflows/deploy-dev.yml`, replace `"YOUR_DEV_WORKSPACE_NAME"` with your actual **dev** workspace name.
     - In `.github/workflows/deploy-prod.yml`, replace `"YOUR_PROD_WORKSPACE_NAME"` with your actual **prod** workspace name.

## Usage

### Branching Strategy

- **Development Branch (`dev`)**: Used for integrating and testing new changes. Automatically deploys to the **dev** workspace upon push.
- **Production Branch (`main`)**: Contains stable and reviewed code. Deployment to the **prod** workspace is triggered on push or manually via GitHub Actions.

### Adding or Updating Pipelines

1. **Add or Modify Pipeline Files**:
   - Create a new directory under `pipelines/` for each pipeline.
   - Each pipeline directory should contain two files: `indexing.yaml` and `query.yaml`.

2. **Commit Changes**:
   ```bash
   git add pipelines/your-pipeline-name/
   git commit -m "Add/Update pipeline: your-pipeline-name"
   ```

3. **Push to the Appropriate Branch**:
   - For development: `git push origin dev`
   - For production: `git push origin main`

### Triggering Deployments

- **Automatic Deployment**:
  - **Dev Workspace**: Pushing to the `dev` branch triggers the `deploy-dev.yml` workflow.
  - **Prod Workspace**: Pushing to the `main` branch triggers the `deploy-prod.yml` workflow.

- **Manual Deployment**:
  - For production, you can manually trigger the workflow:
    1. Go to the **Actions** tab in your GitHub repository.
    2. Select **"Deploy to Prod Workspace"** workflow.
    3. Click **"Run workflow"**.

### Rollback Procedures

To revert to a previous pipeline version:

1. **Revert Changes in Git**:
   ```bash
   git revert <commit-hash>
   git push origin dev  # or main, depending on the branch
   ```

2. The GitHub Actions workflow will automatically redeploy the pipelines based on the reverted code.


## Extending the Template

- **Add More Environments**: Copy and modify existing workflows for additional environments like staging.
- **Notifications**: Add steps to send deployment notifications via email, Slack, etc.

