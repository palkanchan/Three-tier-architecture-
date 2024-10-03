# Three-tier-architecture

Overview

Design and implemented three-tier web application using ReactJs, NodeJs, and MongoDB, with deployment on AWS EKS.This architecture will seperate the application into three layers presentation layer, Businesss logic layer and Data access layer,enhancing modularity, scalibility, and maintaibility.

Objectives

   • Design a robust three-tier architecture tailored for a web application.
 
   •	Develop the Presentation Layer using React.js to create an interactive user interface.
 
   • 	Implement the Business Logic Layer with Node.js and Express.js to handle application logic and API endpoints.
    
   •	Set up the Data Access Layer using MongoDB to manage data storage and retrieval.

 Tools Explored:
 
   •	IAM
    
   •	Ec2
    
   •	Kubectl
    
   •	Eksctl cluster 
    
   •	Docker
    
   •	Load Balancer
    
   •	ECR


Step 1: IAM Configuration

   •	Create a user eks-admin with AdministratorAccess.

   •	Generate Security Credentials: Access Key and Secret Access Key.

Step 2: EC2 Setup

   •	Launch an Ubuntu instance in region (eg. region us-west-2).

   •	SSH into the instance from your local machine.

Step 3: Install AWS CLI v2

    •	curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    •	sudo apt install unzip
    •	unzip awscliv2.zip
    •	sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
    •	aws configure

Step 4: Install Docker

    •	sudo apt-get update
    •	sudo apt install docker.io
    •	docker ps
    •	sudo chown $USER /var/run/docker.sock

Step 5: Install kuectl

    •	curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl 
    •	chmod +x ./kubectl
    •	sudo mv ./kubectl /usr/local/bin
    •	kubectl version --short –client
   
Step 6: Install eksctl

    •	eksctl create cluster --name three-tier-cluster --region us-west-2 --node-type t2.medium --nodes-min 2 --nodes-max 2
    •	aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster
    •	kubectl get nodes
   
Step 7: Setup EKS Cluster

    •	eksctl create cluster --name three-tier-cluster --region us-west-2 --node-type t2.medium --nodes-min 2 --nodes-max 2
    •	aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster
    •	kubectl get nodes
   
Step 8: Run Manifest

    •	kubectl create namespace workshop
    •	kubectl apply -f .
    •	kubectl delete -f .
  
Step 9: Install AWS Load Balancer.

    •	curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
    •	aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json
    •	eksctl utils associate-iam-oidc-provider --region=us-west-2 --cluster=three-tier-cluster --approve
    •	eksctl create iamserviceaccount --cluster=three-tier-cluster --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy- 
      arn=arn:aws:iam:::policy/AWSLoadBalancerControllerIAMPolicy --approve --region=us-west-2
         
Step 10: Deploy AWS Load Balancer Controller.

    •	sudo snap install helm --classic
    •	helm repo add eks https://aws.github.io/eks-charts
    •	helm repo update eks
    •	helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=my-cluster --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer- 
       controller
    •	kubectl get deployment -n kube-system aws-load-balancer-controller
    •	kubectl apply -f full_stack_lb.yaml



