# AWS Module 2: Tooling & Automation — Deep Dive for SAA Exam
## Complete Study Guide + Mind Map + MCQs

---


## 1. AWS Systems Manager (SSM) — Complete Breakdown

AWS Systems Manager is a unified interface for managing your AWS and on-premises infrastructure. Think of it as the "remote control" for your entire fleet of EC2 instances and servers.

### How SSM Works (Architecture)

SSM requires the **SSM Agent** to be installed on instances. The agent communicates with the SSM service endpoint over HTTPS (outbound port 443). This means:
- No inbound ports need to be opened on your security groups
- No SSH keys required for Session Manager
- Works on both Linux and Windows

**IAM prerequisite**: The instance needs an IAM role with the `AmazonSSMManagedInstanceCore` policy attached.

---

### SSM Feature 1 — Automation

Automation lets you run multi-step workflows against AWS resources. Unlike Run Command (which runs on instances), Automation can target AWS API actions too — for example, creating an AMI, stopping instances, patching an AMI.

**Workflow**: Create automation document (SSM Document) → Run it → Monitor execution

**SAA Exam relevance**: If the question asks about automating patching of AMIs, creating golden AMIs, or stopping/starting instances on a schedule — think SSM Automation.

**Exam trap**: Automation ≠ Run Command. Automation targets AWS resources and APIs. Run Command targets the OS inside instances. Don't confuse them.

---

### SSM Feature 2 — Run Command

Run Command lets you remotely execute shell scripts or PowerShell commands across multiple instances simultaneously, without SSH or RDP.

**Key capabilities**:
- Use AWS-provided documents (AWS-RunShellScript, AWS-ApplyPatchBaseline, etc.)
- Target instances by instance ID, tags, or resource groups
- Set rate controls: run on 10% of instances at a time to prevent mass failures
- Set error thresholds: stop if more than 5% fail
- Output goes to S3 or CloudWatch Logs

**Real scenario**: You need to install a security agent on 500 EC2 instances across 3 regions. Use Run Command with a tag-based target and AWS-RunShellScript. No need to SSH into each instance.

**Exam trap**: Run Command does NOT require instances to be in a running state with open SSH ports. It uses the SSM Agent. If a question mentions "without SSH access," Run Command or Session Manager is the answer.

---

### SSM Feature 3 — Session Manager

Session Manager provides browser-based or CLI-based shell access to instances without bastion hosts, open inbound ports, or SSH keys.

**Architecture flow**:
User → AWS Console/CLI → SSM Service → SSM Agent on instance (outbound HTTPS only)

**Security advantages** (critical for exam):
- All session activity is logged to CloudWatch Logs and/or S3
- Eliminates need for bastion hosts (cost saving + security improvement)
- IAM controls who can start sessions
- No need to manage SSH key pairs

**Real scenario**: A company wants to meet a compliance requirement of logging all administrative access to EC2 instances. Session Manager logs every command to CloudWatch Logs automatically.

**Exam trap**: Session Manager sessions use AWS PrivateLink/VPC endpoints if the instances are in private subnets without internet access. You need VPC endpoints for SSM, SSM Messages, and EC2 Messages.

---

### SSM Feature 4 — Patch Manager

Patch Manager automates patching of OS and applications across EC2 and on-premises instances.

**Process flow**:
1. Define a **Patch Baseline** — rules about which patches are approved/rejected
2. Create a **Maintenance Window** — when patching happens
3. Patch Manager scans instances and applies approved patches
4. **Patch Compliance** report shows which instances are compliant

**Default Patch Baselines**: AWS provides default baselines for Amazon Linux, RHEL, Ubuntu, Windows, etc. You can create custom baselines.

**Patch Groups**: Tag instances with `Patch Group` key to associate them with specific baselines. Crucial for separating dev/test/prod patching schedules.

**Real scenario**: A company must patch Windows instances every Sunday at 2am and report compliance to the security team. Use Patch Manager with a Maintenance Window scheduled for Sunday 2am, and export compliance to S3.

**Exam trap**: Patch Manager uses the SSM Agent, NOT AWS Inspector. Inspector scans for vulnerabilities but doesn't patch. Patch Manager patches. Know this distinction.

---

### SSM Feature 5 — Maintenance Windows

Maintenance Windows define a schedule during which SSM can run tasks (Run Command, Automation, Lambda, Step Functions) against targets.

**Components**:
- Schedule (cron or rate expression)
- Duration and cutoff (stop starting new tasks X hours before window closes)
- Targets (instances, resource groups)
- Tasks (what to run)

**Exam trap**: Maintenance Windows are NOT just for patching. They can run any SSM task. They're also independent of Patch Manager — you assign Patch Manager as a task inside a Maintenance Window.

---

### SSM Feature 6 — State Manager

State Manager ensures instances are in a defined, consistent state. It's declarative — you define the desired state and SSM continuously enforces it.

