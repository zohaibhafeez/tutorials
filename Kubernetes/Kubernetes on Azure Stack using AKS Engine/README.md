# Create Kubernetes cluster on Azure Stack using AKS Engine 

We had problems with Microsft Ubuntu based AKS images, so we used Ubuntu image in our cluster definition file. I created a resource group called kubernetes and deployed the kubernets cluster inside it.

I also created VNET and subnets for masters and agents node:

Create a VNET "kubernets-vnet" with 10.3.0.0/16 Address Space

Create a Subnet kubernetes-master-subnet 10.3.8.0/21

Create a Subnet kubernetes-agents-subnet 10.3.16.0/21 

After you have downloaded the Aks Engine to one of your one Windows VMs in Azure Stack, run the following command after replacing the parameters with your environment specific values:

.\aks-engine.exe deploy --azure-env AzureStackCloud --location "local" --resource-group kubernetes --api-model ./azs-clusterdefinition.json --output-directory kube-rg --client-id  "your-client-id" --client-secret "your-client-secret" --subscription-id "your-subscription-id" --output-directory "C:\tools\logs" --force-overwrite 
