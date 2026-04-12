# Module 1 - Systems Operation
<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/b531d2ee-8f7b-4acb-b419-b63bdcba1fb7" />


## Topic 1: Systems Operations in the Cloud

**Core Concept:** Cloud sys ops = managing complex computing systems across the full lifecycle: Build → Test → Deploy → Monitor → Maintain → Safeguard.

<img width="1440" height="678" alt="image" src="https://github.com/user-attachments/assets/2ac3bad4-d12b-4da3-ae12-029e29b69102" />


**Automation enables three critical capabilities:**
1. Repeatable deployment of infrastructure and applications on demand (same CloudFormation template → Dev, Test, Prod)
2. Creation of self-describing systems (the code IS the documentation)
3. Building well-tested, secure systems before they reach production

**Scenario:** A company deploys manually in Prod but uses CloudFormation in Dev. When the Prod environment breaks at 2am, the team cannot reproduce it in Dev. The fix: use the same IaC template for ALL environments.

**Exam Trap:** Manual console configuration is NOT repeatable — drift between environments is guaranteed.

---

## Topic 2: AWS Global Infrastructure and Scope

This is one of the most tested topics in SAA-C03.

<img width="1200" height="900" alt="image" src="https://github.com/user-attachments/assets/a28d092b-c666-4e25-b25a-02cdc55f2b04" />


**Three scopes — services belong to exactly one:**

| Scope | Examples |
|---|---|
| Global | IAM users/groups/roles, Route 53 hosted zones, CloudFront distributions, AWS Organizations SCPs |
| Regional | S3 buckets, AMIs, CloudWatch metrics, EBS snapshots, ElastiCache, VPC, Lambda |
| Availability Zone | EC2 instances, EBS volumes, RDS instances, subnets |

**Region Selection Criteria (4 factors):**
1. Data sovereignty / compliance (Germany data must stay in eu-central-1)
2. Latency to end users (closest region = lowest latency)
3. Service availability (not all services in all regions)
4. Cost (pricing varies 10–40% between regions)

**Service Limits:**
- Soft limits: adjustable by AWS Support request (e.g., 5 VPCs/region, EC2 instance limits)
- Hard limits: cannot be changed (e.g., 1 Internet Gateway per VPC)
- Exceeding soft limits can cause performance degradation before you hit the hard stop

**Exam Traps:**
- S3 is REGIONAL, not global (data stays in chosen region; bucket NAME is globally unique)
- EBS volumes are AZ-scoped, not regional (cannot attach us-east-1a EBS to us-east-1b EC2)
- IAM IS global — creating a user in us-east-1 = that user exists everywhere immediately

---

## Topic 3: Core Services — VPC, EC2, IAM

**Amazon VPC (Virtual Private Cloud):**
- Logically isolated virtual network
- Configurable: IP ranges (CIDR blocks), routing tables, internet gateways, security settings
- A VPC spans all AZs in a region; subnets are AZ-specific

**Amazon EC2:**
- Virtual computing environments (instances)
- Launched from AMIs (Amazon Machine Images) — AMIs are regional
- Scalable: change instance type; Optimizable: choose storage, networking, CPU

**AWS IAM (covered in depth below)**

---

## Topic 4: AWS IAM

**What IAM does:** Centrally manages authentication (who are you?) and authorization (what can you do?) for all AWS resources. Free service included with every AWS account.

<img width="1440" height="1200" alt="image" src="https://github.com/user-attachments/assets/00c57e79-dea7-4f6a-a398-03898f185b7f" />


### IAM Components

**Users:** Individual identities with long-term credentials. Created for humans or applications needing permanent access.

**Groups:** Collections of users. Policies attached to groups apply to all members. Groups CANNOT assume roles and CANNOT be nested (no group-in-group).

**Roles:** Temporary identities assumed by AWS services, applications, or external users via STS. Provide temporary credentials (default 1 hour). Key use cases:
- EC2 instance role → application accesses S3 without hardcoded credentials
- Cross-account access → auditor in Account B assumes role in Account A
- Identity federation → AD users assume IAM roles via SAML 2.0

