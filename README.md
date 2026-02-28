# Kubernetes-End-to-End-project-on-EKS-

#Install using Fargate

eksctl create cluster --name demo-cluster --region us-east-1 --fargate


#IAM OIDC provider

oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5) 


#Download IAM policy

curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json


#Create IAM Policy

aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json


#Create IAM Role

eksctl create iamserviceaccount \
  --cluster=<your-cluster-name> \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve

#ALB controller

helm repo add eks https://aws.github.io/eks-charts
#Update the repo
helm repo update eks


#Install

helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=<your-region> \
  --set vpcId=<your-vpc-id>





  <img width="1903" height="875" alt="Screenshot 2026-02-27 124941" src="https://github.com/user-attachments/assets/fa833dc2-4389-4cf2-9e92-5cd07e8fdec3" />





#game over



<img width="1899" height="869" alt="Screenshot 2026-02-27 115402" src="https://github.com/user-attachments/assets/3a3f1295-2551-427b-898d-af8921d325ca" />
