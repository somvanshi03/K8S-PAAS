# GitOps (ArgoCD)

## Install ArgoCD on K8S Cluster

	kubectl create namespace argocd
	kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
	
## Access The Argo CD API Server

	kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
	
	Or 
	
	Edit the ArgoCD Service and Change ClusterIP with NodePort and Save it.
	
## Get Login Password

	
	
## ArgoCD Component

	