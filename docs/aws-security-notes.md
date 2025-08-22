# 🔐 AWS Security Services – Extended Notes

## **AWS Shield**
- **Purpose:** Protect applications against DDoS (Distributed Denial of Service) attacks.  
- **Tiers:**
  - **Shield Standard** (enabled by default, free):  
    Protects against common network and transport layer DDoS attacks (e.g., SYN/UDP floods).
  - **Shield Advanced** (paid):  
    - Additional detection & mitigation for large and sophisticated attacks.  
    - Integrates with AWS WAF for more granular rules.  
    - 24/7 access to the AWS DDoS Response Team (DRT).  
    - Cost protection: AWS credits for scaling costs during an attack.  

🔗 Ref: [AWS Shield Overview](https://aws.amazon.com/shield/)

---

## **AWS WAF (Web Application Firewall)**
- **Purpose:** Protects web applications from common web exploits (OWASP Top 10).  
- **Features:**
  - Blocks SQL injection, cross-site scripting (XSS), and malicious IPs.  
  - Can be deployed with **CloudFront**, **ALB (Application Load Balancer)**, **API Gateway**, or **AppSync**.  
  - Provides **geo-blocking** (restrict access by country/region).  
  - Offers **Managed Rule Groups** from AWS and 3rd parties (pre-configured sets of protections).  
  - Can be integrated with **Shield Advanced** for enhanced DDoS defense.  

🔗 Ref: [AWS WAF](https://aws.amazon.com/waf/)

---

## **Amazon GuardDuty**
- **Purpose:** Threat detection and continuous monitoring for malicious activity.  
- **Key Capabilities:**
  - Detects anomalous API calls, unusual console logins, and suspicious network activity.  
  - Integrates with **VPC Flow Logs**, **CloudTrail**, and **DNS logs**.  
  - Automatically updates detection models using threat intelligence feeds.  
- **Best Practices:**  
  - Enable across **all accounts** using **AWS Organizations**.  
  - Automate response with **Security Hub + EventBridge** (e.g., isolate compromised EC2).  

🔗 Ref: [GuardDuty](https://aws.amazon.com/guardduty/)

---

## **Amazon Inspector**
- **Purpose:** Automated security assessment service.  
- **Types of Assessments:**
  - **EC2 scans**: Looks for OS vulnerabilities, missing patches, and insecure configurations.  
  - **Container images** in **Amazon ECR**: Scans for CVEs (Common Vulnerabilities and Exposures).  
  - **Lambda functions**: Identifies code/package vulnerabilities.  
- **Integrations:**  
  - Connects with **Security Hub**.  
  - Triggers **Systems Manager Automation runbooks** to patch or remediate issues.  

🔗 Ref: [Amazon Inspector](https://aws.amazon.com/inspector/)

---

## **Amazon Macie**
- **Purpose:** Data security & privacy service that uses **machine learning** to discover and classify **sensitive data**.  
- **Use Cases:**  
  - Detects **PII** (names, addresses, credit cards, SSNs, etc.) in S3.  
  - Useful for **compliance audits** (GDPR, HIPAA, PCI DSS).  
  - Generates findings that can be forwarded to **Security Hub**.  
- **Best Practices:**  
  - Schedule **regular scans of S3 buckets**.  
  - Combine with **S3 Object Lock & encryption (KMS)** for strong data protection.  

🔗 Ref: [Macie](https://aws.amazon.com/macie/)

---

## **AWS Systems Manager (SSM)**
- **Purpose:** Unified management for EC2, on-prem servers, and hybrid environments.  
- **Key Features:**
  - **Run Command** – Execute scripts/commands across instances without SSH.  
  - **Patch Manager** – Automate patching across fleets of EC2/OS.  
  - **Parameter Store** – Securely store secrets/config values.  
  - **Application Manager** – View app resources & configs.  
  - **OpsCenter** – Centralized operational issues dashboard.  
  - **Incident Manager** – Incident response playbooks with notifications (integrates with SNS, Slack, PagerDuty).  

🔗 Ref: [AWS Systems Manager](https://aws.amazon.com/systems-manager/)

---

## **AWS CloudTrail**
- **Purpose:** Records every API call in AWS (who did what, when, and where).  
- **Use Cases:**  
  - Security auditing, compliance, and forensics.  
  - Detect unauthorized activity (e.g., unexpected IAM changes).  
- **Best Practices:**  
  - Enable **organization-wide trail** across all accounts.  
  - Send logs to **S3** with **encryption + Object Lock**.  
  - Stream to **CloudWatch Logs** for real-time monitoring.  

🔗 Ref: [CloudTrail](https://aws.amazon.com/cloudtrail/)

---

## **AWS Security Hub**
- **Purpose:** Centralized **dashboard** to view and prioritize security alerts.  
- **Key Points:**  
  - Aggregates findings from **GuardDuty, Inspector, Macie, Firewall Manager, IAM Access Analyzer** and 3rd parties.  
  - Maps findings against **security standards** (CIS AWS Foundations, PCI DSS, NIST CSF).  
  - Can trigger **automated remediations** via **EventBridge + Lambda/SSM**.  

🔗 Ref: [Security Hub](https://aws.amazon.com/security-hub/)

---

## **Amazon Detective**
- **Purpose:** Investigates **root cause** of security issues.  
- **How it Works:**  
  - Automatically collects and organizes logs from **VPC Flow Logs, CloudTrail, GuardDuty**.  
  - Visualizes relationships between resources, users, and activities.  
- **Use Case:**  
  - After GuardDuty raises an alert (e.g., compromised EC2), Detective can show **API calls, IP connections, and timeline** for deeper investigation.  

🔗 Ref: [Amazon Detective](https://aws.amazon.com/detective/)

---

✅ **Recommended Workflow**
1. **Detect & Monitor** → GuardDuty, Inspector, Macie.  
2. **Aggregate & Prioritize** → Security Hub.  
3. **Investigate** → Detective.  
4. **Respond & Remediate** → Systems Manager + EventBridge automations.  
5. **Prevent** → Shield, WAF, IAM best practices.  
