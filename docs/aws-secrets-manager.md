# 🔑 AWS Secrets Manager

## Purpose
AWS Secrets Manager helps you **store, manage, and rotate secrets** securely.  
Secrets can be:
- Database credentials
- API keys
- OAuth tokens
- Any sensitive configuration value

---

## Key Features
- **Automatic Rotation**  
  - Supports RDS, Aurora, Redshift out of the box.  
  - Custom Lambda functions for rotating other types of secrets.
- **Fine-Grained Access Control** with IAM policies.
- **Encryption** with AWS KMS keys (customer-managed or AWS-managed).
- **Auditing** via AWS CloudTrail (who accessed/changed a secret).

---

## Best Practices
- **Rotate secrets regularly** (minimum every 30–60 days).  
- **Use IAM roles** (not hard-coded credentials) to grant applications access.  
- **Limit access** with resource-based policies.  
- **Combine with Parameter Store**:  
  - Use Secrets Manager for sensitive data.  
  - Use Systems Manager Parameter Store for non-sensitive configs.  
- **Monitor with CloudWatch/CloudTrail** to detect unusual access.  

---

## Example: Store & Retrieve a Secret

```bash
# Store a new secret
aws secretsmanager create-secret   --name MyApp/DBPassword   --secret-string '{"username":"admin","password":"S3cr3tP@ssw0rd"}'

# Retrieve secret value
aws secretsmanager get-secret-value   --secret-id MyApp/DBPassword   --query SecretString --output text
```

---

## Example: Automatic Rotation (RDS)
```bash
aws secretsmanager rotate-secret   --secret-id MyApp/DBPassword   --rotation-lambda-arn arn:aws:lambda:us-east-1:123456789012:function:RotateRDSSecret   --rotation-rules AutomaticallyAfterDays=30
```