**Use cases**:
- Ensure antivirus is always installed
- Keep CloudWatch agent configured correctly
- Prevent configuration drift

**How it works**: Associate a document with instances + set a schedule. SSM re-applies the configuration on that schedule.

**Exam trap**: State Manager = prevent configuration drift on EC2. AWS Config = detect configuration drift on AWS resources/services. Don't confuse these.

---

### SSM Feature 7 — Parameter Store

Parameter Store is a secure, hierarchical storage for configuration data and secrets. It integrates with KMS for encryption.

**Two tiers**:
- Standard: free, up to 10,000 parameters, 4KB max size
- Advanced: paid, up to 100,000 parameters, 8KB max, parameter policies (expiration/notification)

**Parameter types**:
- String
- StringList
- SecureString (KMS-encrypted)

**Hierarchy example**: `/prod/database/password` — apps reference by path, enabling environment separation.

**Exam trap 1**: Parameter Store vs Secrets Manager. Both store secrets, but Secrets Manager has automatic rotation built-in (for RDS, Redshift, DocumentDB). Parameter Store requires Lambda for rotation. If the question says "automatic rotation," use Secrets Manager. If the question says "cost-effective secrets/config storage," use Parameter Store.

**Exam trap 2**: SecureString uses KMS. You need `kms:Decrypt` permission to retrieve a SecureString parameter.

---

### SSM Feature 8 — Inventory

Inventory collects metadata from instances: installed applications, OS details, network configuration, Windows services, server roles, files, registry (Windows), custom inventory.

Inventory data is stored in S3 and can be queried with Athena or Config.

---

### SSM Feature 9 — Insights Dashboard

Aggregates operational data across resource groups. Pulls from CloudTrail, Config, Trusted Advisor, Personal Health Dashboard, and CloudWatch.

---

## 2. AWS Tools for PowerShell

AWS Tools for PowerShell is a set of PowerShell cmdlets built on top of the AWS SDK for .NET. It maps directly to AWS API actions.

**Key points for SAA**:
- Runs on Windows PowerShell and PowerShell Core (cross-platform)
- Each cmdlet corresponds to an AWS API call
- Uses the same credential chain as AWS CLI (IAM roles, profiles, environment variables)
- Great for Windows administrators who prefer PowerShell over bash

**When to choose PowerShell vs CLI**: In exam scenarios with "Windows administrators" or "scripting from PowerShell," the answer is AWS Tools for PowerShell. For general scripting and CI/CD pipelines on Linux, use AWS CLI.

---

## 3. AWS SDKs

SDKs enable you to integrate AWS services into your applications using native language constructs.

**Available SDKs**: Java, Python (boto3), JavaScript/Node.js, .NET, PHP, Ruby, Go, C++

**SAA exam relevance**: SDKs are rarely tested deeply on SAA, but you should know:
- Applications use SDKs to call AWS services programmatically
- SDKs use IAM credentials (instance profile, environment variables, credential file)
- SDKs automatically retry failed API calls with exponential backoff

---

## 4. AWS CloudFormation — Deep Dive

CloudFormation is Infrastructure as Code (IaC) — you define your entire AWS environment in a template and CloudFormation creates/updates/deletes it as a unit called a **Stack**.

### Template Structure

A CloudFormation template has these sections:
- **AWSTemplateFormatVersion** (optional)
- **Description** (optional)
- **Parameters** — inputs at stack creation time
- **Mappings** — key-value lookup tables (e.g., AMI IDs per region)
- **Conditions** — conditional resource creation
- **Resources** (required) — AWS resources to create
- **Outputs** — values exported from the stack

### Key CloudFormation Concepts

**Change Sets**: Preview changes before applying them. Shows what will be added, modified, or deleted. Critical for production environments. Use when the question asks "how do you safely update a production stack."

**Stack Policies**: Protect specific resources from updates/deletions within a stack. For example, prevent RDS databases from being replaced during a stack update.

**Drift Detection**: Compare the current live state of stack resources against the template definition. If someone manually changed a security group rule outside CloudFormation, drift detection catches it.

**Nested Stacks**: Stacks that reference other stacks. Used to manage large, complex environments. A root stack creates child stacks using `AWS::CloudFormation::Stack` resources.

**StackSets**: Deploy stacks across multiple AWS accounts and regions from a single operation. Used for organizations with many accounts.

**Rollback**: If a stack creation/update fails, CloudFormation automatically rolls back to the last successful state.

**DependsOn**: Explicitly declare resource creation order when CloudFormation can't infer it automatically.

**CloudFormation Init (cfn-init)**: Used to bootstrap EC2 instances — install packages, create files, run commands during instance launch via UserData + cfn-init.

**Exam trap 1**: CloudFormation is NOT for deploying applications (use CodeDeploy). It's for provisioning infrastructure.

**Exam trap 2**: If a stack update fails and rollback is triggered, resources that were already created in the failed update get deleted. This is important for understanding rollback behavior.

