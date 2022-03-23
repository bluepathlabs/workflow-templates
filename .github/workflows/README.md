# Shared Workflows
## Build, Tag, Push Docker Image to ACR
### Inputs
- `dockerfile-path`:
  - required: `false`
  - type: `string`
  - default: `Dockerfile`
  - Description:
    - Path to the Dockerfile to be built
- `image-name`:
  - required: `true`
  - type: `string`
  - Description:
    - Text value to be used for the docker image name (do not include the container registry url/name)
- `image-tag`:
  - required: `false`
  - type: `string`
  - default: ' '
  - Description:
    - Custom tag to be applied to the image being built.
    - When default is used the image will be tagged based on the current branch being built (Pull Request = `qa`, develop = `develop`, main = `main`)
- `azure-environment`:
  - type: `string`
  - default: `AzureCloud`
  - Description:
    - Which Azure Environment to be used to login and connect to the target container registry
### Secrets
- `AZURE_CREDENTIALS`:
  - required: `true`
  - Description:
    - JSON object for the Azure Service Principal credentials
    - Can be regenerated following [these instructions](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli#6-reset-credentials) if needed
- `KVT_NAME`:
  - required: `true`
  - Description:
    - Name of the Azure Key Vault resource
- `ACR_LOGIN_SVR_SCT`:
  - required: `true`
  - Description:
    - The name of the key vault secret containing the login server of the Azure Container Registry resource
- `ACR_USR_SCT`:
  - required: `true`
  - Description:
    - The name of the key vault secret containing the username to be used to login/manage the targeted Azure Container Registry
- `ACR_PWD_SCT`:
  - required: `true`
  - Description:
    - The name of the key vault secret containing the password to be used to login to the targeted Azure Container Registry
- `CUSTOM_BUILD_ARGS`:
  - required: `false`
  - Description:
    - Text value of any sensitive build arguments that may need to be passed to the `docker build` command
    - The text value is passed directly to the command so any arguments need to be prefixed with the argument type (e.g. "`--build-arg CUSTOM_VAR=custom`")
### Steps
1. Checkout Repository
1. Generate Docker tag based on input or branch name
1. Login to Azure
1. Pull the passed named Secrets from Azure Key Vault
1. Build the docker image with name, tag, and inputs
1. Push the built docker image to the Azure Container Registry
