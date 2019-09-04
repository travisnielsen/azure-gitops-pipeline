# Azure Machine Learning Workspace

## Defining the parameter file

The following parameters are required:

* `workspaceName`: Friendly name of your workspace

## Testing the deployment

You should test your deployment request in a sandbox enviornment. To do this, first ensure you have the latest version of the Azure CLI tools installed.

Next, run the following deployment command.

```bash
az group deployment create --name testworkspace --resource-group mltest --parameters .\mlworkspace.params.json --template-file mlworkspace.json
```