**Exam trap 3**: CloudFormation doesn't support all resource types natively. For unsupported resources, use **Custom Resources** (backed by Lambda).

---

## 5. AWS OpsWorks — Deep Dive

OpsWorks is a configuration management service based on Chef and Puppet. It enables infrastructure as code through Chef recipes and Puppet manifests.

**Three offerings**:

1. **OpsWorks for Chef Automate** — fully managed Chef Automate server. Use existing Chef cookbooks. No need to manage Chef infrastructure.

2. **OpsWorks for Puppet Enterprise** — fully managed Puppet Enterprise master. Use existing Puppet modules.

3. **OpsWorks Stacks** — AWS's own stack-based deployment model. Define stacks with layers, instances, and apps. Uses Chef Solo recipes. (Note: OpsWorks Stacks is being retired — AWS recommends migrating to Systems Manager or CloudFormation.)

**When to choose OpsWorks on SAA exam**: If the question mentions "Chef" or "Puppet" in the context of configuration management, the answer is OpsWorks. If it mentions "existing Chef cookbooks," it's specifically OpsWorks for Chef Automate.

**Exam trap**: OpsWorks Stacks ≠ CloudFormation. OpsWorks manages the configuration and deployment lifecycle (install, configure, deploy, undeploy, shutdown). CloudFormation manages infrastructure provisioning. They complement each other.

---

## 6. Amazon S3 Static Website Hosting — Deep Dive

Amazon S3 can serve static content (HTML, CSS, JavaScript, images) directly to end users, without any web servers.

### Setup Steps

1. Create an S3 bucket (bucket name must match the custom domain name if using Route 53)
2. Enable static website hosting on the bucket
3. Set bucket policy to allow public read access (`s3:GetObject`)
4. Upload content (index document + error document)
5. Access via S3 website endpoint

### S3 Website Endpoint Formats

Two formats exist depending on region:
- `http://bucket-name.s3-website-us-east-1.amazonaws.com` (dash)
- `http://bucket-name.s3-website.eu-west-1.amazonaws.com` (dot)

**Important**: The S3 website endpoint only supports HTTP, not HTTPS directly. To add HTTPS, put CloudFront in front.

### Custom Domain with Route 53

1. Register domain in Route 53 (or transfer)
2. Create S3 bucket with exact same name as domain (e.g., `www.example.com`)
3. Create Route 53 Alias record pointing to S3 website endpoint
4. For apex domain redirect: create a second bucket `example.com` with redirect to `www.example.com`

### S3 Static Hosting vs EC2 Web Server

| Feature | S3 Static Hosting | EC2 Web Server |
|---|---|---|
| Cost | Very low (per request/storage) | EC2 + EBS cost always running |
| Scalability | Infinite, automatic | Manual scaling |
| Dynamic content | No (HTML/CSS/JS only) | Yes (PHP, Node.js, etc.) |
| HTTPS | Needs CloudFront | Native with certificate |
| Database | No | Yes |

**Exam trap 1**: S3 static hosting cannot serve dynamic content (no server-side code). For dynamic content, use EC2 or Lambda + API Gateway.

**Exam trap 2**: The S3 website endpoint is different from the REST API endpoint. The website endpoint supports index documents and error documents. The REST endpoint does not.

**Exam trap 3**: Bucket name must match the domain name exactly when using Route 53 Alias records. A bucket named `mysite` can't be aliased to `www.mysite.com`.

**Exam trap 4**: You need to disable "Block all public access" setting on the bucket before making objects publicly accessible. This is a common gotcha in the real world too.

**Exam trap 5**: S3 Transfer Acceleration uses CloudFront edge locations for faster uploads. This is different from CloudFront for delivery.

---

## Key Architecture Patterns for SAA Exam

**Pattern 1 — Patch management at scale**: EC2 Fleet → SSM Patch Manager + Maintenance Windows → Patch Baseline → Patch Compliance → S3/CloudWatch

**Pattern 2 — Secure admin access**: Developer → Session Manager (no SSH, logged to CloudWatch) → Private EC2 instances (no bastion host needed)

**Pattern 3 — Configuration consistency**: EC2 instances → SSM State Manager → desired state document → enforce on schedule

**Pattern 4 — Secrets management**: Application on EC2 → SSM Parameter Store (SecureString) → KMS decryption → database password retrieved at runtime

**Pattern 5 — Static website with HTTPS**: S3 bucket (static content) → CloudFront distribution (HTTPS, edge caching) → Route 53 (custom domain)

**Pattern 6 — Infrastructure deployment**: CloudFormation template (JSON/YAML) → Stack → multi-service deployment → Change Sets for safe updates → Drift Detection for compliance

---

## Critical Comparison Tables

**SSM vs Other Services**:

