# demo-aks-osba

The Open Service Broker API is an API designed to make integration between your (micro)services and your paas services effortless.
This demo shows an example of a python app deployed on a Managed Kubernetes cluster that will use the Redis Paas Service from Azure by using the OSBA.

## How

* clone this repo
* make sure you have your managed kube up and running
* make sure you are logged in to azure with the cli (az login) and that your user has the rights to create resources
* make sure you have draft,helm installed and configure (not required for the OSBA, but I am using it)
* follow the instructions on https://docs.microsoft.com/en-us/azure/aks/integrate-azure:
  * install osba
    * helm repo add azure https://kubernetescharts.blob.core.windows.net/azure
    * SERVICE_PRINCIPAL=$(az ad sp create-for-rbac)
    * AZURE_CLIENT_ID=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 4)
    * AZURE_CLIENT_SECRET=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 16)
    * AZURE_TENANT_ID=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 20)
    * AZURE_SUBSCRIPTION_ID=$(az account show --query id --output tsv) 
    * helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET


## More info
* https://www.openservicebrokerapi.org/
* https://github.com/Azure/open-service-broker-azure

