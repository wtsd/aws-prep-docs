# AWS Best Practices

## Security
- Enable MFA for root and IAM users.
- Use IAM roles instead of long-lived credentials.
- Apply the principle of least privilege.
- Enable AWS CloudTrail in all regions.
- Regularly rotate keys and credentials.

## Cost Optimization
- Use AWS Cost Explorer and Budgets.
- Choose the right pricing model (On-Demand, Reserved, Spot).
- Use AWS Trusted Advisor for cost-saving recommendations.

## Performance Efficiency
- Use Auto Scaling groups for elasticity.
- Choose appropriate instance types for workloads.
- Use caching layers (CloudFront, ElastiCache).

## Reliability
- Design for failure with multi-AZ and multi-region deployments.
- Implement automated backups and disaster recovery.
- Use health checks and monitoring (CloudWatch).

## Operational Excellence
- Automate deployments using Infrastructure as Code (CloudFormation, CDK, Terraform).
- Implement observability (metrics, logs, traces).
- Use CI/CD pipelines for fast, reliable deployments.

---

For extended reference: [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
