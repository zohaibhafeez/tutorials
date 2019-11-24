# Setup AKS on Azure and add Windows Nodes

I am setting up AKS cluster with Windows nodes using a dedicated Subnet for our cluster and a custom CIDR for services in Kubernetes.

Lets start with creating a resource group:
`az group create --name zlabaks-rg1 --location westus2`

#### Create a VNET and Subnet:
`az network vnet create --name zlabaks-vnet1   --resource-group zlabaks-rg1 --address-prefixes 10.98.0.0/16 \

--subnet-name zlabaks-kubecluster-subnet --subnet-prefix 10.98.16.0/21 `

#### Install aks-preview CLI extension
` az  extension  add --name aks-preview `

` az extension update --name aks-preview `
#### Register Windows preview feature
` az feature register --name WindowsPreview --namespace Microsoft.ContainerService `

It takes a few minutes for the registration to complete. Check on the registration status using the [az feature list](https://docs.microsoft.com/en-us/cli/azure/feature#az-feature-list) command:

`az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"`

When the registration state is `Registered`, press Ctrl-C to stop monitoring the state. Then refresh the registration of the _Microsoft.ContainerService_ resource provider using the [az provider register](https://docs.microsoft.com/en-us/cli/azure/provider#az-provider-register) command:

    az provider register --namespace Microsoft.ContainerService
   
#### Create AKS Cluster

    PASSWORD_WIN="zlabaksP@ssw0rd1"
    az aks create \
    --resource-group zlabaks-rg1 \
	--location WestUS2 \
    --name zlabaks1 \
    --node-count 3 \
    --enable-addons monitoring \
    --kubernetes-version 1.15.5 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username zohaib \
	--dns-name-prefix zlabaks1 \
	--dns-service-ip 10.99.0.10 \
    --enable-vmss \
	--node-vm-size Standard_DS11_v2 \
    --network-plugin azure \
	--service-cidr 10.99.0.0/16 \
	--load-balancer-sku standard \
    --network-plugin azure \
	--skip-subnet-role-assignment \
	--vnet-subnet-id /subscriptions/4d594161-de89-4fd0-9b9b-8c84d8000000/resourceGroups/zlabaks-rg1/providers/Microsoft.Network/virtualNetworks/zlabaks-vnet1/subnets/zlabaks-kubecluster-subnet
Once the cluster has been created successfully, add the Windows nodes:

    az aks nodepool add \
    --resource-group zlabaks-rg1 \
    --cluster-name zlabaks1 \
    --os-type Windows \
    --name wnpool \
    --node-count 3 \
	--node-vm-size Standard_DS11_v2 \
    --kubernetes-version 1.15.5
