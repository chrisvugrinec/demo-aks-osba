# demo-aks-osba

The Open Service Broker API is an API designed to make integration between your (micro)services and your paas services effortless.
This demo shows an example of a python app deployed on a Managed Kubernetes cluster that will use the Redis Paas Service from Azure by using the OSBA.

This is how it works on Azure with the Managed Kubernetes Cluster (AKS):
* You will create a service principal on your azure tenant
* You will install and configure the  Service Broker API in your Kubernetes cluster;  this will use your service principal
* Then you will install a tool that will use the broker in combination with kubectl, this tool is named svcat
* Install a PAAS service with the svcat tool, for eg REDIS with this command: svcat provision redis --class azure-rediscache --plan basic -p location=westeurope -p resourceGroup=datalinks
* Then create a binding to this service, this will create a secret in Kubernetes. This secret can be used by applications (like this voting app) to access the PAAs service


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
    * curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
 * by now you are good to go...just watch the flic on my youtube channel


## More info
* osba; https://www.openservicebrokerapi.org/
* osba on azure; https://github.com/Azure/open-service-broker-azure
* draft; https://github.com/Azure/draft/

