# CI/CD Template with GitHub Actions for deepset Cloud

This repository serves as a template for using **GitHub Actions** as a CI/CD pipeline to manage and deploy pipelines to **deepset Cloud**. It demonstrates how to work with two separate workspaces: **dev** and **prod**.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Repository Structure](#repository-structure)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
  - [Branching Strategy](#branching-strategy)
  - [Adding or Updating Pipelines](#adding-or-updating-pipelines)
  - [Triggering Deployments](#triggering-deployments)
  - [Rollback Procedures](#rollback-procedures)
- [Security Considerations](#security-considerations)
- [Extending the Template](#extending-the-template)
- [Support and Contributions](#support-and-contributions)

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
│   ├── pipeline1.yaml
│   └── pipeline2.yaml
└── README.md
```

- **`.github/workflows/`**: Contains GitHub Actions workflow files for CI/CD.
- **`pipelines/`**: Stores your pipeline configuration files.
- **`README.md`**: Provides instructions and information about the repository.

## Setup Instructions

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/your-username/your-repo.git
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

     - In `.github/workflows/deploy-dev.yml`, replace `"your-dev-workspace-name"` with your actual **dev** workspace name.
     - In `.github/workflows/deploy-prod.yml`, replace `"your-prod-workspace-name"` with your actual **prod** workspace name.

4. **Install Dependencies (Optional)**:

   - If you plan to run the deepset Cloud CLI locally, install it with:

     ```bash
     pip install deepset-cloud
     ```

## Usage

### Branching Strategy

This template uses a simple branching strategy to manage deployments to different environments:

- **Development Branch (`dev`)**:

  - Used for integrating and testing new changes.
  - Automatically deploys to the **dev** workspace upon push.

- **Production Branch (`main`)**:

  - Contains stable and reviewed code.
  - Deployment to the **prod** workspace is triggered on push or manually via GitHub Actions.

### Adding or Updating Pipelines

1. **Add or Modify Pipeline Files**:

   - Pipeline configuration files are located in the `pipelines/` directory.
   - Create new pipeline YAML files or modify existing ones.

   **Example**:

   ```yaml:pipelines/pipeline1.yaml
   version: "1.0"
   components:
     - name: Retriever
       type: BM25Retriever
       params:
         document_store: MyDocumentStore

     - name: Reader
       type: TransformersReader
       params:
         model_name_or_path: deepset/roberta-base-squad2

   pipelines:
     - name: question-answering
       nodes:
         - name: Retriever
           inputs: [Query]
         - name: Reader
           inputs: [Retriever]
   ```

2. **Commit Changes**:

   ```bash
   git add pipelines/your-pipeline.yaml
   git commit -m "Add new pipeline"
   ```

3. **Push to the Appropriate Branch**:

   - For development:

     ```bash
     git push origin dev
     ```

   - For production:

     ```bash
     git push origin main
     ```

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

If you need to revert to a previous pipeline version:

1. **Revert Changes in Git**:

   ```bash
   git revert <commit-hash>
   git push origin dev  # or main, depending on the branch
   ```

2. **Deployment**:

   - The GitHub Actions workflow will redeploy the pipelines based on the reverted code.

## Security Considerations

- **API Keys**:

  - Store sensitive information like API keys in **GitHub Secrets**.
  - **Do not** commit sensitive data to the repository.

- **Workspace Names**:

  - Workspace names are specified in the workflow files. Ensure they are correct to avoid deploying to unintended workspaces.

- **Secret Rotation**:

  - Regularly rotate your API keys and update the GitHub Secrets accordingly.

## Extending the Template

- **Add More Environments**:

  - To add environments like staging, copy one of the existing workflows and modify the workspace name and trigger conditions.

- **Validation and Testing**:

  - Incorporate pipeline validation steps before deployment.

    ```yaml
    - name: Validate Pipelines
      run: |
        for pipeline in pipelines/*.yaml; do
          deepset-cloud pipelines validate --file "$pipeline"
        done
    ```

- **Notifications**:

  - Add steps to send deployment notifications via email, Slack, etc.

## Support and Contributions

- **Issues**: If you encounter problems, please open an issue.
- **Contributions**: Contributions are welcome! Feel free to fork the repository and submit a pull request.

---
