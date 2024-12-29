# CICD 

## Enable container repository, artifactregistry, Cloud Build Services

	gcloud services enable container.googleapis.com cloudbuild.googleapis.com sourcerepo.googleapis.com artifactregistry.googleapis.com

## list of artifacts repo

	gcloud artifacts repositories list

## Create artifacts repo 
	
	gcloud artifacts repositories create myapps-repository --repository-format=docker --location=us-central1 

## List of artifacts

	gcloud artifacts repositories list

## Describe artifacts repo

	gcloud artifacts repositories describe myapps-repository --location=us-central1

## Source repo list 

	gcloud source repos list

## Create Repo

	gcloud source repos create myapp1-app-repo

## List Repo 

	gcloud source repos list
	https://source.cloud.google.com/repos
	
	