| Need | Use This |
|---|---|
| Run commands on instances without SSH | SSM Run Command |
| Shell access without bastion/SSH key | SSM Session Manager |
| Patch OS automatically on schedule | SSM Patch Manager + Maintenance Windows |
| Store config values securely | SSM Parameter Store |
| Automatic secret rotation | Secrets Manager |
| Detect drift on AWS resources | AWS Config |
| Prevent config drift on EC2 OS | SSM State Manager |
| Deploy infrastructure as code | CloudFormation |
| Configuration management (Chef/Puppet) | OpsWorks |
| Vulnerability scanning | Amazon Inspector |

---

## Exam Traps Summary

1. SSM Session Manager needs VPC endpoints if instances are in private subnets without NAT
2. Parameter Store SecureString requires KMS — you need `kms:Decrypt` permission
3. Patch Manager ≠ Inspector (scan vs patch)
4. State Manager (EC2 config drift) ≠ Config (AWS resource drift)
5. CloudFormation Change Sets preview changes — use before production updates
6. S3 website endpoint = HTTP only; add CloudFront for HTTPS
7. S3 bucket name must match domain for Route 53 alias
8. OpsWorks = Chef/Puppet (configuration management), not infrastructure provisioning
9. Run Command targets instance OS; Automation targets AWS APIs too
10. Secrets Manager has built-in rotation; Parameter Store needs Lambda for rotation

---

# 20 Scenario-Based MCQs

---

**Q1.** A company runs 2,000 Windows EC2 instances across 5 AWS regions. Their security team requires that all instances be patched with approved security patches every Saturday between 1am–3am, and that a compliance report be generated automatically. The company does NOT want to open any inbound ports on their instances. Which combination of services meets ALL requirements?

- A) AWS Inspector + Scheduled Lambda function + CloudWatch Events
- B) SSM Patch Manager + SSM Maintenance Windows + SSM Patch Compliance + CloudWatch Logs
- C) SSM Run Command with AWS-ApplyPatchBaseline + EventBridge scheduled rule
- D) AWS Config + Systems Manager Automation + SNS notifications
- E) Both B and C achieve this, but B is more operationally efficient

**Correct: E**

Explanation: Both B and C technically work. B uses the native Patch Manager workflow (most appropriate). C manually triggers patch application via Run Command on a schedule. The question asks which "meets ALL requirements" — both do since SSM Agent uses outbound HTTPS only (no inbound ports). E is correct because B is the designed approach. Exam trap: Don't dismiss C as wrong — Run Command CAN apply patch baselines. But the "complete solution" with compliance reporting is best handled by Patch Manager natively.

---

**Q2.** A solutions architect needs to allow developers to access EC2 instances in private subnets for debugging, without exposing bastion hosts, without managing SSH keys, and while logging all commands for a compliance audit. The instances do NOT have internet access. What is the MINIMUM configuration required?

A) Create a bastion host in a public subnet and configure CloudTrail
B) Enable SSM Session Manager and create VPC endpoints for SSM, SSM Messages, and EC2 Messages; configure CloudWatch Logs for session logging
C) Enable SSM Session Manager with an Internet Gateway and configure S3 bucket for logs
D) Use EC2 Instance Connect and send logs to CloudTrail
E) Use AWS Systems Manager with only the SSM VPC endpoint

**Correct: B**

Explanation: Session Manager for private instances without internet requires THREE VPC interface endpoints: `com.amazonaws.region.ssm`, `com.amazonaws.region.ssmmessages`, and `com.amazonaws.region.ec2messages`. Option C is wrong (internet gateway defeats the purpose of private subnet). Option E is wrong (one endpoint is insufficient). Option D doesn't log commands. Exam trap: Many candidates forget that Session Manager needs 3 VPC endpoints, not just 1.

---

**Q3.** A company stores database credentials in AWS SSM Parameter Store as SecureString parameters. An EC2 instance running an application needs to retrieve these credentials at startup. The application is failing with an "AccessDeniedException" error. What are the TWO most likely causes?

A) The EC2 instance does not have an IAM role attached
B) The IAM role lacks `ssm:GetParameter` permission
C) The parameter is stored in the wrong AWS region
D) The IAM role lacks `kms:Decrypt` permission for the KMS key used to encrypt the SecureString
E) SSM Parameter Store does not support SecureString parameters on EC2

**Correct: A and D** (or B and D)

Explanation: To retrieve a SecureString parameter you need BOTH: (1) `ssm:GetParameter` permission AND (2) `kms:Decrypt` permission for the KMS key. The most common mistake is granting SSM permission but forgetting KMS. If there's no IAM role at all (A), both permissions are missing. Exam trap: People grant `ssm:GetParameter` but forget `kms:Decrypt` — this is the #1 Parameter Store gotcha on the exam.

---

**Q4.** A company wants to host a static marketing website for global customers. The solution must support HTTPS, serve content with low latency worldwide, minimize operational overhead, and remain cost-effective. The website contains only HTML, CSS, images, and JavaScript. Which architecture is BEST?

