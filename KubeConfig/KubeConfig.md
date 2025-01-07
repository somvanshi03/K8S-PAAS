# Create Kubeconfig from scratch

## Add Cluster Details.

	kubectl config --kubeconfig=base-config set-cluster development --server=https://1.2.3.4
	
## Add User details.

	kubectl config --kubeconfig=base-config set-credentials experimeter --username=dev --password=password123
	

## Setting Contexts

	kubectl config --kubeconfig=base-config set-context dev-frontend --cluster=development --namespace=frontend --user=experimeter