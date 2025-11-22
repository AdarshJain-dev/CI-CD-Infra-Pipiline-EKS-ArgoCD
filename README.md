# Configure aws cli in your system
aws congigure 

# Terraform Execution to create 
- Create a VPC (optional: use EKS module with VPC support)
- Deploy an EKS Cluster
- Create IAM roles and node groups
- Output kubeconfig credentials to access the cluster.

# Terraform Dirctory
cd terraform

# 1. Initialize
terraform init

# 2. Check plan
terraform plan

# 3. Apply
terraform apply -auto-approve

# configure kubeconfig
aws eks --region ap-south-1 update-kubeconfig --name demo-eks-argocd-cluster

# Test (Wait a bit then try check)
kubectl get nodes


# Install ArgoCD on EKS

kubectl apply -f argocd/namespace.yaml

# Install ArgoCD (official install manifest)
kubectl apply -n argocd \ -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Validate
kubectl get pods -n argocd

# Expose ArgoCD server
kubectl patch svc argocd-server -n argocd \ -p '{"spec": {"type": "LoadBalancer"}}'

# Get external IP / hostname:
kubectl get svc argocd-server -n argocd

# Get ArgoCD admin password
kubectl -n argocd get secret argocd-initial-admin-secret \ -o jsonpath="{.data.password}" | base64 --decode; echo

Username: admin

##