A) EC2 Auto Scaling Group + Application Load Balancer + Route 53
B) S3 static website hosting + CloudFront distribution with SSL/TLS certificate from ACM + Route 53
C) S3 static website hosting + Route 53 with latency-based routing
D) Elastic Beanstalk + CloudFront + Route 53
E) Lambda@Edge + S3 + CloudFront

**Correct: B**

Explanation: S3 + CloudFront is the canonical serverless static website architecture. CloudFront provides HTTPS via ACM certificate (free), global edge caching for low latency, and origin being S3. Route 53 provides custom domain. Option C is wrong because S3 website endpoint only supports HTTP (no HTTPS). Option A/D have unnecessary operational overhead for static content. Option E adds Lambda@Edge unnecessarily for content that doesn't require dynamic processing. Exam trap: S3 website endpoint = HTTP only. ALWAYS add CloudFront for HTTPS.

---

**Q5.** A company is deploying infrastructure for a multi-tier web application using CloudFormation. During a stack update to add a new RDS instance, the CloudFormation update fails at the EC2 Auto Scaling Group modification step. What happens to the EXISTING RDS instance that was SUCCESSFULLY created before the failure?

A) The RDS instance remains running — partial updates are kept
B) CloudFormation rolls back the entire stack, deleting the new RDS instance
C) CloudFormation marks the stack as UPDATE_ROLLBACK_FAILED and requires manual intervention
D) The RDS instance is stopped but not deleted
E) CloudFormation creates a snapshot of the RDS instance before deleting it

**Correct: B**

Explanation: When a CloudFormation update fails, it automatically attempts to roll back to the last successful state. Resources that were newly created during the failed update (the new RDS instance) are deleted during rollback. Exam trap: Many candidates think partial updates are kept. They're not — rollback means the entire stack returns to its previous state. Option C (UPDATE_ROLLBACK_FAILED) can happen if the rollback itself fails, but the default behavior is automatic rollback with deletion of newly created resources.

---

**Q6.** A security team needs to ensure that all EC2 instances in a production account always have the CloudWatch agent installed and properly configured, even if an administrator manually removes it. Which service provides the MOST appropriate solution?

A) AWS Config with auto-remediation using SSM Automation
B) SSM State Manager with a scheduled association
C) SSM Run Command with an EventBridge scheduled rule every hour
D) CloudFormation with drift detection
E) Both A and B are equally appropriate

**Correct: B**

Explanation: SSM State Manager is specifically designed for maintaining desired configuration state on EC2 instances. It continuously enforces the association on a defined schedule. When the agent is removed, the next scheduled execution re-installs it. Option A (Config + remediation) works but is designed for AWS resource compliance, not EC2 OS-level configuration. Option C could work but is not the designed pattern — State Manager exists precisely for this use case. Exam trap: State Manager = enforce desired state on EC2. Config = detect/remediate AWS resource configuration.

---

**Q7.** An e-commerce company wants to run automated tasks (backup snapshots, scale down dev instances) every weeknight between 11pm–midnight. The tasks should stop starting new executions 15 minutes before midnight. They need to verify task completion status. Which SSM feature is DESIGNED for this use case?

A) SSM Run Command with EventBridge cron rule
B) SSM Automation with a CloudWatch Events schedule
C) SSM Maintenance Windows with duration=1hr and cutoff=15min
D) AWS Lambda triggered by EventBridge
E) SSM State Manager with a rate expression

**Correct: C**

Explanation: SSM Maintenance Windows is the designed service for scheduled maintenance tasks with a defined window, duration, and cutoff. The cutoff ensures no new tasks start within 15 minutes of window close. It tracks task completion status natively. Option A would work technically but doesn't have built-in cutoff or completion tracking. Option E (State Manager) is for configuration enforcement, not one-time scheduled tasks. Exam trap: Maintenance Windows have a CUTOFF (stop starting tasks N hours before end) and DURATION — these are key features not available in plain EventBridge triggers.

---

**Q8.** A company is migrating from a traditional data center. They use Chef cookbooks extensively for server configuration management and have invested 3 years in developing them. They want to use AWS but leverage their existing Chef investments with minimal changes. Which service should they use?

A) AWS Systems Manager State Manager
B) AWS CloudFormation with cfn-init
C) AWS OpsWorks for Chef Automate
D) AWS OpsWorks Stacks
E) AWS Elastic Beanstalk

**Correct: C**

Explanation: OpsWorks for Chef Automate is a fully managed Chef Automate server on AWS that allows you to reuse existing Chef cookbooks without modification. Option D (OpsWorks Stacks) uses Chef Solo and has limitations compared to full Chef Automate. Option A (State Manager) requires SSM Documents, not Chef cookbooks. Exam trap: OpsWorks Stacks ≠ OpsWorks for Chef Automate. If the question says "existing Chef cookbooks" and "fully managed," it's Chef Automate, not Stacks.

