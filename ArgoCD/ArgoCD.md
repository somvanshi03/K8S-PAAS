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
		
		kubectl get application -n argocd 
		
		
## ArgoCD Component

### argocd-server
		
		This is arogcd api server which we interacts argocd via GUI and CLI. and It takes request from user.
	
### argocd-repo
	
		Repo Server plays an important role in Argo CD architecture as it is responsible for interacting with the Git repository to generate the desired state for all Kubernetes resources that belongs to a given application.
	
### argocd-redis
	
		Redis is used by Argo CD to provide a cache layer reducing requests sent to the Kube API as well as to the Git provider. It also supports a few UI operations.
	
### argocd-notification-controller
	
	The Argo CD Notification Controller is a component within the Argo CD ecosystem, which is a continuous delivery (CD) tool for Kubernetes applications. It allows you to integrate Argo CD with notification systems, enabling you to receive alerts or notifications on specific events related to the state of your applications and clusters.
	
### argocd-dex-Server
	
	Argo CD relies on Dex to provide authentication with external OIDC providers. However other tools can be used instead of Dex. Check the user management documentation for more details.
	
### argocd-application-controller

	The Application Controller is responsible for reconciling the Application resource in Kubernetes synchronizing the desired application state (provided in Git) with the live state (in Kubernetes). The Application Controller is also responsible for reconciling the Project resource.	
	
### argocd-applicationset-controller

	The ApplicationSet Controller is responsible for reconciling the ApplicationSet resource.