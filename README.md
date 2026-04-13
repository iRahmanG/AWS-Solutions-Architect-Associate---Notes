#  AWS Solutions Architect Associate — Study Notes

> Comprehensive, exam-focused notes for the **AWS Certified Solutions Architect – Associate (SAA-C03)** exam.  
> Each module covers core concepts, comparison tables, architecture scenarios, exam traps, and MCQ practice questions.

---

##  Table of Contents

| Module | Topic |
|--------|-------|
| [Module 1](./Module%201%20-%20Systems%20Operation.md) | Systems Operation |
| [Module 2](./Module%202%20-%20Tooling%20%26%20Automation.md) | Tooling & Automation |
| [Module 3](./Module%203%20-%20Computing%20(Servers).md) | Computing — Servers (EC2, AMI, EBS) |
| [Module 4](./Module%204%20%E2%80%94%20Scaling%20%26%20Name%20Resolution.md) | Scaling & Name Resolution (ELB, ASG, Route 53) |
| [Module 5](./Module%205%20%E2%80%94%20Containers%20%26%20Serverless.md) | Containers & Serverless (ECS, EKS, Lambda, Fargate) |
| [Module 6](./Module%206%3A%20Computing%20(Database%20Services).md) | Database Services (RDS, Aurora, DynamoDB, ElastiCache) |
| [Module 7](./Module%207%3A%20AWS%20Networking%20%26%20Amazon%20VPC.md) | AWS Networking & Amazon VPC |
| [Module 8](./Module%208%20-%20AWS%20Storage%20%26%20Archiving.md) | AWS Storage & Archiving (S3, EFS, FSx, Glacier) |
| [Module 9](./Module%209%20%E2%80%94%20Monitoring%20%26%20Security.md) | Monitoring & Security (CloudWatch, GuardDuty, KMS, IAM) |
| [Module 10](./Module%2010%20-%20Managing%20Resource%20Consumption.md) | Managing Resource Consumption (Cost, Budgets, Trusted Advisor) |
| [Module 11](./Module%2011%20%E2%80%94%20Creating%20Automated%20%26%20Repeatable%20Deployments.md) | Automated & Repeatable Deployments (CloudFormation, Elastic Beanstalk) |

---

##  About This Repository

These notes are structured for active exam preparation — not passive reading. Each module is built around the way the SAA-C03 exam actually tests knowledge:

- **Core Concepts** — clear explanations with real-world analogies
- **Comparison Tables** — side-by-side service comparisons (e.g., NAT Gateway vs NAT Instance, RDS vs Aurora vs DynamoDB)
- **Exam Traps** — common misconceptions explicitly called out
- **Architecture Scenarios** — scenario-based questions that mirror the exam format
- **MCQ Practice** — multi-correct and single-answer questions with detailed explanations

---

##  Exam Overview

| Detail | Info |
|--------|------|
| **Exam Code** | SAA-C03 |
| **Format** | 65 questions (multiple choice & multiple response) |
| **Duration** | 130 minutes |
| **Passing Score** | 720 / 1000 |
| **Domains** | Design Secure Architectures, Resilient Architectures, High-Performing Architectures, Cost-Optimized Architectures |

---

##  How to Use These Notes

1. **Go module by module** — the order mirrors the official AWS SAA course structure.
2. **Don't skip the Exam Traps** — they highlight the specific misconceptions AWS tests in every domain.
3. **Attempt MCQs before reading explanations** — cover the answer, think it through, then check.
4. **Revisit comparison tables** — service selection questions make up a large portion of the exam.

---

##  Topics Covered (Key Services)

**Compute:** EC2, AMI, Auto Scaling Groups, Elastic Beanstalk, AWS Batch, Lambda, ECS, EKS, Fargate

**Networking:** VPC, Subnets, IGW, NAT Gateway, NAT Instance, NACLs, Security Groups, VPC Peering, VPC Endpoints, Transit Gateway, Site-to-Site VPN, Direct Connect, Route 53, CloudFront

**Storage:** S3 (storage classes, lifecycle, replication, encryption), EBS, EFS, FSx, Glacier, Storage Gateway

**Databases:** RDS, Aurora, DynamoDB, ElastiCache, Redshift, DMS

**Security:** IAM (users, groups, roles, policies, SCPs), KMS, SSM Parameter Store, Secrets Manager, ACM, WAF, Shield, GuardDuty, Inspector, Macie, AWS Firewall Manager

**Monitoring & Management:** CloudWatch, CloudTrail, AWS Config, Trusted Advisor, Cost Explorer, Budgets

**Deployment:** CloudFormation, Elastic Beanstalk, AWS Batch

**DR & HA:** Backup & Restore, Pilot Light, Warm Standby, Multi-Site Active-Active

---

##  Recommended Study Resources

- [AWS Official SAA-C03 Exam Guide](https://aws.amazon.com/certification/certified-solutions-architect-associate/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Free Tier](https://aws.amazon.com/free/) — hands-on practice
- [AWS Documentation](https://docs.aws.amazon.com/)

---

## License

These notes are shared for educational purposes. Feel free to fork, study, and contribute.

---

> *"The best way to predict the exam is to understand why AWS designed each service the way it did."*
