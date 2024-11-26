# Deploy and Verify


		# Deploy Apps
		kubectl apply -f manifests/

		# List Deployments
		kubectl get deploy

		# List Pods
		kubectl get pods

		# List Services
		kubectl get svc

		# List Ingress
		kubectl get ingress
		
# Access Applications

		# Access using curl
		curl http://<INGRESS-IP>

		# Access using Browser
		http://<INGRESS-IP>
		

# Verify Application Gateway Ingress Pod Logs
			
		# Verify ingress-appgw pod Logs 
		kubectl -n kube-system logs -f $(kubectl -n kube-system get po | egrep -o 'ingress-appgw-deployment[A-Za-z0-9-]+')
		
# Verify Azure AKS Cluster using Azure Portal


# Verify Application Gateway using Azure Portal 		

		# Verify pod Logs 
		kubectl get pods 
		kubectl logs -f <POD-NAME>
		kubectl logs -f myapp-nginx-deployment-77df6b5877-wlznj  

		# In parallel access Application via browser
		http://<INGRESS-IP>

		## HEALTH PROBE LOGS (Application Gateway polling the Pod)
		10.225.0.6 - - [03/Sep/2023:11:33:27 +0000] "GET / HTTP/1.1" 200 218 "-" "-" "-"
		10.225.0.4 - - [03/Sep/2023:11:33:28 +0000] "GET / HTTP/1.1" 200 218 "-" "-" "-"

		# Log for request we made from browser
		10.225.0.6 - - [03/Sep/2023:11:34:07 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36" "175.101.19.211:19596"
		
# Clean-Up Applications
		# Delete Apps
		kubectl delete -f manifests/

		# Review Application Gateway Tabs
		1. Backend Pools Tab 
		2. Backend Settings Tab
		3. Rules Tab		