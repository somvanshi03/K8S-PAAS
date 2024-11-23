# AKS Cluster Setup

## Create Resource Group

	az group create -l eastus --g agicdemo

## Create AKS Cluster

	az aks create --name agic-cluster --resource-group agicdemo --network-plugin azure --enable-manged-identity --enable-addons ingresss-appgw --appgw-name agic-appgw --appgw-subnet-cidr "10.255.0.0/16" --node-count 1 --enable-cluster-autoscaler --min-count 1 --max-count 2 --generate-ssh-keys
## Get the AKS Cluster credentials

	az aks get-credentials --resource-group agicdemo --name agic-cluster
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