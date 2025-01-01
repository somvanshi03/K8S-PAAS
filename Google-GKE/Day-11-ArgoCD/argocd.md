# Integrate GCP with ArgoCD

## Steps to Integrate ArgoCD

	> Provision GKE Private Cluster and connect via your local desktop
	> Install ArgoCD
	> Expose the service
	> Access via tunnel

## Notes:

	> Private GKE cluster dont have internet access.
	> Need to create NAT rule in default vpc. so cluster can pull the docker images.
	> 
	
	
	
## Create GKE Private Cluster with Authorized Networks option enabled

	gcloud container clusters create argo-cd --create-subnetwork name=my-subnet-2 --enable-master-authorized-networks --enable-ip-alias --enable-private-nodes --master-ipv4-cidr 192.168.0.0/28 --num-nodes=3
 
## Create NAT router
	 gcloud compute routers create nat-router --network default
	 gcloud compute routers nats create nat-config --router nat-router --nat-all-subnet-ip-ranges --auto-allocate-nat-external-ips
 
## Install Argo CD
	https://argo-cd.readthedocs.io/en/stable/getting_started/ 

	kubectl create namespace argocd
	kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## Verify all pods related to argocd.

	Check via GUI and cli too.
	kubectl get pod -n argocd 
	
## Note: 

	> Argocd service using ClusterIP and we need to change this to nodeport
	
## Expose the service 

	kubectl -n argocd patch svc argocd-server --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]'

## Access the Argo from browser
	
	gcloud compute firewall-rules create allow-ingress-from-iap --direction=INGRESS --action=allow   --rules=tcp:32147 --source-ranges=35.235.240.0/20
    
	gcloud compute start-iap-tunnel gke-argo-cd-default-pool-683b0de0-fsst 32147  --local-host-port=localhost:8085 --zone=us-east1-b
	   

## Get the login password

	argocd admin initial-password -n argocd
	kubectl -n argocd get secret argocd-initial-admin-password -o jsonpath="{.data.password}" | base64 -d; echo
	
