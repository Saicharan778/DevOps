# Kubernetes EKS Cluster Setup

## Step 1 — Update Ubuntu
```bash
sudo apt update
sudo apt upgrade -y
```

## Step 2 — Install AWS CLI
```bash
aws --version
```
If AWS CLI is not installed:
```bash
sudo apt install -y unzip curl
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

## Step 3 — Configure AWS CLI
```bash
aws configure
```
Use your AWS credentials and region, for example:
```text
Default region name: ap-south-1
Default output format: json
```

Verify:
```bash
aws sts get-caller-identity
```

## Step 4 — Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

## Step 5 — Install eksctl
```bash
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH
curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_${PLATFORM}.tar.gz"
tar -xzf eksctl_${PLATFORM}.tar.gz -C /tmp
sudo install -m 0755 /tmp/eksctl /usr/local/bin/eksctl
eksctl version
```

## Step 6 — Create the EKS Cluster
Example: Mumbai region, 2 worker nodes.
```bash
eksctl create cluster   --name my-eks-cluster   --region ap-south-1   --nodegroup-name worker-nodes   --node-type t3.medium   --nodes 2   --nodes-min 2   --nodes-max 3   --managed
```

## Step 7 — Verify the Cluster
```bash
eksctl get cluster --region ap-south-1
kubectl get nodes
kubectl cluster-info
```

Both worker nodes should normally show `Ready`.

## Step 8 — Deploy a Test Application
```bash
kubectl create deployment nginx --image=nginx
kubectl get deployments
kubectl get pods
```

## Step 9 — Expose NGINX with NodePort
```bash
kubectl expose deployment nginx --type=NodePort --port=80
kubectl get svc
```

Find the NodePort from the `PORT(S)` column, for example:
```text
80:30080/TCP
```

## Step 10 — Check Which Node Runs the Pod
```bash
kubectl get pods -o wide
```
The `NODE` column shows the worker node running the pod.

## Step 11 — Get Node Information
```bash
kubectl get nodes -o wide
```

## Step 12 — Delete the Test Application
```bash
kubectl delete service nginx
kubectl delete deployment nginx
```

## Step 13 — Delete the Entire EKS Cluster
When finished with the lab:
```bash
eksctl delete cluster   --name my-eks-cluster   --region ap-south-1
```

Verify:
```bash
eksctl get cluster --region ap-south-1
```

## Quick Command Summary
```bash
aws sts get-caller-identity
kubectl version --client
eksctl version

eksctl create cluster   --name my-eks-cluster   --region ap-south-1   --nodegroup-name worker-nodes   --node-type t3.medium   --nodes 2   --nodes-min 2   --nodes-max 3   --managed

kubectl get nodes
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=NodePort --port=80
kubectl get svc
kubectl get pods -o wide
```

## Important
- `ap-south-1` is the Mumbai region. Change it if needed.
- `t3.medium` is used as a practical lab node size; AWS charges apply.
- Do not commit AWS access keys to GitHub.
