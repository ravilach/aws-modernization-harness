# Verification Setup

In this step will be deploying the Kubernetes Metrics Server in EKS and Prometheus on the EKS Cluster.
Kubernetes Metrics Server
Installing the Metrics Server can be done in one kubectl command. 
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml


## Prometheus Installation

Harness can facilitate the installation of Prometheus. For this workshop, the Prometheus deployables have been configured to be accessible by the workloads in the  EKS Cluster in the Git Repository. 

Setup -> AWS Canary Lab -> Services  + Add Service
Name: Install Prometheus 
Deployment Type: Kubernetes


Once you hit Submit, click on three ellipses on the right hand side to Link Remote Manifests. 



Enter the following information:
Manifest Format: Kubernetes Specs
Source Repository: AWS Workshop
Branch: master
File/Folder: static/yamls/prometheus


Click Submit and you are ready to deploy Prometheus. 

## Prometheus Deployment

To deploy something in Harness, defining a Harness Workflow will define the steps on the deployment will be achieved. 

Setup-> AWS Canary Lab -> Workflows + Add Workflow
Name: Deploy Prometheus
Workflow Type: Rolling Deployment
Environment: The EKS Cluster
Service: Install Prometheus
Infrastructure Definition: My EKS Cluster


Click Submit and you are ready to deploy.

In the upper right hand corner of the Workflow, click Deploy.  



Hit Submit and Prometheus will be deployed. 



## Get Prometheus ELB Address

To grab the HTTP accessible address for Prometheus, run the following command.

kubectl describe service prometheus-service



Copy down the LoadBalancer Ingress address.
E.g a8241cbf8317f4c1ab5507fbce9d576c-671821569.us-east-2.elb.amazonaws.com
You can navigate to the Prometheus UI to validate installation by adding port 8080. 
E.g a8241cbf8317f4c1ab5507fbce9d576c-671821569.us-east-2.elb.amazonaws.com:8080



Add Prometheus as a Verification Provider
To wire Prometheus to Harness, setup as a Verification Provider. 

Setup -> Connectors -> Verification Providers + Add Verification Provider -> Prometheus
Display Name: EKS Prometheus 
URL: <http://_elb_address:8080/> e.g http://a8241cbf8317f4c1ab5507fbce9d576c-671821569.us-east-2.elb.amazonaws.com:8080/


Click Submit and Prometheus will be wired to Harness.
