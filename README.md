# Cloud Resource Scanner - Azure Functions Sample App

[![Build Status](https://travis-ci.com/Microsoft/cloud-scanner-azure-functions-sample.svg?branch=master)](https://travis-ci.com/Microsoft/cloud-scanner-azure-functions-sample)

This is an example Azure Function App that demonstrates the use of the [cloud-scanner](https://github.com/Microsoft/cloud-scanner) library and its providers. [cloud-scanner](https://github.com/Microsoft/cloud-scanner) is a Python package that pulls cloud resources from different providers (Azure, AWS, GCP) and puts the metadata into data stores.

## Running Locally
1. Create Python 3.6 virtualenv `env` with all dependencies installed
    ```
    python3.6 -m virtualenv env
    source env/bin/activate
    (env) pip install -r requirements.txt
    ```
   If running on Windows CMD Prompt/Powershell:
   ```
   python3.6 -m virtualenv env
   .env\Scripts\activate
   (env) pip install -r requirements.txt
   ```
2. Create an [Azure Service Principal](docs/md/service-principal.md)
3. Create `.env` file in root directory and populate with appropriate data:
    ```
    # Azure authentication
    AZURE_CLIENT_ID=<service-principal-app-id>
    AZURE_CLIENT_SECRET=<service-principal-secret>
    AZURE_STORAGE_ACCOUNT=<azure-storage-account-name>
    AZURE_STORAGE_KEY=<azure-storage-account-key
    AZURE_TENANT_ID=<service-principal-tenant-id>
    AzureWebJobsStorage=<azure-storage-connection-string>

    # Container & Queue Names
    CONFIG_CONTAINER=config-files
    PAYLOAD_QUEUE_NAME=resource-payloads
    TAG_UPDATES_QUEUE_NAME=resource-tag-updates
    TASK_QUEUE_NAME=resource-jobs

    # Resource types
    RESOURCE_STORAGE_TYPE=rest_storage_service
    STORAGE_CONTAINER_TYPE=azure_storage
    QUEUE_TYPE=azure_storage_queue

    # Needed for 'rest_storage_service' storage type
    REST_STORAGE_URL=<api-url-for-posting-resources>
    
    # App Insights (optional)
    APPINSIGHTS_APPID=<app-insights-app-id>
    APPINSIGHTS_INSTRUMENTATIONKEY=<app-insights-instrumentation-key>
    ```
4. Create `local.settings.json` file in root directory and populate:
   ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "<azure-storage-connection-string",
            "FUNCTIONS_WORKER_RUNTIME": "python"
        }
    }
   ```
5. Generate cloud config file
   ```bash
   (env) generate-config -p <cloud-provider1,cloud-provider2> -t <resource-type1,resource-type2>
   ```
   Example: Generate config to pull all Azure resources
   ```bash
   (env) generate-config -p azure -t "*"
   ```
6. Install [Azure Core Tools](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local)
7. Register binding types
   ```bash
   (env) func extensions install
   ```
8.  [Deploy](docs/md/deployment.md) necessary resources
9.  Run Azure Functions locally
    ```bash
    (env) func host start
    ```

## Publish Function App to Azure

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
