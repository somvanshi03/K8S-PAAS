# AKS Cluster Setup

## Create Resource Group

	az group create -l eastus -g agicdemo

## Create AKS Cluster

	az aks create -n agic-cluster -g agicdemo --network-plugin azure --enable-managed-identity --enable-addons ingress-appgw --appgw-name agic-appgw --appgw-subnet-cidr "10.255.0.0/16" --node-count 1 --enable-cluster-autoscaler --min-count 1 --max-count 2 --node-vm-size Standard_F2s_v2 --generate-ssh-keys
	or
	az aks create -g agicdemo -n agic-cluster --os-sku Ubuntu --network-plugin azure --enable-managed-identity --os-sku Ubuntu --appgw-name myApplicationGateway --appgw-subnet-cidr 10.10.0.0/24 --node-count 1 --enable-cluster-autoscaler --min-count 1 --max-count 2 --node-vm-size Standard_F2s_v2 --generate-ssh-keys
	
	--os-sku Ubuntu  ### {AzureLinux, CBLMariner, Mariner, Ubuntu}
	--enable-aad --enable-azure-rbac
	--enable-ultra-ssd
	--node-osdisk-size 48
	
## Create application gateway with vNet.

	az network public-ip create -n ingress-appgw-ip -g agicdemo --allocation-method Static --sku Standard

	az network vnet create -n AKS-vNet -g agicdemo --address-prefix 10.20.0.0/16 --subnet-name AKS-Subnet --subnet-prefix 10.20.0.0/24 

	az network application-gateway create -n appgw -g agicdemo --sku Standard_v2 --public-ip-address ingress-appgw-ip --vnet-name AKS-vNet --subnet AKS-Subnet --priority 100



	appgwId=$(az network application-gateway show -n appgw -g agicdemo -o tsv --query "id") 

	az aks enable-addons --name agic-cluster -g agicdemo --addon ingress-appgw --appgw-id $appgwId
	
## Get the AKS Cluster credentials

	az aks get-credentials -g agicdemo -n agic-cluster
	
## Verify Cluster info
	
	kubectl get nodes
	kubectl get nodes -o wide

## Assign network contributer role to AGIC addon Managed identity

### Get application gateway id from AKS addon profile

	appGatewayId=$(az aks show -n agic-cluster -g agicdemo -o tsv --query "addonProfiles.ingressApplicationGateway.config.effectiveApplicationGatewayId")
	echo $appGatewayId

### Get Application Gateway subnet id

	appGatewaySubnetId=$(az network application-gateway show --ids $appGatewayId -o tsv --query "gatewayIPConfigurations[0].subnet.id")
	echo $appGatewaySubnetId

### Get AGIC addon identity

	agicAddonIdentity=$(az aks show -n agic-cluster -g agicdemo -o tsv --query "addonProfiles.ingressApplicationGateway.identity.clientId")
	echo $agicAddonIdentity

### Assign network contributor role to AGIC addon Managed Identity to subnet that contains the Application Gateway

	az role assignment create --assignee $agicAddonIdentity --scope $appGatewaySubnetId --role "Network Contributor"

## Verify AKS Add On
	# List Kubernetes Deployments in kube-system namespace
		kubectl get deploy -n kube-system
	Observation:
	1. Should find the deployment with name "ingress-appgw-deployment"
	2. This is the Azure Application Gateway Ingress Controller Kubernetes Deployment Object

## List Pods
	kubectl get pods -n kube-system

## Describe Pod
	kubectl -n kube-system describe pod <AGIC-POD-NAME>
	kubectl -n kube-system describe pod ingress-appgw-deployment-55965f45cf-x28fm  
	Observation:
	1. Review the line where you can find ingress current version downloaded
	2. Pulling image "mcr.microsoft.com/azure-application-gateway/kubernetes-ingress:1.5.3"
	3. You can also run the below command to find the AppGW Ingress version
	
	kubectl get deploy ingress-appgw-deployment -o yaml -n kube-system | grep "image:"

	# Verify ingress-appgw pod Logs
	
	kubectl -n kube-system logs -f $(kubectl -n kube-system get po | egrep -o 'ingress-appgw-deployment[A-Za-z0-9-]+')