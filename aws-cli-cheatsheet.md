# AWS CLI Cheat Sheet

A practical, copy-pasteable guide from **authentication** → **create** → **monitor** → **delete**.
All commands accept `--profile <name>` and `--region <code>`.

> Safety first: many delete/stop APIs are **irreversible**. Use `--dry-run` where supported and scope with tags.

---

## 0) Install & Basics
```bash
# install v2 on Linux (see docs for macOS/Windows)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
unzip awscliv2.zip && sudo ./aws/install

aws --version
aws configure list
aws help
```

### Output & Filtering
```bash
aws ec2 describe-instances --output table
aws ec2 describe-instances --query "Reservations[].Instances[].InstanceId"
aws s3api list-buckets --query "Buckets[].Name" --output text
```

---

## 1) Authentication

### 1.1 Access keys (basic)
```bash
aws configure
# Prompts for AWS Access Key ID, Secret, region, output
```

### 1.2 Profiles
```bash
aws configure --profile dev
export AWS_PROFILE=dev
aws sts get-caller-identity
```

### 1.3 SSO (recommended for enterprises)
```bash
aws configure sso --profile corp
aws sso login --profile corp
aws sts get-caller-identity --profile corp
```

### 1.4 Assume role (with optional MFA)
```bash
aws sts assume-role   --role-arn arn:aws:iam::123456789012:role/Admin   --role-session-name cli-session   --serial-number arn:aws:iam::123456789012:mfa/your.name   --token-code 123456

# Export the temporary credentials from JSON (example using jq)
creds=$(aws sts assume-role --role-arn arn:aws:iam::123:role/Admin --role-session-name x | jq -r '.Credentials')
export AWS_ACCESS_KEY_ID=$(echo "$creds" | jq -r .AccessKeyId)
export AWS_SECRET_ACCESS_KEY=$(echo "$creds" | jq -r .SecretAccessKey)
export AWS_SESSION_TOKEN=$(echo "$creds" | jq -r .SessionToken)
```

---

## 2) Identity & Accounts

```bash
# Who am I?
aws sts get-caller-identity

# Create an IAM user
aws iam create-user --user-name demo

# Attach policy to user
aws iam attach-user-policy   --user-name demo   --policy-arn arn:aws:iam::aws:policy/PowerUserAccess

# Create an access key (avoid for humans; prefer roles/SSO)
aws iam create-access-key --user-name demo
```

---

## 3) S3 (Storage)

```bash
# Create bucket (note: globally unique name; region-specific)
aws s3api create-bucket --bucket my-demo-bucket --region us-east-1   --create-bucket-configuration LocationConstraint=us-east-1

# Upload/download & sync
aws s3 cp ./file.txt s3://my-demo-bucket/path/file.txt
aws s3 sync ./site s3://my-demo-bucket/site/

# Public read (static hosting via CloudFront recommended instead)
aws s3api put-bucket-policy --bucket my-demo-bucket --policy file://bucket-policy.json

# List and query
aws s3 ls s3://my-demo-bucket
aws s3api list-objects-v2 --bucket my-demo-bucket --query "Contents[].{Key:Key,Size:Size}"

# Delete (permanently removes objects)
aws s3 rm s3://my-demo-bucket --recursive
aws s3api delete-bucket --bucket my-demo-bucket
```

---

## 4) EC2 (Compute)

```bash
# Create key pair & security group
aws ec2 create-key-pair --key-name demo-key --query "KeyMaterial" --output text > demo-key.pem
chmod 600 demo-key.pem

aws ec2 create-security-group --group-name demo-sg --description "demo sg"
aws ec2 authorize-security-group-ingress --group-name demo-sg --protocol tcp --port 22 --cidr 0.0.0.0/0

# Launch an instance
aws ec2 run-instances   --image-id ami-xxxxxxxx   --instance-type t3.micro   --key-name demo-key   --security-groups demo-sg   --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=demo}]'

# List instances with a tag
aws ec2 describe-instances --filters "Name=tag:Name,Values=demo"   --query "Reservations[].Instances[].InstanceId"

# Stop/Start/Terminate
aws ec2 stop-instances --instance-ids i-0123456789abcdef0 --dry-run
aws ec2 start-instances --instance-ids i-0123456789abcdef0
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```

---

## 5) VPC (Networking) – minimal example

```bash
# Create VPC & subnet
aws ec2 create-vpc --cidr-block 10.0.0.0/16
aws ec2 create-subnet --vpc-id vpc-xxxx --cidr-block 10.0.1.0/24

# Internet gateway & route
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-xxxx --internet-gateway-id igw-xxxx
aws ec2 create-route-table --vpc-id vpc-xxxx
aws ec2 create-route --route-table-id rtb-xxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxx
aws ec2 associate-route-table --subnet-id subnet-xxxx --route-table-id rtb-xxxx
```

---

## 6) ECR & Containers

