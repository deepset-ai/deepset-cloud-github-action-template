# CI/CD Template with GitHub Actions for deepset Cloud

This repository provides a sample template for implementing CI/CD using GitHub Actions with deepset Cloud REST APIs. It demonstrates working with two separate workspaces: **dev** and **prod**.

The template illustrates best practices and offers a starting point for setting up automated pipeline management and deployment in deepset Cloud environments. While it is fully functional, you may need to customize it to fit your specific use case and requirements.

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
- A **GitHub** account with permissions to create repositories and configure GitHub Actions.
- A basic understanding of **Git** and GitHub workflows.

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

1. Clone the repository locally:

   ```bash
   git clone https://github.com/deepset-ai/deepset-cloud-github-action-template
   cd your-repo
   ```

2. Set up GitHub Secrets:

   1. Navigate to your GitHub repository.
   2. Go to **Settings** > **Secrets and variables** > **Actions**.
   3. Click **"New repository secret"** and add the following secret: `DEEPSET_CLOUD_API_KEY`: Your deepset Cloud API key.

     **Note**: The same API key is used for dev and prod environments.

3. Configure workspace names. Update the workspace names in the workflow files:

     - In `.github/workflows/deploy-dev.yml`, replace `"YOUR_DEV_WORKSPACE_NAME"` with your actual **dev** workspace name.
     - In `.github/workflows/deploy-prod.yml`, replace `"YOUR_PROD_WORKSPACE_NAME"` with your actual **prod** workspace name.

## Usage

### Branching Strategy

- **Development Branch (`dev`)**: Used for integrating and testing pipeline changes. Automatically deploys to the **dev** workspace on push.
- **Production Branch (`main`)**: Contains stable and reviewed pipeline YAMLs. Deployment to the **prod** workspace is triggered on push or manually through GitHub Actions.

### Adding or Updating Pipelines

1. Add pipeline files:
  
   1. For each pipeline, create a new directory under `pipelines/`.
   2. Each pipeline directory should contain two files: `indexing.yaml` (with the indexing pipeline) and `query.yaml` (with the query pipeline).

4. After you updated your pipeline, commit the changes:
   ```bash
   git add pipelines/your-pipeline-name/
   git commit -m "Add/Update pipeline: your-pipeline-name"
   ```

5. Push your changes to the appropriate branch:
   - For development: `git push origin dev`
   - For production: `git push origin main`

### Triggering Deployments

- **Automatic Deployment**:
  - Dev Workspace: Pushing to the `dev` branch triggers the `deploy-dev.yml` workflow.
  - Prod Workspace: Pushing to the `main` branch triggers the `deploy-prod.yml` workflow.

- **Manual Deployment**:
  For production, you can manually trigger the workflow:
    1. Go to the **Actions** tab in your GitHub repository.
    2. Select **"Deploy to Prod Workspace"** workflow.
    3. Click **"Run workflow"**.

### Rollback Procedures

To revert to a previous pipeline version, revert your changes in Git:
   ```bash
   git revert <commit-hash>
   git push origin dev  # or main, depending on the branch
   ```

The GitHub Actions workflow automatically redeploys the pipelines based on the reverted code.


## Extending the Template

- To add more environments, copy and update existing workflows for additional environments, like staging.
- To enable deployment notifications, add steps to send them using email, Slack, and the like.