---

**Q9.** A developer stores an API key in SSM Parameter Store as a SecureString. Another team member stored a database password in Secrets Manager. A compliance requirement says all secrets must be rotated every 90 days automatically without application changes. The database is Amazon RDS. Which statement is CORRECT?

A) Both services support automatic rotation natively for all secret types
B) SSM Parameter Store supports automatic rotation; Secrets Manager does not
C) Secrets Manager supports automatic rotation for RDS; SSM Parameter Store requires a Lambda function for rotation
D) Neither service supports automatic rotation; you must use AWS Key Management Service
E) SSM Parameter Store Advanced tier supports automatic rotation natively

**Correct: C**

Explanation: Secrets Manager has BUILT-IN automatic rotation for RDS, Redshift, DocumentDB, and other supported services. SSM Parameter Store does NOT have native rotation — you must create a Lambda function to rotate the secret. For the API key in Parameter Store, the team needs to write a Lambda. For the RDS password in Secrets Manager, rotation is built-in. Exam trap: Parameter Store Advanced tier has "parameter policies" (expiration notifications) but NOT automatic rotation. Many candidates confuse parameter policies with rotation.

---

**Q10.** A company has a CloudFormation stack managing production infrastructure. They want to update the stack to change the RDS instance type from db.t3.medium to db.r5.large. Before applying the change, they want to see exactly which resources will be modified, replaced, or deleted. What should they do?

A) Run the stack update and monitor the Events tab in CloudFormation console
B) Enable CloudFormation Drift Detection before the update
C) Create a Change Set for the stack update and review it before execution
D) Deploy the changes to a staging stack first using StackSets
E) Use CloudTrail to preview the API calls CloudFormation will make

**Correct: C**

Explanation: Change Sets allow you to preview proposed changes to a stack (add/modify/replace/delete) before executing them. This is the exact use case described — "see what will change before applying." Option B (drift detection) shows differences between current state and template, not future changes. Option A is too late — changes are already happening. Exam trap: Changing an RDS instance type is a REPLACEMENT operation in CloudFormation (the old instance is deleted and a new one is created), which means DATA LOSS unless you have snapshots. A Change Set would reveal the "Replacement: True" indicator — this is critical knowledge.

---

**Q11.** A company's S3 static website is accessible at `http://www.example.com.s3-website-us-east-1.amazonaws.com` but NOT at `https://www.example.com`. Users are reporting that some corporate firewalls block HTTP traffic. What is the MOST appropriate solution with LEAST operational overhead?

A) Enable S3 Transfer Acceleration on the bucket
B) Create a CloudFront distribution with the S3 bucket as origin, request an ACM certificate, and update Route 53
C) Migrate the website to EC2 with nginx and an Application Load Balancer
D) Enable S3 Requester Pays for the bucket
E) Use AWS Certificate Manager to attach an SSL certificate directly to the S3 bucket

**Correct: B**

Explanation: S3 website endpoints do NOT support HTTPS natively. You must add CloudFront, which can use ACM certificates (free for CloudFront use) to serve content over HTTPS. ACM certificates cannot be attached directly to S3 buckets (Option E is invalid). Option A (Transfer Acceleration) is for faster uploads, not HTTPS. Option C has high operational overhead for a static website. Exam trap: ACM certificates can be attached to CloudFront, ALB, and API Gateway — but NOT directly to S3 buckets.

---

**Q12.** An application needs to retrieve different database connection strings depending on whether it's running in dev, staging, or production environments. The strings must be encrypted at rest and the application must be able to retrieve the correct string without hardcoding environment-specific logic. Which approach BEST achieves this?

A) Store all strings in an S3 bucket with SSE-S3 encryption and use bucket policies
B) Use SSM Parameter Store with hierarchical naming like `/dev/db/connection`, `/staging/db/connection`, `/prod/db/connection` with SecureString type, and use the environment prefix at runtime
C) Use AWS Secrets Manager with separate secrets per environment and Lambda rotation
D) Store strings in AWS KMS as encrypted data keys
E) Use environment variables in EC2 launch configurations per environment

**Correct: B**

Explanation: SSM Parameter Store with hierarchical paths is purpose-built for this pattern. The app reads its environment name (from instance tags or an environment variable) and constructs the parameter path at runtime — no hardcoded strings. SecureString provides KMS encryption. This is cost-effective and operationally clean. Option C works but Secrets Manager is overkill (and more expensive) if you don't need rotation. Option E requires separate launch configs per environment and doesn't encrypt values. Exam trap: Parameter Store is the preferred answer for "environment-specific config values with encryption" unless automatic rotation is required.

---

**Q13.** A company deploys infrastructure using CloudFormation. After several manual changes made by operations staff directly in the AWS Console, the actual infrastructure differs from the CloudFormation template. The team wants to identify ALL differences between the live infrastructure and the template definition. Which feature addresses this?

