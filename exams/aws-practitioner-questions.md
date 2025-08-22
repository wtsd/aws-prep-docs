# 🎓 AWS Certified Cloud Practitioner – Q&A

## Beginner Level

**Q1: What is cloud computing in AWS?**  
**A:** Cloud computing is the **on-demand delivery of IT resources** (servers, storage, databases, networking, software) over the Internet with **pay-as-you-go pricing**.  
AWS provides global infrastructure where customers can scale resources without investing in physical hardware.

---

**Q2: What is the AWS Free Tier?**  
**A:** A set of limited free resources (for 12 months after account creation, plus always-free offers). Examples:  
- 750 hours/month of **EC2 t2.micro/t3.micro**  
- 5 GB of **S3 storage**  
- 750 hours of **RDS usage**  
👉 Purpose: Let new users learn AWS hands-on.

---

**Q3: What is the difference between EC2 and Lambda?**  
**A:**  
- **EC2:** Virtual servers where you manage OS, runtime, scaling.  
- **Lambda:** Serverless compute service where AWS manages infrastructure; you only provide code and pay per execution.  

---

## Intermediate Level

**Q4: What is the AWS Shared Responsibility Model?**  
**A:**  
- **AWS:** Responsible for **security OF the cloud** (hardware, network, data centers, managed services).  
- **Customer:** Responsible for **security IN the cloud** (applications, data, OS patches, IAM permissions).  
👉 Example: AWS secures S3 infrastructure, but you must configure bucket policies correctly.

---

**Q5: What are Availability Zones (AZs) and Regions?**  
**A:**  
- **Region:** A geographical area (e.g., us-east-1 in Virginia).  
- **AZ:** Independent data centers within a Region (typically ≥3).  
👉 Deploying apps across multiple AZs improves **fault tolerance**.

---

**Q6: What is Amazon CloudFront?**  
**A:**  
A Content Delivery Network (CDN) that caches data closer to users across **edge locations**, reducing latency.  
Commonly used with S3 static websites, streaming media, and API acceleration.

---

## Advanced (Exam-style)

**Q7: How does AWS billing and pricing work?**  
**A:**  
- Pricing models: On-Demand, Reserved Instances, Savings Plans, Spot Instances.  
- Free services: IAM, VPC, CloudFormation.  
- Tools:  
  - **Cost Explorer** – visualize usage.  
  - **Budgets** – alerts on spend.  
  - **Trusted Advisor** – optimization recommendations.  
👉 Example: Long-term stable workloads = Reserved Instances/Savings Plans.  

---

**Q8: How can you secure data in AWS?**  
**A:**  
- **At rest:** S3 SSE (AES-256/KMS), EBS encryption.  
- **In transit:** TLS/SSL.  
- **Access:** IAM roles, least privilege, MFA.  
- **Monitoring:** CloudTrail, Config, GuardDuty.  
- **Detection & prevention:** Macie (PII), WAF, Shield.  

---

**Q9: Which services are serverless in AWS?**  
**A:**  
- Compute: Lambda, Fargate  
- Databases: DynamoDB, Aurora Serverless  
- Integration: SQS, SNS, EventBridge  
- Storage: S3  
👉 Serverless = no server management, pay-per-use, automatic scaling.
