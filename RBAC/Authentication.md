# K8S User and RBAC

## Use below steps to create use in k8s

	> Generate key file
	> Generate csr file
	> Generate csr k8s objects
	> k8s CA approved
	> cert file 
	
## Notes:

	Once you have key and cert file can be authenticated by kube-api using kubectl client.
	
	
## Generate a private key for the user

	openssl genpkey -algorithm RSA -out user.key

## Generate a certificate signing request (CSR)

	openssl req -new -key user.key -out user.csr -subj "/CN=user/O=group"

## Encode the csr with Base64

	cat user.csr | base64 | tr -d '\n'
	  
## Sign the CSR with the Kubernetes CA

	openssl x509 -req -in user.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out user.crt -days 365
	
	or 
	
	you can use below 
	
	
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: user-csr
spec:
  groups:
  - system: authenticated
  request: |
    -----BEGIN CERTIFICATE REQUEST-----
    (your CSR content here)
    -----END CERTIFICATE REQUEST-----
  usages:
  - digital signature
  - key encipherment
  - client auth


## approve CSR 

	kubectl certificate approve user-csr
	
## Download the certificate from csr

	kubectl get csr user-csr -o jsonpath='{.status.certificate}' | base64 -d > user.csr 
	
## Create new contexts

	kubectl config get-contexts 
	kubectl config set-credentials user --client-certificate=user.crt --client-key=user.key 
	
## Set new contents

	kubectl config set-contents user-context --cluster  <k8s_Cluster_Name> user=user 
	
	kubectl config get-contexts
	
	kubectl --context=user -context get pod
	
## Switch contexts

	kubectl config use-context user 
