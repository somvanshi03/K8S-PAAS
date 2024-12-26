# Install and Configure GKE Cluster

 ## Install GCP CLI on windows
	
	https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe

 ## Step 1: Ensure You Are Authenticated

		gcloud auth login
		gcloud auth list
		gcloud config set project ms-lab-dec-2024
		gcloud container clusters list



 ## Step 2: Enable the GKE API

		gcloud services enable container.googleapis.com

 ## Step 3: Create a GKE Cluster

		gcloud container clusters create my-gke-cluster --zone us-central1-a --num-nodes 2 --machine-type n1-standard-1

 ## Step 4: Get Authentication Credentials for the Cluster
		
		gcloud components install gke-gcloud-auth-plugin
		gcloud container clusters get-credentials my-gke-cluster --region us-central1 --project ms-lab-dec-2024
		gcloud container clusters get-credentials my-gke-cluster --zone us-central1-a

 ## Step 5: Verify the Cluster

		kubectl get nodes

 ## Step 6: Delete GKE Cluster
 
		gcloud container clusters delete my-gke-cluster --zone us-central1-a


gcloud container clusters create my-cluster \
  --zone us-central1-a \
  --num-nodes 2 \
  --machine-type e2-medium \
  --disk-size 20GB \
  --enable-autoscaling \
  --min-nodes 2 \
  --max-nodes 2

gcloud container clusters describe my-cluster --zone us-central1-a



gcloud beta container --project "ms-lab-dec-2024" clusters create "cluster-1" --region "us-central1" --tier "standard" --no-enable-basic-auth --cluster-version "1.30.6-gke.1125000" --release-channel "regular" --machine-type "e2-medium" --image-type "COS_CONTAINERD" --disk-type "pd-balanced" --disk-size "20" --metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --max-pods-per-node "100" --num-nodes "2" --logging=SYSTEM,WORKLOAD --monitoring=SYSTEM,STORAGE,POD,DEPLOYMENT,STATEFULSET,DAEMONSET,HPA,CADVISOR,KUBELET --enable-ip-alias --network "projects/ms-lab-dec-2024/global/networks/default" --subnetwork "projects/ms-lab-dec-2024/regions/us-central1/subnetworks/default" --no-enable-intra-node-visibility --default-max-pods-per-node "110" --enable-ip-access --security-posture=standard --workload-vulnerability-scanning=disabled --no-enable-master-authorized-networks --no-enable-google-cloud-access --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver --enable-autoupgrade --enable-autorepair --max-surge-upgrade 1 --max-unavailable-upgrade 0 --binauthz-evaluation-mode=DISABLED --enable-managed-prometheus --enable-shielded-nodes --no-shielded-integrity-monitoring --node-locations "us-central1-a"



gcloud beta container --project "ms-lab-dec-2024" clusters create-auto "autopilot-cluster-1" --region "us-central1" --release-channel "regular" --tier "standard" --enable-ip-access --no-enable-google-cloud-access --network "projects/ms-lab-dec-2024/global/networks/default" --subnetwork "projects/ms-lab-dec-2024/regions/us-central1/subnetworks/default" --cluster-ipv4-cidr "/17" --binauthz-evaluation-mode=DISABLED

