# GitOps (ArgoCD)

## Install ArgoCD on K8S Cluster

	kubectl create namespace argocd
	kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## Verify ArgoCD Pods

	kubectl get pods -n argocd 

## Get the Service list

	kubectl get svc -n arogcd

	
## Access The Argo CD API Server

	kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
	
	Or 
	
	Edit the ArgoCD Service and Change ClusterIP with NodePort and Save it.

## Create Tunnel or Port forwarding

	kubectl service list -n argocd 
	kubectl service argocd-server -n argocd 
		
	
## Get Login Password

	kubectl get secret -n argod
	kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d 
	
	or 
	
	kubectl edit secret argocd-initial-admin-secret -n argocd
	
	
	### Decrypt Password with base64
	
		echo  SEHR@#@@#$!EREGS== | base64 decode



## Install ArgoCD using CLI
	
	### Create application 
		
		Ref: https://argo-cd.readthedocs.io/en/stable/user-guide/commands/argocd_app_create/
		
		argocd login 

		argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-namespace default --dest-server https://kubernetes.default.svc --directory-recurse --sync-policy automated --self-heal --sync-option Prune=true --sync-option CreateNamespace=true
		
		argocd app create app-spring-petclinic --repo https://github.com/redhat-developer/openshift-gitops-getting-started.git --path app --revision main --dest-server https://kubernetes.default.svc --dest-namespace spring-petclinic --directory-recurse --sync-policy automated --self-heal --sync-option Prune=true --sync-option CreateNamespace=true
		
		
## ArgoCD Component

	### argocd-server
		
		This is arogcd api server which we interacts argocd via GUI and CLI. and It takes request from user.
	
	### argocd-repo
	
		This workes with verion control system to store manifest files. and argocd only supports git based version control.
	
	### argocd-redis
	
		This keeps for caching the data.
	
	### argocd-notification-controller
	
	
	### argocd-dex-Server
	
	
	### argocd-application-controller
	
	
	### argocd-applicationset-controller