A) CloudFormation Change Sets
B) AWS Config Configuration Recorder
C) CloudFormation Drift Detection
D) AWS Trusted Advisor
E) AWS Systems Manager Inventory

**Correct: C**

Explanation: CloudFormation Drift Detection compares the current live state of stack resources against the template. It identifies properties that have been manually changed outside CloudFormation. Option A (Change Sets) previews future changes, not current differences. Option B (Config) records resource configuration history but isn't specific to CloudFormation stacks. Exam trap: Change Sets = future changes preview. Drift Detection = current state vs template comparison. These are commonly confused.

---

**Q14.** A company has 500 EC2 instances and needs to run a custom Python script on all instances to collect application-specific metrics. The script must run WITHOUT logging into each instance, results must be stored in S3, and failed executions on individual instances must NOT stop execution on the remaining instances. Which service and configuration achieves this?

A) SSM Run Command with AWS-RunPythonScript document, S3 output, and error threshold set to 100%
B) SSM Automation with a custom automation document
C) AWS Lambda triggered by EventBridge with EC2 metadata permissions
D) AWS Batch with a managed compute environment
E) SSM State Manager with a Python script association

**Correct: A**

Explanation: SSM Run Command with AWS-RunShellScript (or AWS-RunPythonScript) is the correct approach. Setting the error threshold to 100% means execution continues even if some instances fail. S3 output stores results. No SSH required (uses SSM Agent). Option E (State Manager) continuously enforces configuration — not for one-time script execution. Option B (Automation) is for AWS API-level tasks, not for running arbitrary scripts inside instances. Exam trap: SSM Run Command has RATE CONTROL (how many instances to run on simultaneously) and ERROR THRESHOLD (stop if X% fail) — knowing these parameters is key.

---

**Q15.** A startup wants to deploy a complete web application stack (VPC, subnets, EC2, RDS, ALB, Security Groups) using infrastructure as code. They want to be able to recreate the IDENTICAL environment in different AWS regions and tear it all down as a single operation. Which service is MOST appropriate?

A) AWS Elastic Beanstalk
B) AWS OpsWorks Stacks
C) AWS CloudFormation with parameterized templates
D) AWS Systems Manager Automation
E) AWS CodeDeploy

**Correct: C**

Explanation: CloudFormation with parameterized templates is the designed solution for repeatable, regional infrastructure deployment as a single stack unit. Parameters allow region-specific values (AMI IDs, VPC CIDR). Stack deletion tears down everything. Option A (Beanstalk) is for application deployment, not arbitrary infrastructure. Option D (Automation) is for operational tasks, not initial infrastructure provisioning. Option E (CodeDeploy) is for application deployment. Exam trap: Elastic Beanstalk creates infrastructure, but you have less control and can't customize VPC/subnet architecture as precisely. CloudFormation is the answer when you need full infrastructure control.

---

**Q16.** A company's CloudFormation stack contains an RDS database and an EC2 instance. They realize a developer accidentally deployed a stack without a deletion policy, and they need to delete the stack for cost savings. However, they want to PRESERVE the RDS data. What should they do BEFORE deleting the stack?

A) Create a CloudFormation Change Set to remove only the EC2 instance
B) Add a DeletionPolicy: Snapshot or Retain attribute to the RDS resource in the template, then update the stack, then delete
C) Manually create an RDS snapshot, then delete the stack — the snapshot persists independently
D) Use CloudFormation StackSets to migrate the RDS to another stack
E) Both B and C are valid approaches

**Correct: E**

Explanation: Both approaches work. Option B adds a `DeletionPolicy: Snapshot` to the RDS resource before stack deletion — CloudFormation will take a snapshot before deleting the RDS instance. Option C is simpler — create a manual snapshot (which lives independently of the stack), then delete the stack. The manual snapshot survives stack deletion. Exam trap: Without a DeletionPolicy, RDS is DELETED when the stack is deleted with no automatic backup. Default deletion policies: most resources are deleted, but some (like S3 buckets with objects) fail deletion. You must know which resources have special deletion behaviors.

---

**Q17.** A company wants to implement SSM Session Manager for their EC2 instances in private subnets. The instances use IMDSv2 and have no internet connectivity. An engineer sets up Session Manager but connections fail. Which combination of actions will MOST LIKELY fix the issue? (Select TWO)

A) Add a NAT Gateway to the private subnet
B) Create VPC interface endpoints for `ssm`, `ssmmessages`, and `ec2messages`
C) Open inbound port 443 on the instance security group
D) Attach an IAM role with `AmazonSSMManagedInstanceCore` policy to the instances
E) Enable VPC Flow Logs on the subnet

**Correct: B and D**

