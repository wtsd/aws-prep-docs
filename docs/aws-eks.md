# ☸️ Amazon Elastic Kubernetes Service (EKS)

## Purpose
Amazon EKS is a **managed Kubernetes service** that makes it easy to run Kubernetes without managing control plane nodes.

---

## Key Features
- **Fully managed control plane** (AWS runs masters, etcd, API server).  
- **Integration with AWS services**: IAM, CloudWatch, Load Balancers, EBS/EFS/FSx.  
- **Supports Managed Node Groups** and **Fargate Profiles**.  

---

## Best Practices
- **Cluster Security**
  - Use **IRSA (IAM Roles for Service Accounts)** for pod-level permissions.  
  - Enable **control plane logging** to CloudWatch.  
  - Restrict public API endpoint access.  
- **Networking**
  - Use **CNI plugin** for VPC-native pods.  
  - Plan IP ranges carefully.  
- **Scaling**
  - Use **Cluster Autoscaler** for nodes.  
  - Use **HPA (Horizontal Pod Autoscaler)** for pods.  
- **Observability**
  - Deploy **Prometheus & Grafana**.  
  - Use **FluentBit/CloudWatch agent** for logs.  
- **Cost Optimization**
  - Spot instances for non-critical workloads.  
  - Fargate for bursty workloads.  

---

## Example: Create an EKS Cluster with eksctl

```bash
eksctl create cluster   --name demo-cluster   --region us-east-1   --nodegroup-name standard-workers   --node-type t3.medium   --nodes 3
```

---

## Example: Update kubeconfig

```bash
aws eks update-kubeconfig   --region us-east-1   --name demo-cluster

kubectl get nodes
```

---

## Example: IAM Roles for Service Accounts (IRSA)

1. Create IAM policy for S3 access:
```bash
aws iam create-policy   --policy-name S3ReadOnly   --policy-document file://s3-readonly-policy.json
```

2. Associate IAM role with Kubernetes service account:
```bash
eksctl create iamserviceaccount   --cluster demo-cluster   --namespace default   --name s3-reader   --attach-policy-arn arn:aws:iam::123456789012:policy/S3ReadOnly   --approve
```

3. Deploy a pod using this service account:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: s3-reader-pod
  namespace: default
spec:
  serviceAccountName: s3-reader
  containers:
  - name: app
    image: amazonlinux
    command: [ "sleep", "3600" ]
```

---

## Example: Enable CloudWatch Logging

```bash
eksctl utils update-cluster-logging   --enable-types all   --region us-east-1   --cluster demo-cluster   --approve
```

---

✅ **Summary Workflow**  
1. Provision cluster with eksctl or CloudFormation.  
2. Configure IAM OIDC provider for IRSA.  
3. Deploy workloads with least-privilege IAM roles.  
4. Enable monitoring/logging.  
5. Automate scaling and updates.  
