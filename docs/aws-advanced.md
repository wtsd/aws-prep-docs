# Advanced AWS Interview Questions with Real-Life Examples

## **1. High Availability & Failover**
**Q:** How would you design a **multi-region failover strategy** for a production application running on AWS?  

**A:**  
- Use **Route 53** with **health checks** to route traffic between primary and secondary regions.  
- Deploy workloads in **multiple AZs** per region (ALB + Auto Scaling groups).  
- Replicate databases using:  
  - **Aurora Global Database** (RPO < 1s, RTO < 1 min).  
  - **DynamoDB Global Tables** for NoSQL.  
- Store static assets in **S3 Cross-Region Replication**.  
- Use **CloudFront** for global edge caching.  

*Real-life example:* At a fintech company, we built active-passive architecture across **us-east-1** and **eu-west-1**. When us-east-1 had an outage, Route 53 failed over in under 2 minutes, ensuring minimal downtime.

---

## **2. Cost Optimization vs Performance**
**Q:** Your team runs hundreds of EC2 instances 24/7, but usage spikes only during business hours. How would you optimize costs?  

**A:**  
- Implement **Auto Scaling** to shut down instances outside peak hours.  
- Use **Savings Plans** or **Reserved Instances** for baseline usage.  
- Run non-critical workloads on **Spot Instances** (e.g., batch jobs).  
- Use **S3 Intelligent-Tiering** for storage cost savings.  
- Continuously monitor with **Cost Explorer + Budgets**.  

*Real-life example:* At a media company, switching part of video transcoding to **Spot Instances** saved **~60% monthly compute costs**.

---

## **3. Security Incident Response**
**Q:** What would you do if you detected **unauthorized API calls** in CloudTrail?  

**A:**  
1. **Immediate actions**:  
   - Revoke compromised IAM keys (via IAM or AWS CLI).  
   - Isolate affected EC2 instances (change SG/NACL).  
2. **Investigate**:  
   - Use **GuardDuty findings** to identify source.  
   - Analyze CloudTrail logs for user identity and actions.  
   - Use **AWS Detective** to correlate suspicious activity.  
3. **Remediate & prevent**:  
   - Rotate credentials.  
   - Enforce **MFA** and **least privilege IAM policies**.  
   - Set up **Security Hub** for continuous monitoring.  

*Real-life example:* During a security drill, we simulated stolen IAM keys. With GuardDuty + Security Hub, the detection latency was **under 5 minutes**. Automated runbooks in Systems Manager disabled the keys and alerted the SOC.

---

## **4. Migration Strategy**
**Q:** How would you migrate a legacy **on-prem monolithic app** to AWS?  

**A:**  
- **Step 1 – Lift & Shift (Rehost):**  
  - Move VMs to EC2 with **AWS Application Migration Service**.  
- **Step 2 – Optimize:**  
  - Use **RDS** for databases instead of self-managed DBs.  
  - Offload static assets to **S3 + CloudFront**.  
- **Step 3 – Modernize:**  
  - Break monolith into **microservices** deployed on **EKS/ECS**.  
  - Use **SQS/SNS** for decoupling.  
  - Adopt **CI/CD pipelines** (CodePipeline/GitHub Actions).  

*Real-life example:* A retail app took 6 months: first rehosted to EC2, then modernized into **ECS + RDS**, cutting infra costs by 35% and improving deployments from weekly → daily.

---

## **5. Networking Troubleshooting**
**Q:** A team reports that an app running in private subnets can’t connect to the Internet. How would you debug?  

**A:**  
- Check **NAT Gateway** or **NAT Instance** exists in a public subnet.  
- Ensure correct **route tables** (private subnet → NAT → IGW).  
- Validate **Security Groups** and **NACLs** (outbound 0.0.0.0/0 allowed).  
- Use **VPC Flow Logs** to confirm dropped packets.  

*Real-life example:* Once, a missing route table entry caused **outbound traffic drops** for a batch-processing app. Fixing the NAT route restored connectivity.

---

## **6. Scaling Stateful Applications**
**Q:** How would you run a **stateful workload** (like Kafka or Elasticsearch) on AWS?  

**A:**  
- Use **Managed Services** when possible:  
  - Amazon MSK (Managed Kafka).  
  - Amazon OpenSearch (Managed Elasticsearch).  
- If self-managed on EKS/EC2:  
  - Attach **EBS volumes** or **EFS** for persistent storage.  
  - Spread brokers across AZs.  
  - Use **Auto Scaling** with careful node count (stateful scaling is complex).  

*Real-life example:* A real-time analytics pipeline used MSK + DynamoDB Streams. Switching from self-managed Kafka on EC2 reduced ops overhead by ~70%.

---

## **7. Multi-Account Strategy**
**Q:** Why would you use **AWS Organizations** instead of running everything in one account?  

**A:**  
- Separate **prod, dev, test** environments for security & billing isolation.  
- Apply **Service Control Policies (SCPs)** to restrict risky actions.  
- Consolidated billing with discounts (RI/SP sharing).  
- Centralize logging/security with **CloudTrail Organization Trails**.  

*Real-life example:* A SaaS company used Organizations to separate client workloads. SCPs prevented engineers from launching unapproved resources, reducing risks.

---

## **8. Disaster Recovery (DR)**
**Q:** Explain different **DR strategies** in AWS.  

**A:**  
1. **Backup & Restore** – Cheapest, longest RTO/RPO.  
2. **Pilot Light** – Minimal core infra running, scale up during DR.  
3. **Warm Standby** – Partially scaled infra running.  
4. **Multi-Site Active-Active** – Fully redundant infra in 2+ regions.  

*Real-life example:* A bank used **Warm Standby** in a second region. RTO ~1 hour, balanced cost and reliability.

@TODO: Add glossary 