```bash
# Create repo
aws ecr create-repository --repository-name demo

# Login to ECR (Docker required)
aws ecr get-login-password | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com

# Tag & push
docker tag local:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/demo:latest
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/demo:latest
```

---

## 7) ECS / EKS (Containers orchestration)

```bash
# ECS: minimal cluster
aws ecs create-cluster --cluster-name demo-ecs

# EKS: create cluster (control plane; nodes via managed node group)
aws eks create-cluster   --name demo-eks   --role-arn arn:aws:iam::123:role/EKSClusterRole   --resources-vpc-config subnetIds=subnet-1,subnet-2,securityGroupIds=sg-xxx

# Update kubeconfig to interact with cluster
aws eks update-kubeconfig --name demo-eks --region us-east-1
kubectl get nodes
```

---

## 8) Lambda (Serverless)

```bash
# Create IAM role for Lambda (trust policy in trust.json)
aws iam create-role --role-name lambda-basic --assume-role-policy-document file://trust.json
aws iam attach-role-policy --role-name lambda-basic --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

# Create a simple function (zip must contain handler.py with handler)
zip function.zip handler.py
aws lambda create-function   --function-name hello   --runtime python3.11   --role arn:aws:iam::123:role/lambda-basic   --handler handler.handler   --zip-file fileb://function.zip

# Invoke
aws lambda invoke --function-name hello out.json && cat out.json
```

---

## 9) RDS (Database)

```bash
# Create PostgreSQL instance (dev/demo)
aws rds create-db-instance   --db-instance-identifier demo-db   --db-instance-class db.t3.micro   --engine postgres   --allocated-storage 20   --master-username postgres   --master-user-password 'P@ssw0rd!'

# Describe & delete
aws rds describe-db-instances --db-instance-identifier demo-db
aws rds delete-db-instance --db-instance-identifier demo-db --skip-final-snapshot
```

---

## 10) CloudFormation (IaC)

```bash
# Deploy or update a stack
aws cloudformation deploy   --template-file infra.yaml   --stack-name demo-stack   --capabilities CAPABILITY_NAMED_IAM   --parameter-overrides Env=dev ImageTag=latest

# Stack outputs
aws cloudformation describe-stacks --stack-name demo-stack   --query "Stacks[0].Outputs"
```

---

## 11) CloudWatch (Monitoring)

```bash
# Logs
aws logs create-log-group --log-group-name /app/demo
aws logs create-log-stream --log-group-name /app/demo --log-stream-name app-1
# (use put-log-events to push events)

# Metrics list & get values
aws cloudwatch list-metrics --namespace AWS/EC2 --metric-name CPUUtilization
aws cloudwatch get-metric-statistics   --namespace AWS/EC2 --metric-name CPUUtilization   --statistics Average --start-time 2025-08-21T00:00:00Z   --end-time 2025-08-22T00:00:00Z --period 300

# Create alarm
aws cloudwatch put-metric-alarm   --alarm-name cpu-high   --metric-name CPUUtilization   --namespace AWS/EC2   --statistic Average   --period 300   --threshold 80   --comparison-operator GreaterThanThreshold   --evaluation-periods 2   --dimensions Name=InstanceId,Value=i-0123456789abcdef0   --alarm-actions arn:aws:sns:us-east-1:123:sns-topic
```

---

## 12) CloudTrail (Audit)

```bash
aws cloudtrail create-trail --name org-trail --s3-bucket-name org-trail-bucket
aws cloudtrail start-logging --name org-trail
aws cloudtrail lookup-events --lookup-attributes AttributeKey=Username,AttributeValue=your.name
```

---

## 13) Budgets & Cost

```bash
# Create a monthly cost budget (USD)
aws budgets create-budget   --account-id 123456789012   --budget '{
    "BudgetName": "monthly-cost",
    "BudgetType": "COST",
    "TimeUnit": "MONTHLY",
    "BudgetLimit": {"Amount": "50", "Unit": "USD"}
  }'
```

---

## 14) Clean-up (Deletes)

```bash
# EC2
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0

# ECR images & repo
aws ecr batch-delete-image --repository-name demo --image-ids imageTag=latest
aws ecr delete-repository --repository-name demo --force

# ECS cluster
aws ecs delete-cluster --cluster demo-ecs

# EKS cluster
aws eks delete-cluster --name demo-eks

# Lambda
aws lambda delete-function --function-name hello

# RDS
aws rds delete-db-instance --db-instance-identifier demo-db --skip-final-snapshot

# CloudFormation
aws cloudformation delete-stack --stack-name demo-stack

# S3
aws s3 rm s3://my-demo-bucket --recursive
aws s3api delete-bucket --bucket my-demo-bucket
```

---

## Tips
- Prefer **SSO + roles** over long-lived keys.
- Tag everything: `--tags Key=Project,Value=demo` when supported.
- Use `--dry-run` on EC2, and stack policies for CloudFormation safety.
- Script repeatable workflows; store parameters in SSM Parameter Store.
