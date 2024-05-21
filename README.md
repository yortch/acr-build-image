# acr-build-image
Demo for CI flow to build and deploy image to ACR


## Setup service principal

1. Create resource group
    ```
    RG=acr-demo
    az group create --name $RG --location eastus
    ```
1. Get the resource group id:
    ```
    RG_ID=$(az group show --name $RG --query id --output tsv)
    ```
1. Create a service principal (*NOTE:* if this command results in error 324, execute using Cloud Shell instead):
    ```
    az ad sp create-for-rbac --name acrPrincipal --scope $RG_ID --role Contributor --sdk-auth
    ```
1. Copy the json output from the command above to use in subsquent step.
1. Assign the `clientId` value as an environment variable:
    ```
    CLIENT_ID=<clientId value>
    ```
## Setup ACR
1. Create a new Azure Container Registry
    ```
    ACR_NAME=acrgithubactiondemo
    az acr create -n $ACR_NAME -g $RG --sku Standard
    ```
1. Get the ACR Id:
    ```
    ACR_ID=$(az acr show --name $ACR_NAME --resource-group $RG --query id --output tsv)
    ```
1. Grant `AcrPush` role to the service principal client id (*NOTE:* also execute from CloudShell if receiving Bad Request error):
    ```
    az role assignment create --assignee $CLIENT_ID --scope $RG_ID --role AcrPush
    ```

## Setup github secrets
1. Navigate to the "Settings" of your repo: [https://github.com/yortch/acr-build-image/settings]
1. Expand "Secrets and Variables" and click on "Actions": [https://github.com/yortch/acr-build-image/settings/secrets/actions]
1. Create each of the following secrets:
    |Secret                 | Value
    |-----------------------|-------
    |`AZURE_CREDENTIALS`    | The entire JSON output from the service principal creation step
    |`REGISTRY_LOGIN_SERVER`| The login server name of your registry (all lowercase). Example: myregistry.azurecr.io
    |`REGISTRY_USERNAME`    | The clientId from the JSON output from the service principal creation
    |`REGISTRY_PASSWORD`    | The clientSecret from the JSON output from the service principal creation
    |`RESOURCE_GROUP`       | The name of the resource group you used to scope the service principal