Explanation: For Session Manager to work in private subnets without internet: (1) VPC endpoints for the three SSM services (B) allow the SSM Agent to communicate with SSM service without internet. (2) The instance needs `AmazonSSMManagedInstanceCore` IAM policy (D) to authenticate to SSM. Option A (NAT Gateway) would work but is more expensive and less secure. Option C (inbound 443) is incorrect — SSM uses OUTBOUND connections from the agent, not inbound. Option E (Flow Logs) is for monitoring, not connectivity. Exam trap: Session Manager uses OUTBOUND HTTPS from the instance. Never open inbound ports for SSM.

---

**Q18.** A company stores application configuration in SSM Parameter Store. They have 15,000 parameters and need some parameters to automatically expire after 90 days, triggering an EventBridge event for rotation. Standard Tier parameters are currently in use. What change is required?

A) Migrate to Secrets Manager for expiration support
B) Upgrade to SSM Parameter Store Advanced Tier to enable parameter policies
C) Create a Lambda function triggered by EventBridge every day to check parameter ages
D) Use SSM Maintenance Windows to delete expired parameters
E) No change needed — Standard Tier supports parameter policies

**Correct: B**

Explanation: Parameter policies (expiration, expiration notification, no-change notification) are ONLY available in Advanced Tier. Standard Tier supports up to 10,000 parameters (this company has 15,000 — already requiring Advanced Tier for count alone). Advanced Tier also unlocks parameter policies for automatic expiration. Option C works but is unnecessarily complex when the native feature exists. Exam trap: Standard Tier limit is 10,000 parameters. 15,000 parameters already requires Advanced Tier. Parameter policies are an Advanced Tier ONLY feature.

---

**Q19.** A team is using CloudFormation to manage a stack that includes an S3 bucket. When they attempt to delete the stack, the deletion fails with an error. The S3 bucket contains objects. What is the CORRECT explanation and solution?

A) CloudFormation cannot delete S3 buckets — you must exclude them from the stack
B) The S3 bucket must be emptied before CloudFormation can delete it; the team must empty the bucket first, then retry stack deletion
C) Add DeletionPolicy: Delete to the S3 resource in the template
D) Both B and C are valid — B is a manual workaround, C makes it automatic but requires first emptying the bucket
E) Create a custom resource backed by Lambda to empty and delete the bucket

**Correct: D** (with B being the immediate fix and E being the automated long-term solution, but D as presented in the options is most accurate)

Explanation: CloudFormation cannot delete an S3 bucket that contains objects — this is a hard AWS constraint. The only options are: (1) manually empty the bucket and retry (B), (2) use a Custom Resource backed by Lambda that empties the bucket programmatically before deletion (E). Adding `DeletionPolicy: Delete` alone doesn't empty the bucket first. Exam trap: S3 bucket deletion in CloudFormation fails if the bucket has objects, REGARDLESS of DeletionPolicy. This is a very common real-world and exam trap scenario.

---

**Q20.** A company has a multi-account AWS organization. They need to deploy a baseline CloudFormation stack (VPC, security groups, IAM roles) to ALL 50 accounts in the organization automatically, including new accounts that join in the future. Which feature meets this requirement with LEAST operational overhead?

A) CloudFormation Nested Stacks deployed from the management account
B) AWS CloudFormation StackSets with AWS Organizations integration and automatic deployment enabled
C) AWS Service Catalog with a portfolio shared to all accounts
D) AWS Control Tower with Account Vending Machine
E) CloudFormation Change Sets deployed via CodePipeline to each account

**Correct: B**

Explanation: CloudFormation StackSets with AWS Organizations integration can deploy stacks to ALL accounts in an OU or entire organization. The "automatic deployment" setting automatically deploys to NEW accounts when they join the organization — exactly what's required. Option A (Nested Stacks) doesn't cross account boundaries. Option D (Control Tower) is for account governance, not just stack deployment. Option E is overly complex. Exam trap: StackSets require TRUST between the management account (administrator) and target accounts (stack instance accounts). With Organizations integration, this trust is set up automatically. Without Organizations integration, you must manually set up trust in each account.

---

# Quick Reference Card

**Service → Exam Keyword Mapping**:
- "without SSH / without bastion" → SSM Session Manager
- "patch at scale / scheduled patching" → SSM Patch Manager + Maintenance Windows
- "config drift on EC2" → SSM State Manager
- "run scripts remotely" → SSM Run Command
- "hierarchical config / secrets" → SSM Parameter Store
- "automatic secret rotation" → Secrets Manager
- "Chef / Puppet" → AWS OpsWorks
- "infrastructure as code / entire stack" → CloudFormation
- "preview infrastructure changes" → CloudFormation Change Sets
- "detect manual changes to stack" → CloudFormation Drift Detection
- "deploy to multiple accounts/regions" → CloudFormation StackSets
- "static website + HTTPS" → S3 + CloudFront + ACM
- "Windows scripting / PowerShell automation" → AWS Tools for PowerShell
- "build apps using AWS APIs" → AWS SDKs
