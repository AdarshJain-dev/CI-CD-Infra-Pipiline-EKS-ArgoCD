# 1️⃣ Terraform – Provision AWS EKS Cluster
Step 1: Initialize Terraform
cd terraform
terraform init

Step 2: Validate Terraform
terraform validate

Step 3: Apply Terraform
terraform apply -auto-approve

Step 4: Update kubeconfig
aws eks update-kubeconfig --region ap-south-1 --name demo-eks-argocd-cluster

# 2️⃣ Deploy NGINX on Kubernetes
Step 1: Apply Deployment
kubectl apply -f manifests/deployment.yaml

Step 2: Apply Service
kubectl apply -f manifests/service.yaml

Step 3: Verify Deployment
kubectl get pods
kubectl get svc nginx-service

# 3️⃣ Install & Access ArgoCD
Step 1: Create Namespace
kubectl create namespace argocd

Step 2: Install ArgoCD
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Step 3: Port-forward ArgoCD UI
kubectl port-forward svc/argocd-server -n argocd 8080:443

Step 4: Retrieve Admin Password
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 --decode

Step 5: Login

Open browser and navigate to:

https://localhost:8080


User: admin
Password: (output from previous step)

# 4️⃣ Deploy Application Using ArgoCD (GitOps)
Step 1: Apply ArgoCD Application Manifest
kubectl apply -f argocd/application.yaml

Step 2: Verify Application Sync
kubectl get applications -n argocd


Open ArgoCD UI → Sync Application.

# 5️⃣ Access NGINX Application
Option A: Using LoadBalancer
kubectl get svc nginx-service


Copy EXTERNAL-IP → Open in browser.

Option B: Port-Forward
kubectl port-forward deployment/nginx-app 8081:80


Open:

http://localhost:8081
