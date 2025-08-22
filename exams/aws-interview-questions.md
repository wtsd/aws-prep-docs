# 💼 AWS Interview Prep – Q&A

## Beginner Level

**Q1: How would you explain AWS to a non-technical manager?**  
**A:** AWS is like renting IT resources instead of buying hardware. You only pay for what you use, scale instantly, and rely on AWS for reliability and security.  
Example: Instead of buying servers, a startup can launch a website in minutes on AWS.

---

**Q2: How would you secure an S3 bucket containing sensitive customer data?**  
**A:**  
- Block public access.  
- Apply least-privilege IAM policies.  
- Encrypt objects (SSE-KMS).  
- Enable CloudTrail to log access.  
- Use Macie to scan for PII.  
👉 This demonstrates knowledge of security best practices.

---

## Intermediate Level

**Q3: What are the main components of a Well-Architected Framework?**  
**A:** 5 Pillars:  
1. **Operational Excellence** – automate, monitor, improve.  
2. **Security** – IAM, encryption, compliance.  
3. **Reliability** – fault tolerance, recovery.  
4. **Performance Efficiency** – right resources, elasticity.  
5. **Cost Optimization** – reduce waste, choose pricing models.  

---

**Q4: Explain how IAM roles differ from IAM users.**  
**A:**  
- **User:** Long-term identity (username + password/keys).  
- **Role:** Temporary access assumed by users/apps. Roles don’t have long-term credentials.  
👉 Roles are more secure (used in EC2, Lambda, EKS via IRSA).

---

**Q5: How would you migrate a traditional application to AWS?**  
**A:** Steps:  
1. **Assess** – use Migration Hub, TCO calculator.  
2. **Plan** – choose strategy (Rehost, Replatform, Refactor).  
3. **Migrate** – use tools like DMS (databases), Snowball (bulk data).  
4. **Optimize** – scale, add monitoring, security.  
👉 Shows understanding of CAF (Cloud Adoption Framework).

---

## Advanced Level (Scenario-based)

**Q6: Your EC2 instance is compromised. What do you do?**  
**A:**  
1. Isolate instance (security group, NACL).  
2. Snapshot disk for forensics.  
3. Check CloudTrail logs for access history.  
4. Rotate IAM credentials.  
5. Patch vulnerability and redeploy.  
👉 Demonstrates incident response workflow.

---

**Q7: Design a scalable web app on AWS.**  
**A:**  
- **Frontend:** CloudFront + S3 static hosting.  
- **Backend:** ALB → EC2 Auto Scaling group or Lambda.  
- **DB:** RDS Multi-AZ or DynamoDB.  
- **Networking:** VPC with public/private subnets.  
- **Security:** WAF + Shield + IAM roles.  
- **Observability:** CloudWatch, X-Ray, Config.  
👉 This covers performance, reliability, and security pillars.

---

**Q8: How do you connect on-premises data center to AWS?**  
**A:** Options:  
- **VPN (IPSec)** – quick, over Internet.  
- **Direct Connect** – dedicated private line, low latency.  
- **Transit Gateway** – hub for multiple VPCs + on-prem.  
👉 Common interview scenario question.
