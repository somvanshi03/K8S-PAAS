# HelmChart

## Install Helm on Linux

### Update Your System
		
		sudo apt update
		sudo apt upgrade -y

### Install Dependencies
	
		sudo apt install -y apt-transport-https ca-certificates curl

###  Add the Helm APT Repository
	
		curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
		sudo apt update && sudo apt install -y software-properties-common
		sudo add-apt-repository "deb https://baltocdn.com/helm/stable/debian/ all main"


### Install Helm
	
		sudo apt update
		sudo apt install helm

### Verify Installation
	
		helm version
