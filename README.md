# Introduction

This repository contains sample Azure DevOps pipelines that support the "GitOps" flow for Azure infrastructure as well as sample ARM templates to illustrate how this flow could work. The pipelines are designed to be used in together with [azure-gitops](https://github.com/travisnielsen/azure-gitops). The purpose of these pipelines is to highlight an automated, test-driven approach to Azure infrastructure lifecycle management while maintaining enterprise security standards via a test driven (aka "shift left") approach.

## Getting Started

### Pipeline and Azure DevOps Configuration

Please see [this document](.azuredevops/pipeline.md) for configuring Azure DevOps and the pipeline.

### Create a branch

In a GitOps model, all infrastructure changes must be made in a feature branch. Create a branch using the following command:

```bash
git checkout -b <feature-name>
```

### Configure and submit deployment

### Create a Pull Request (PR)

When you are ready, commit and push all changes to your feature branch.

```bash
git push
```

Finally, submit a pull request for the Deployment branch in Azure DevOps. This will initiate the deployment process.
