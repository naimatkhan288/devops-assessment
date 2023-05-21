# DevOps Assessment
## _intro_

hi...

this is an assessment that consist of four stages, complete each stage as you see fit, then submit it to a github repo.

## _Prerequisites_

This assessment requires running a k8s cluster (k3d), you can run the cluster on your personal machine or for a better environment you can use any cloud service (e.g, an ec2 instance from AWS).
To complete this assessment your machine need to have the following installed :

- docker
- make (CLI tool)
- WSL (in case you are running windows)


## I - environment preparation

This assessment uses a lightweight k8s cluster called k3d that can simulate a cluster of more than one node on your personal machine. 

∴ the first part of the asseessment is to install _k3d_.

## II - cluster preparation 

Once k3d is installed, you can run the command `make cluster` in the root dirctory of the project, this command will create a k3d cluster of one node. once the cluster is created you can interact with it using `kubectl`.

First, in order to prepare the cluster you will need to install _helm_ (the k8s package manager). 

Afterwards, you need to install and deploy the following services to your cluster: 

- nginx ingress 
- prometheus 

## III - service deployment

- create a dockerfile for the nodejs sample service in the root dir.
- create a deployment file for the sample service with the following conditions :
    - set a soft limit and a hard limit on the service resources consumption.
    - deploy the service without root privileges.
    - limlit the deployment capabilities to only the necessary ones.
    - set the filesystem to be read only.
    - disallow privilege escalation.
    - livenessProbe and readinessProbe must be set on the deployment file (you can find the health endpoit by browsing to /sample-service/index.js).
- create a service file for the sample service.
- create an ingress file that exposes the sample service to outside of the cluster.

all the resources mentioned above should exist on the same one namespace, and the service should be running and accessible. 

## IV - bonus

- deploy jenkins (CICD tool) to your cluster using helm.
- create a pipeline in jenkins that consists of two main stages : 
    - stage 1 : build the sample service docker image and pushes it to dockerhub.
    - stage 2 : deploy the sample service to the cluster with a rollout strategy.


 ✨ Good Luck ✨

 ------------------------------------------------------------------------
 Steps to do the Assessment

## I - environment preparation:
a) Create a cluster:
Export export K3S_CLUSTER_ARGS terminal

export K3S_CLUSTER_ARGS=‘--no-deploy=traefik’

And then 
make cluster

Cluster is ready, t heck the cluster
k3d cluster list 
kubectl get all 

b) Install helm:
Download Helm: Visit the Helm GitHub repository (https://github.com/helm/helm/releases)
Install helm using the 
Verify Helm installation: To ensure that Helm is installed correctly and working, you can run the following command:
helm version


## II - cluster preparation:
Install the following services to your cluster: 
- nginx ingress
- prometheus

- Make sure you have helm installed on your machine 
- Add the necessary Helm repositories 

Create a namespace 
kubectl create namespace ingress-nginx

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

Install Nginx Ingress:

helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx
Install Prometheus:

helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx
helm install prometheus prometheus-community/prometheus --namespace ingress-nginx

Once the installation is complete, you should have Nginx Ingress and Prometheus running in your cluster. You can verify their deployment using kubectl commands,
kubectl get deployments -n <namespace>.
kubectl get deployments -n ingress-nginx

## III - service deployment:
Build image from the Dockerfile and run image locally
docker build -t naimat/devops-task .
docker run -p 3000:80 -d naimat/devops-task
-# docker ps 
    
To verify the application locally or using ec2 instance public ip, I used public ip of ec2 intance to check the it works locally.

<public-ip>:3000

<public-ip>:3000/health

<public-ip>:3000/info
    
# Push to Dockerhub:
docker login
docker push naimat/devops-task 
    
# Pull the image:
docker push naimat/devops-task

# Deploy to Kuberntes cluster:
kubectl apply -f  sample-service-deployment.yaml

kubectl apply -f  sample-service-service.yaml

kubectl apply -f  sample-service-ingress.yaml

# The cluster is up and live     
kubectl get all
    
# To check service
kubectl get svc 
    
# To check pods running
kubectl get pods
    
# To check deployment
kubectl get deployment 
    
# Ingress controler:

kubectl get ingress

kubectl get po -n kube-system

kubectl get all -n kube-system

kubectl get all -n ingress-nginx

kubectl describe svc -n ingress-nginx

kubectl get svc -n ingress-nginx

kubectl -n ingress-nginx get pods 

sudo vi /etc/hosts 
127.0.0.1  devopstask.com

kubectl -n ingress-nginx port-forward svc/ingress-nginx-controller 3000:443
 
# To Access 

https://devopstask.com:3000/

https://devopstask.com:3000/info

https://devopstask.com:3000/health
