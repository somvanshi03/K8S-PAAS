# HelmChart Part 2

## Create a Directory for Your Helm Charts

	mkdir my-helm-repo
	cd my-helm-repo

## Package Your Helm Chart

	helm create my-chart

## List of repo
	
	Helm repo list 
	
## Add remote repo 

	helm repo add bitnami https://charts.bitnami.com/bitnami
	
## Search remote repo

	helm search repo bitnami
	
## Install Helm Chart
	
	helm install my-release bitnami/<chart>
	
## Remove repo

	helm repo remove bitnami
	

 
