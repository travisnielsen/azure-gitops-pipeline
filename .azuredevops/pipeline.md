# Pipeline Setup

It is assumed you have control of an Azure DevOps (ADO) project. If you do not have one already, please create one.

## Create a Service Principal

Currently, this pipeline makes use of a Service Principal as a security context when interacting with Azure subscriptions. This may change in future updates. If you do not have one already, please create a Service Principal via the following command:

```bash
az ad sp create-for-rbac --name gitops-automation
```

Record the output from this command as it will be required for the next section.

## Configure Automation Credentials

In ADO, navigate to **Pipelines** / **Library** and create a new Variable group. Name the group **pipeline-automation-credentials** and create the variables:

- `servicePrincipalId` where the value equals 'appId' from the previous step
- `servicePrincipalPwd` where value equals 'password' from the previous step. Use the lock icon to secure this parameter.
- `tenantId` where value equals 'tenant' from the previous step

## Configure ADO Environment

The deployment task within the Release pipeline targets a specifc [ADO environment](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/environments?view=azure-devops) called `test-environment`. This is necessary for assigment of release approval gates for the release (future).

In ADO, navigate to **Pipelines** / **Environmnets** and click the **New Environment** button in the upper right. Name the environment `test-environment` and set the Resource field to `None`. Click **Create** when finished.

## Download source and create your repository in ADO

Clone this repository to your local computer:

```bash
git clone https://github.com/travisnielsen/azure-gitops-pipeline.git
```

In ADO, create a Project (if one does not exist already) and then navigate to Repos. Create a new Repo and give it a descriptive name. Follow the instructions to push your local copy to this new ADO repo.

## Update Pipeline Variables

In [pipeline-build.yaml](pipeline-build.yaml), update the following pipeline variables:

- `rootFolder` must match the name of your repository
- `subscriptionId`  must match a given Azure subscription you intend to target for deployments

In [pipeline-release.yaml](pipeline-release.yaml), update the following pipeline variables:

- `projectName` must match the name of your ADO project
- `subscriptionId`  must match a given Azure subscription you intend to target for deployments

When finished, commit and push your changes to the ADO repoistory.

## Create Pipelines

In this design, two pipelines must be created, one for builds and a second one for releases. Follow these steps to create each one:

1. In ADO, navigate to **Pipelines** and select the **New pipeline** button in the upper right corner of the screen.
2. Select **Azure Repos Git**.
3. Pick the name of the repository you created earlier.
4. Select **Existing Azure Pipelines YAML file**
5. At the next prompt, set the value of **Path** to `.azuredevops/pipeline-build.yaml`.
6. Click the `Run` button to create the pipeline
7. After creation, you must manually change the name of the pipeline. This can be done by selecting the new pipeline and then clicking the elipsis in the upper right-hand corner. Click **Rename/move** and name the pipeline `gitops-build`.

Repeat the above steps for the release pipeline. For `step 6`, use `.azuredevops/pipeline-release.yaml` for the path name. For `step 7`, name the pipeline `gitops-release`

## Configure Branch Policy

The final step is to configure a Branch Policy to enforce Pull Requests for the main branch of your repository with required successful execution of the build (test) process for any infrastructure-related changes.

Navigate to **Repos** / **Branches** and find the `master` branch. Use the elipsis in the middle of the screen and select **Branch policies**

On the next screen, check **Require a minimum number of reviewers** and set the value of **Minimum number of reviewers to** to `1`.

Check **Requestors can approve their own changes**

Under **Build validation**, click `Add build policy` and select `gitops-build` for the **Build pipeline** field.

Next, set the following in the **Path filter** field: `/parameters/*;/templates/*`. This will limit the build validation process to only ARM templates included in a Pull Request.

Click **Save** to finish.

## Testing

You should now be able to test the pipeline by following the [developer instructions](../README.md).