**Policies:** JSON documents defining permissions. Key elements:
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "identifier",
    "Effect": "Allow" or "Deny",
    "Action": "ec2:*" or ["s3:GetObject","s3:PutObject"],
    "Resource": "*" or specific ARN,
    "Condition": { ... }
  }]
}
```

### Policy Types (order of frequency)

1. Identity-based policies — attached to users, groups, roles
   - Managed (AWS or customer) — reusable, versioned
   - Inline — embedded directly in one identity, 1:1 relationship
2. Resource-based policies — attached to resources (S3, SQS, KMS). Include a Principal element.
3. Permission boundaries — set maximum permissions for an identity (cannot grant beyond boundary)
4. AWS Organizations SCPs — control max permissions for entire accounts/OUs
5. Session policies — temporary permissions for assumed-role sessions
6. Access Control Lists (ACLs) — legacy, mainly for S3 cross-account

### IAM Authorization Logic (Policy Evaluation)

1. Start with DENY (default)
2. Check for explicit DENY → if found, DENY (this beats everything)
3. Check for explicit ALLOW → if found, check permission boundaries and SCPs
4. If no ALLOW found → DENY (implicit)

**Critical rule: Explicit DENY always wins over any ALLOW — even root cannot override an SCP Deny.**

### Authentication Methods

**Console access:** Account ID or alias + IAM username + password. If MFA enabled → OTP code required.

**Programmatic access:** Access Key ID + Secret Access Key. Used for CLI, SDK, API calls. Access keys have two parts — never embed in code, store in `~/.aws/credentials`.

### Credential Types (all 5 tested)

| Credential | Use |
|---|---|
| Email + password | Root account only |
| IAM username + password | Console login |
| Access key ID + secret | CLI, SDK, API |
| MFA token | Extra security layer |
| Key pairs | EC2 SSH (RSA) |

### The IAM authorization flow diagram:
<img width="1440" height="1186" alt="image" src="https://github.com/user-attachments/assets/e60bb0b2-bc48-49e1-bd31-0452247f58ea" />


### IAM Best Practices (memorize all 4)

1. Never use root credentials for daily administration
2. Delegate with least-privilege (minimum permissions needed)
3. Use IAM roles for cross-account access
4. Enable MFA for root and privileged users

### Same-Account vs Cross-Account Policy Logic

**Same account:** Resource policy Allow alone OR identity policy Allow alone = access granted (union)

**Cross-account:** BOTH the resource policy (must trust the other account) AND the identity policy in the other account (must allow the action) are required

---

## Topic 5: AWS CLI

### Installation and Configuration

```bash
pip3 install awscli --upgrade --user   # Linux
aws configure                           # Set credentials
```

`aws configure` stores 4 values in `~/.aws/credentials` and `~/.aws/config`:
1. AWS Access Key ID
2. AWS Secret Access Key
3. Default region name (e.g., us-east-1)
4. Default output format (json / text / table)


<img width="1440" height="1200" alt="image" src="https://github.com/user-attachments/assets/a3466bdb-93cd-43df-8bfe-2b7b3df5f7f9" />


### Command Structure

```
aws  [service]  [operation]  [parameters]  [options]
aws    ec2    describe-instances  --filter "..."  --output json
```

### The Three Output Formats

| Format | Best for |
|---|---|
| json (default) | Programmatic parsing, scripts |
| text | Tab-delimited, bash/PowerShell pipes |
| table | Human-readable, quick review |

### --filter vs --query (most tested CLI topic)

**--filter (server-side):**
- Restricts what AWS RETURNS to your CLI
- Reduces API payload and cost
- Syntax: `--filter "Name=instance-type,Values=t2.micro"`
- Multiple values: `--filter "Name=instance-type,Values=t2.micro,t2.small"`

**--query (client-side):**
- JMESPath expression that shapes the DISPLAY of what was already returned
- Does NOT reduce API calls or response size
- All data still transferred from AWS to client
- Syntax: `--query 'Reservations[*].Instances[*].State.Name'`
- Use [0] for first item, [*] for all items, dot notation for nesting

**Best practice:** Use both together — filter reduces response, query shapes display.

**--dry-run:**
- Tests permissions without executing the action
- Returns "DryRunOperation" if permissions are sufficient
- Returns "UnauthorizedOperation" if permissions insufficient
- Re-run with `--no-dry-run` to actually execute

### Common Commands

```bash
aws ec2 run-instances        # Launch EC2
aws ec2 describe-instances   # List EC2 instances
aws ec2 create-volume        # Create EBS volume
aws ec2 create-vpc           # Create VPC
aws s3 ls                    # List S3 buckets/objects
aws s3 cp                    # Copy files
aws s3 mv                    # Move files
aws s3 rm                    # Delete files
aws help                     # Top-level help
aws ec2 help                 # Service-level help
aws ec2 describe-instances help  # Command-level help
```

### Three Ways to Use AWS

1. AWS Management Console — GUI, easiest for one-off tasks
2. AWS CLI — command line, best for automation and scripting
3. SDKs — programmatic API calls from Python, Java, Node.js, etc.

**Exam Trap:** Using CLI vs Console does NOT change the underlying architecture or what gets created — it's just the interface.

---
### AWS Scope / Global Infrastructure flowchart

<img width="1440" height="1058" alt="image" src="https://github.com/user-attachments/assets/432ef62a-9d09-40ec-8b5f-69e1c8e780c5" />


## 20 Scenario MCQs with Exam Traps


**Q1 [EXAM TRAP] [MULTI-CORRECT]** A startup says "We need global availability" and recreates IAM users in every region and deploys S3 buckets everywhere. Which statements are TRUE? (choose 2)
- A. IAM users are region-specific and must be recreated per region
- B. IAM users are global and work across all regions
- C. S3 buckets are regional but have a globally unique namespace
- D. S3 automatically replicates data globally
- E. EC2 AMIs are automatically available globally

**Answer: B, C**
*Explanation — TRAP: IAM is GLOBAL — one user works everywhere instantly. S3 bucket data stays in the chosen region (regional), but bucket names must be globally unique. AMIs are regional — copy them manually.*

---

**Q2 [EXAM TRAP] [MULTI-CORRECT]** Security audit: root account used for daily EC2 ops. Apply IAM best practices (choose 3):

- A. Create IAM user with AdministratorAccess for daily tasks
- B. Enable MFA on root account
- C. Delete the root account
- D. Create IAM groups with appropriate policies
- E. Share root credentials only with senior admins

**Answer: A, B, D**
*Explanation — TRAP: You CANNOT delete the root account — it is the account owner. Never share root credentials with anyone, ever.*

---

**Q3 [SCENARIO]** 5,000 on-prem Active Directory employees need AWS Console access without individual IAM accounts. Best approach?

- A. Create 5,000 IAM users matching AD usernames
- B. Configure identity federation using SAML 2.0 between AD and AWS
- C. Give all employees a shared IAM user
- D. Use root for all staff operations
- E. Create IAM groups mapped 1:1 to AD groups

**Answer: B**
*Explanation: SAML 2.0 federation allows AD users to assume IAM roles — no individual IAM user creation needed. TRAP: Groups alone cannot provide SSO from on-prem AD.*

---

**Q4 [EXAM TRAP]** `aws ec2 run-instances --dry-run` returns "DryRunOperation — Request would have succeeded." What is true?

- A. Instance was created
- B. Developer has permissions but no instance was launched
- C. AMI is invalid
- D. B is true AND must use --no-dry-run to actually launch
- E. dry-run only works for IAM checks

**Answer: D**
*Explanation — TRAP: --dry-run never creates resources. "DryRunOperation" = permissions OK. Must re-run with --no-dry-run to actually create.*

---

**Q5 [EXAM TRAP]** Account has 100 EC2 instances, 10 are t2.micro. You run: `aws ec2 describe-instances --filter "Name=instance-type,Values=t2.micro" --query "Reservations[*].Instances[*].InstanceId"`. How many returned by AWS, how many displayed?

- A. 100 returned, 100 displayed
- B. 100 returned, 10 displayed
- C. 10 returned, 10 displayed
- D. 10 returned, 100 displayed
- E. Error — filter and query conflict

**Answer: C**
*Explanation — TRAP: --filter is SERVER-SIDE (only 10 returned from AWS). --query is CLIENT-SIDE (shapes those 10 to show only IDs). Option B is the classic trap assuming query is server-side.*

---

**Q6 [SCENARIO] [MULTI-CORRECT]** German financial firm — EU data must stay in EU, fastest response for EU customers (choose 2):

- A. Data sovereignty — use eu-central-1
- B. Use us-east-1 for most services
- C. Latency — region closest to EU users
- D. Always pick cheapest region
- E. Use global service to avoid restrictions

**Answer: A, C**
*Explanation: Data sovereignty (legal compliance) and latency to users are the primary criteria.*

---

**Q7 [EXAM TRAP]** IAM policy: Allow s3:* all resources. SCP: Deny s3:DeleteBucket. Developer tries to delete a bucket. Result?

- A. Bucket deleted — IAM allows s3:*
- B. DENIED — SCP explicit deny overrides IAM allow
- C. Need AWS Support to remove SCP
- D. Succeeds if MFA enabled
- E. SCP only applies to root account

**Answer: B**
*Explanation — CRITICAL TRAP: Explicit DENY in ANY policy (especially SCP) ALWAYS wins. No Allow can override an explicit Deny. SCPs apply to ALL accounts, not just root.*

---

**Q8 [SCENARIO]** Third-party auditor (own AWS account) needs temporary read-only CloudTrail access. Most secure approach?

- A. Create IAM user, email credentials
- B. Give auditor temporary root access
- C. Create IAM role with read-only, allow auditor's account to assume it
- D. Create public S3 bucket with logs
- E. Add auditor to IAM group

**Answer: C**
*Explanation: Cross-account IAM role via STS = temporary credentials, no permanent creds shared. TRAP: IAM user for externals = long-term credentials that can be lost/stolen.*

---

**Q9 [EXAM TRAP] [MULTI-CORRECT]** All valid AWS credential types (choose ALL):

- A. Email + password (root)
- B. IAM username + password (console)
- C. Access keys (CLI/API/SDK)
- D. MFA token
- E. Key pairs (EC2 SSH)
- F. STS session tokens (assumed roles)

**Answer: A, B, C, D, E, F**
*Explanation — TRAP: Students forget key pairs (E) for EC2 SSH and STS session tokens (F) for assumed roles. All 6 are tested.*

---

**Q10 [SCENARIO]** List ONLY running EC2 in us-west-2, show InstanceId and State. Correct command?

- A. `aws ec2 describe-instances --region us-west-2 --query '[InstanceId,State.Name]'`
- B. `aws ec2 list-instances --region us-west-2`
- C. `aws ec2 describe-instances --filter 'Name=instance-state-name,Values=running' --query '[InstanceId,State.Name]' --region us-west-2`
- D. `aws ec2 get-instances --state running`
- E. A and C produce identical results

**Answer: C**
*Explanation: C is optimal — --filter reduces API payload (server-side), --query shapes display (client-side). TRAP: "list-instances" and "get-instances" commands do not exist in AWS CLI.*

---

**Q11 [EXAM TRAP]** IAM user created in us-east-1. 'IAM is global' means what?

- A. User can only access us-east-1 resources
- B. User exists globally and can access any region per attached policies
- C. IAM replicates to all regions with 5-minute delay
- D. User must be recreated per region
- E. User auto-gets access to all 200+ AWS services

**Answer: B**
*Explanation — TRAP: Global identity ≠ global permissions. The IAM user IDENTITY is global (exists everywhere instantly). ACCESS to services is controlled by attached policies.*

---

**Q12 [SCENARIO]** 50 developers need identical S3 read-only access, policy must be updatable in one action later. Best design?

- A. 50 users with inline S3ReadOnly policy each
- B. IAM group 'Developers' with AmazonS3ReadOnlyAccess, add all 50 users
- C. One IAM role, all developers share role credentials
- D. Root account for all developer operations
- E. 50 users, update each policy individually when needed

**Answer: B**
*Explanation: Groups enable centralized permission management — one policy update = all 50 members updated. TRAP: Roles cannot be shared. Inline policies require 50 individual updates.*

---

**Q13 [EXAM TRAP] [MULTI-CORRECT]** TRUE statements about IAM roles (choose 3):

- A. EC2 can be assigned IAM role to access S3 without hardcoding credentials
- B. IAM roles can be assumed cross-account
- C. IAM roles have permanent access keys that never expire
- D. EC2 with attached role uses temporary creds from instance metadata service
- E. IAM roles support federation SAML 2.0 and OAuth 2.0

**Answer: A, B, E**
*Explanation — TRAP: C is FALSE — roles provide TEMPORARY credentials via STS. D is misleading — instance metadata service delivers credentials that ARE temporary and auto-rotate, not permanent.*

---

**Q14 [EXAM TRAP]** Which service-scope combination is INCORRECT?

- A. IAM users — Global
- B. S3 buckets — Regional
- C. EC2 instances — Availability Zone
- D. CloudFront distributions — Global
- E. EBS volumes — Regional (shared across AZs in same region)

**Answer: E**
*Explanation — CRITICAL TRAP: EBS volumes are AZ-SCOPED, not regional. A volume in us-east-1a CANNOT be attached to EC2 in us-east-1b. Must snapshot and restore in target AZ.*

---

**Q15 [EXAM TRAP]** Team hits 5 VPC limit and EC2 instance limit. Correct action?

- A. Both soft limits — submit service limit increase request to AWS Support
- B. VPC: hard limit. EC2: soft limit
- C. Both hard limits — cannot change
- D. Both reset automatically after 30 days
- E. VPC limit is 5 forever; EC2 has no limit

**Answer: A**
*Explanation — TRAP: Confusing 'default limit' with 'hard limit.' Both VPC per region (5) and EC2 instance limits are SOFT — raise via AWS Service Quotas or Support request.*

---

**Q16 [SCENARIO]** DevOps needs identical Dev/Test/Prod infrastructure deployments. Which cloud operations capability enables this?

- A. Manual console config replicated by screenshots
- B. Repeatable automated deployments via CloudFormation/scripts/templates
- C. Separate AWS accounts per environment
- D. AWS Support replicates on request
- E. IAM policies mirror environments

**Answer: B**
*Explanation: Infrastructure as Code with repeatable templates creates identical environments. This is the core value of cloud automation.*

---

**Q17 [EXAM TRAP] [MULTI-CORRECT]** VPC 'logical isolation' defined by which configurable features? (choose 3)

- A. IP address ranges (CIDR blocks)
- B. Routing tables
- C. Physical server assignment
- D. Security groups and NACLs
- E. Internet gateways
- F. Geographic location of data centers

**Answer: A, B, E**
*Explanation — TRAP: Physical servers are abstracted (you don't control hardware). SGs/NACLs are access controls within a VPC, not isolation features. Geographic location = Region.*

---

**Q18 [EXAM TRAP]** S3 bucket resource policy grants s3:GetObject to an IAM user. User's identity policy has NO S3 permissions. Same account. Can user access the bucket?

- A. No — needs both resource and identity policy
- B. Yes — cross-account: either policy alone works
- C. Yes — same account: resource policy Allow alone is sufficient
- D. No — resource policies only for cross-account
- E. Only if bucket is public

**Answer: C**
*Explanation — CRITICAL TRAP: SAME ACCOUNT = union of policies (resource policy alone can grant). CROSS-ACCOUNT = intersection required (both must allow). This distinction appears in multiple SAA exam questions.*

---

**Q19 [EXAM TRAP] [MULTI-CORRECT]** `aws configure` sets which 4 values?

- A. AWS Access Key ID
- B. AWS Secret Access Key
- C. Default region name
- D. Default output format
- E. IAM username
- F. AWS Account ID

**Answer: A, B, C, D**
*Explanation — TRAP: IAM username and Account ID are NOT stored in CLI config. Console uses username+password; CLI uses access keys. Never configure root access keys in CLI.*

---

**Q20 [SCENARIO] [EXAM TRAP]** IAM policy: Allow ec2:* with Condition: MFA=true AND SourceIp=10.0.0.0/24. Developer has MFA enabled, connects from 203.0.113.5. Tries to start EC2. Result?

- A. Allowed — MFA satisfies the condition
- B. DENIED — source IP 203.0.113.5 not in 10.0.0.0/24
- C. Allowed — ec2:* covers all EC2 actions
- D. Denied — BoolIfExists doesn't work with IP conditions
- E. Allowed — MFA overrides IP restriction

**Answer: B**
*Explanation — TRAP: Policy conditions use AND logic — BOTH conditions must be simultaneously true. MFA = satisfied. SourceIp = NOT satisfied (203.x.x.x is outside 10.0.0.0/24). Action DENIED. 'BoolIfExists' means enforce MFA if the context key exists — IP is still independently checked.*

---

## Quick Reference: Hardest Exam Traps Summary

| Trap | Correct Answer |
|---|---|
| S3 scope | Regional (not global) |
| IAM scope | Global |
| EBS scope | AZ (not regional!) |
| AMI scope | Regional (must copy to share) |
| Explicit DENY | Always wins over any Allow |
| SCP Deny vs root | Root CANNOT override SCP Deny |
| --filter | Server-side (reduces API response) |
| --query | Client-side (shapes display only) |
| --dry-run success | Permissions OK, NO resource created |
| Same-account resource policy | Alone can grant access |
| Cross-account resource policy | BOTH policies needed |
| Role credentials | Temporary (STS, ~1 hour) |
| Default VPC limit | 5/region — soft limit (can increase) |
| 1 IGW per VPC | Hard limit (cannot change) |
