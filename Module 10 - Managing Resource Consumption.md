# Module 10 - Managing Resource Consumption

---

## TOPIC 1: AWS TAGGING

### What is a Tag?
A tag is a **key-value pair** attached to AWS resources for identification, organization, cost tracking, and access control.

**Structure:**
```
Key   = "Environment"
Value = "Production"
```

### Tag Characteristics (Exam Critical Points)

**Limits:**
- Maximum **50 user-defined tags** per resource
- Tag keys: up to **128 characters**
- Tag values: up to **256 characters**
- Tags with `aws:` prefix are **AWS-reserved** — you cannot create, modify, or delete them

**Case Sensitivity:**
- Tag keys and values are **case-sensitive**
- `Environment` ≠ `environment` ≠ `ENVIRONMENT`

> 🪤 **Exam Trap:** A question might say "your tags aren't working as expected" — the answer is often a case-sensitivity mismatch.

**Propagation:**
- Resources created via **AWS CloudFormation** automatically inherit the stack tag `aws:cloudformation:<stack-name>`
- Tags can be propagated in **Auto Scaling Groups** to EC2 instances

> 🪤 **Exam Trap:** Tags do NOT automatically propagate from a parent resource to child resources unless explicitly configured (e.g., ASG to EC2 instances requires enabling tag propagation).

---

### Common Tag Categories (Memorize for Scenario Questions)

| Tag Key | Example Values |
|---|---|
| Environment | Production, Development, Test |
| Application | WebApp, DatabaseApp |
| Owner | TeamA, JohnDoe |
| Department | Marketing, Finance |
| CostCenter | 115, 220 |
| Purpose | Temporary, Permanent |
| Stack | Frontend, Backend |

---

### Real Scenario Example
> A company has 200 EC2 instances across Dev, Test, and Production. Finance asks for a cost breakdown per department. **Solution:** Apply tags like `Department=Marketing`, `CostCenter=115` and activate **Cost Allocation Tags** in the Billing console. Then use **AWS Cost Explorer** to filter by tag.

---

## TOPIC 2: ENFORCING TAGGING

### Method 1 — IAM Policy with Condition Element

You can write IAM policies that **deny resource creation** unless specific tags are provided.

```json
{
  "Effect": "Allow",
  "Action": "ec2:CreateVolume",
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "aws:RequestTag/costcenter": "115",
      "aws:RequestTag/department": "Accounting"
    },
    "ForAllValues:StringEquals": {
      "aws:TagKeys": ["costcenter", "department"]
    }
  }
}
```

**Key IAM Tag Condition Keys:**

| Condition Key | Purpose |
|---|---|
| `aws:RequestTag/key` | Tag must be present when creating resource |
| `aws:ResourceTag/key` | Tag must be on existing resource |
| `aws:TagKeys` | Controls which tag keys are allowed |

> 🪤 **Exam Trap:** `aws:RequestTag` applies at **creation time**. `aws:ResourceTag` applies to **existing resources**. Questions often swap these to trick you.

---

### Method 2 — AWS Config Required-Tags Rule

AWS Config uses a **managed rule** called `required-tags` to periodically check if resources have mandatory tags.

**Parameters:**
- `tag1Key` — Required tag key
- `tag1Value` — Optional required value (comma-separated for multiple)

**How it works:**
1. AWS Config continuously monitors resource configurations
2. Compares against the `required-tags` rule
3. Marks non-compliant resources
4. Can trigger **AWS Systems Manager Automation** to remediate

> 🪤 **Exam Trap:** AWS Config **does NOT prevent** resource creation — it only **identifies non-compliance after the fact**. IAM policies PREVENT creation. This distinction is heavily tested.

---

### Scenario: Conformity Monkey Pattern

```
instances = describe-instances
for each instance:
    if instance does NOT have "Required_Tag":
        terminate instance
```

This is used to **enforce tagging compliance** — tag your resource or it gets terminated. This is a well-known Netflix Chaos Engineering pattern adapted for AWS.

---

### Tagging Best Practices (SAA Loves These)

1. Create **business-relevant tag groupings** (technical, business, security dimensions)
2. Use **more tags** rather than fewer — you can always filter later
3. Use **meaningful names** for keys and values
4. Use a **standardized, case-sensitive format** consistently
5. Use the **Resource Groups Tagging API** for automation

---

## TOPIC 3: COST MANAGEMENT TOOLS

### AWS Cost Management Tool Comparison (Heavily Tested)

| Tool | Purpose | Key Feature |
|---|---|---|
| AWS Billing Dashboard | Overview of costs and usage | Monthly bill access |
| AWS Cost Explorer | Visualize and analyze spending | Forecast future costs, export to CSV |
| AWS Budgets | Set spending limits | Alert via SNS when threshold exceeded |
| Amazon CloudWatch Billing Alarms | Monitor usage charges | Alert when charges hit threshold |
| AWS Cost and Usage Report | Detailed usage data | Most granular, integrates with S3/Athena |

---

### AWS Cost Explorer — Deep Dive

- Can group costs by **Service, Region, Tag, Account**
- Can **forecast** costs based on historical trends
- Supports **Reserved Instance recommendations**
- Can export data to **.csv** file
- Covers **13 months** of historical data by default

> 🪤 **Exam Trap:** Cost Explorer is for **analysis and forecasting**. AWS Budgets is for **alerting**. Questions often mix these up.

---

### AWS Budgets — Deep Dive

**Four types of budgets:**

| Budget Type | What it tracks |
|---|---|
| Cost Budget | Dollar amount spent |
| Usage Budget | Service usage (e.g., EC2 hours) |
| Reservation Budget | RI or Savings Plan utilization/coverage |
| Savings Plans Budget | Savings Plan utilization/coverage |

**Alerts:**
- Up to **5 SNS notifications** per budget
- Alerts at **actual** or **forecasted** cost thresholds
- Example: Alert when actual cost exceeds 80% of $500 budget

---

### Amazon CloudWatch Billing Alarms

- Must be created in **us-east-1 (N. Virginia)** region — this is where billing metrics are stored globally
- Sends notifications via **Amazon SNS**
- Based on **EstimatedCharges** metric
- Works on total charges or per-service charges

> 🪤 **Exam Trap:** If a question asks where to create a billing alarm, the answer is always **us-east-1**, regardless of where your resources are deployed.

---

## TOPIC 4: CLOUD COST OPTIMIZATION STRATEGIES

### The AWS Pay-As-You-Go Advantage

Unlike on-premises where you pay for capacity whether or not you use it, AWS allows you to:
- Turn off **development/test environments** nights and weekends
- Shut down **disaster recovery** environments when not needed
- Terminate **instances tagged as temporary** automatically

---

### Stopinator Script Pattern

The **Stopinator** is a script that starts/stops AWS resources based on tags.

```bash
stopinator stop [tags]    # Stop resources with matching tags
stopinator start [tags]   # Start resources with matching tags
```

**Serverless implementation (Best Practice):**

| Component | Role |
|---|---|
| AWS Lambda | Runs the stopinator code (Python/Node.js) without servers |
| Amazon CloudWatch Events (EventBridge) | Triggers Lambda on schedule (e.g., every day at 18:00) |

> 🪤 **Exam Trap:** You do NOT need an EC2 instance running 24/7 just to run a scheduled script. Lambda + EventBridge is the serverless, cost-efficient answer.

**Cost savings example:**
- EC2 t3.medium runs 8 hours/day instead of 24 = saves **67% of instance cost**

---

### Designing for Cost Reduction — The 5 Strategies

```
1. Set Budgets         → AWS Budgets
2. Right-size          → T2/T3 instances, CloudWatch metrics
3. Serverless          → AWS Lambda (no idle server costs)
4. Managed Services    → Amazon RDS, DynamoDB (no DB admin overhead)
5. Trusted Advisor     → Real-time cost optimization guidance
```

### Finding and Eliminating Waste

- Use **CloudWatch CPU Utilization metrics** to find long-running idle instances
- Use **Cost Explorer** to identify expensive projects
- Look for instances with **< 10% CPU utilization** over 14 days (Trusted Advisor threshold)

---

## TOPIC 5: AWS TRUSTED ADVISOR

### What is AWS Trusted Advisor?

Trusted Advisor **scans your AWS infrastructure**, compares it against AWS best practices, and provides **recommendations with action links** in 5 categories.

### The 5 Categories

```
1. Cost Optimization    → Find idle/underused resources
2. Performance          → Improve service performance
3. Security             → Close security gaps
4. Fault Tolerance      → Increase resiliency
5. Service Limits       → Monitor approaching limits
```

---

### Cost Optimization Checks (Memorize These)

| Check | What it looks for |
|---|---|
| Idle EC2 Instances | CPU < 10% for 14+ days |
| Underutilized EBS Volumes | Volumes with low I/O |
| Idle RDS Instances | DB connections = 0 for 7+ days |
| Unused Elastic IP Addresses | EIPs not associated with running instances |
| Underutilized Load Balancers | Low request count |
| Reserved Instance Optimization | Suggests RI purchases |

---

### Trusted Advisor Access Tiers (HEAVILY TESTED)

| Support Plan | Checks Available |
|---|---|
| Basic & Developer | 7 core checks only (S3 permissions, SG ports, MFA on root, IAM use, EBS snapshots, RDS public, service limits) |
| Business | All checks + API access + CloudWatch integration |
| Enterprise | All checks + dedicated TAM |

> 🪤 **Exam Trap:** A question might say "you want full Trusted Advisor recommendations" — the answer requires **Business or Enterprise support plan**, not Basic or Developer.

> 🪤 **Exam Trap:** The 7 core checks are available to ALL customers. Don't say "Trusted Advisor is only for Business/Enterprise."

---

### Trusted Advisor vs Other Services

| Question | Answer |
|---|---|
| Who gives real-time best practice guidance? | Trusted Advisor |
| Who monitors costs and sends alerts? | AWS Budgets / CloudWatch |
| Who analyzes and visualizes spending? | Cost Explorer |
| Who enforces tagging at resource creation? | IAM Policy |
| Who identifies untagged resources after creation? | AWS Config |

---

## TOPIC 6: SCENARIO-BASED EXAM QUESTIONS

### Scenario 1
> A company wants to shut down all EC2 instances tagged `Environment=Development` every Friday at 6 PM automatically. What is the MOST cost-effective solution?

**Answer:** Create an **AWS Lambda function** that calls `ec2:StopInstances` filtered by tag `Environment=Development`. Use **Amazon EventBridge (CloudWatch Events)** to trigger the Lambda function on a cron schedule (`cron(0 18 ? * FRI *)`).

> Why not EC2-based cron? Because you'd pay for that instance 24/7 just to run a script. Lambda is serverless — you only pay for execution time.

---

### Scenario 2
> A developer created an EC2 instance without the required `CostCenter` tag. The IAM policy should have prevented this but didn't. What is the likely cause?

**Answer:** The IAM policy uses `aws:RequestTag/CostCenter` but the developer used `costcenter` (lowercase). Tags and tag conditions are **case-sensitive**.

---

### Scenario 3
> A security team wants to ensure all new S3 buckets have the tag `DataClassification=Confidential`. They want **real-time prevention**, not just detection. Which service should they use?

**Answer:** **IAM Policy** with `Condition: StringEquals: aws:RequestTag/DataClassification: Confidential`. AWS Config only **detects** non-compliance — it cannot **prevent** resource creation.

---

### Scenario 4
> A company receives a large AWS bill and wants to understand which department is responsible for the most spending. What should they configure?

**Answer:**
1. Apply **cost allocation tags** (`Department=Marketing`, etc.) to all resources
2. **Activate** the tags in the Billing & Cost Management console
3. Use **AWS Cost Explorer** grouped by tag to analyze spending per department

> 🪤 **Exam Trap:** Tags must be **activated as cost allocation tags** in the billing console before they appear in Cost Explorer reports. Simply applying tags is not enough.

---

### Scenario 5
> A startup wants to receive an email alert when their AWS bill is projected to exceed $200 this month. What is the simplest solution?

**Answer:** Create an **AWS Budget** of type "Cost" set to $200, with an alert at 80% forecasted threshold, sending notification to an **SNS topic** that emails the team.

> Alternative: CloudWatch Billing Alarm (must be in us-east-1). Both are valid but Budgets is more feature-rich.

---

## TOPIC 7: KEY EXAM TRAPS SUMMARY

| # | Trap | Correct Answer |
|---|---|---|
| 1 | Tags prevent resource creation | FALSE — IAM policies prevent; Config detects |
| 2 | Billing alarms can be in any region | FALSE — Must be in us-east-1 |
| 3 | All customers get full Trusted Advisor | FALSE — Full access requires Business/Enterprise plan |
| 4 | Tags are case-insensitive | FALSE — Tags ARE case-sensitive |
| 5 | Cost Explorer sends alerts | FALSE — Budgets sends alerts; Explorer analyzes |
| 6 | Tags auto-propagate to all child resources | FALSE — Must configure explicitly |
| 7 | 50 tag limit includes AWS-reserved tags | FALSE — 50 is for user-defined tags only |
| 8 | EC2 needed to run scheduled scripts | FALSE — Lambda + EventBridge is serverless best practice |
| 9 | Config enforces tagging at creation | FALSE — Config only identifies non-compliance |
| 10 | Cost allocation tags work immediately after applying | FALSE — Must activate them in billing console first |

---

## QUICK REVISION MIND MAP

```
Managing Resource Consumption
│
├── TAGGING
│   ├── Key-value pair on AWS resources
│   ├── Max 50 user-defined tags/resource
│   ├── Case-sensitive
│   ├── aws: prefix = reserved
│   └── Use: Cost tracking, Automation, Access control
│
├── ENFORCE TAGGING
│   ├── IAM Policy → PREVENTS creation (Condition element)
│   └── AWS Config → DETECTS non-compliance (required-tags rule)
│
├── COST TOOLS
│   ├── Billing Dashboard → Overview + Monthly bill
│   ├── Cost Explorer → Analyze + Forecast + Export CSV
│   ├── AWS Budgets → Alerts when threshold exceeded
│   └── CloudWatch Billing Alarm → Must be in us-east-1
│
├── COST STRATEGIES
│   ├── Right-size instances (T2/T3)
│   ├── Reserved Instances / Spot Instances
│   ├── Serverless (Lambda)
│   ├── Managed Services (RDS, DynamoDB)
│   └── Stopinator (Lambda + EventBridge)
│
└── TRUSTED ADVISOR
    ├── 5 categories: Cost, Performance, Security, Fault Tolerance, Limits
    ├── Core checks → All customers (7 checks)
    └── Full checks → Business/Enterprise support only
```

---

# 20 Scenario-Based MCQs 
---

## MCQ 1 — Tagging Enforcement

**Scenario:** A large enterprise has a policy that all EC2 instances must have two tags: `CostCenter` and `Department`. A junior developer created an EC2 instance last week without either tag. The security team wants to **prevent** this from happening in the future AND **identify** all currently untagged instances. Which combination of services should they use?

**A.** AWS Config with `required-tags` rule to prevent future untagged instances, and AWS Trusted Advisor to find existing ones

**B.** IAM Policy with `aws:RequestTag` condition to prevent future untagged instances, and AWS Config `required-tags` rule to identify existing non-compliant ones

**C.** AWS Budgets to prevent untagged instances, and Cost Explorer to find existing ones

**D.** AWS Config to prevent future untagged instances, and IAM Policy to identify existing non-compliant ones

**E.** Service Control Policies (SCP) in AWS Organizations to prevent future untagged instances, and AWS Config to identify existing non-compliant ones

---

**✅ Correct Answers: B and E**

**Explanation:**

- **B is correct** because IAM Policy with `aws:RequestTag` condition **prevents** resource creation without required tags, and AWS Config `required-tags` managed rule **detects** existing non-compliant resources.

- **E is correct** because in a multi-account AWS Organizations setup, **SCPs** can enforce tagging at creation across all accounts, and AWS Config identifies existing non-compliant resources.

- **A is wrong** because AWS Config CANNOT prevent resource creation — it only identifies non-compliance after the fact.

- **C is wrong** because AWS Budgets is for cost alerts, not for enforcing tagging.

- **D is wrong** because the roles of Config and IAM are swapped. Config cannot prevent; IAM cannot identify existing resources.

> 🪤 **Exam Trap:** Many students assume AWS Config enforces/prevents tagging. It only DETECTS non-compliance. Prevention requires IAM Policy or SCP.

---

## MCQ 2 — CloudWatch Billing Alarm

**Scenario:** A startup's CTO wants to receive an **email alert** whenever the estimated AWS charges exceed **$500** in any month. A solutions architect sets up a CloudWatch billing alarm in the **ap-south-1 (Mumbai)** region. Two weeks later, charges exceed $600 but no alert was received. What are the TWO most likely reasons?

**A.** CloudWatch billing alarms only work for accounts older than 6 months

**B.** Billing alarms must be created in the **us-east-1** region because all billing metrics are stored there globally

**C.** The CTO did not enable **Receive Billing Alerts** in the Billing Preferences console

**D.** CloudWatch billing alarms require a Business support plan

**E.** The SNS topic was created in a different region than the billing alarm

---

**✅ Correct Answers: B and C**

**Explanation:**

- **B is correct** because CloudWatch billing alarms MUST be created in **us-east-1 (N. Virginia)**. This is the only region where AWS stores the `EstimatedCharges` billing metric. Creating it in ap-south-1 means no billing metric data will flow to that alarm.

- **C is correct** because before CloudWatch can track billing metrics, you must first go to **Billing & Cost Management → Billing Preferences → Enable "Receive Billing Alerts"**. Without this, no billing metric data is published to CloudWatch.

- **A is wrong** — There is no account age requirement for billing alarms.

- **D is wrong** — Billing alarms are available to ALL support tiers.

- **E is wrong** — SNS topics can be in different regions. While best practice is same region, this alone wouldn't cause a complete failure if the alarm itself was in the wrong region.

> 🪤 **Exam Trap:** "us-east-1 for billing alarms" is one of the most commonly tested facts. Remember: ALL billing metrics globally are stored in us-east-1 ONLY.

---

## MCQ 3 — Trusted Advisor Access Tiers

**Scenario:** A company is on the **AWS Developer support plan**. Their DevOps team wants to use AWS Trusted Advisor to check for: (1) Security groups with unrestricted access, (2) Underutilized EC2 instances for cost savings, (3) MFA on root account, (4) Service limit checks. Which of these checks are available to them WITHOUT upgrading their support plan?

**A.** Security groups with unrestricted access only

**B.** Security groups with unrestricted access, MFA on root account, and service limit checks only

**C.** All four checks — Developer plan includes full Trusted Advisor

**D.** Underutilized EC2 instances only, as it's the most critical cost check

**E.** None — Trusted Advisor requires at least Business support

---

**✅ Correct Answer: B**

**Explanation:**

The **7 core Trusted Advisor checks** available to Basic and Developer plan customers are:

| Check | Category |
|---|---|
| S3 Bucket Permissions | Security |
| Security Groups - Unrestricted Access | Security |
| IAM Use | Security |
| MFA on Root Account | Security |
| EBS Public Snapshots | Security |
| RDS Public Snapshots | Security |
| Service Limits | Service Limits |

- **Security groups with unrestricted access** ✅ Core check
- **MFA on root account** ✅ Core check
- **Service limit checks** ✅ Core check
- **Underutilized EC2 instances** ❌ This is a COST OPTIMIZATION check — requires Business/Enterprise plan

- **C is wrong** — Developer plan does NOT get full Trusted Advisor.
- **E is wrong** — Core checks ARE available on Basic/Developer.

> 🪤 **Exam Trap:** Students memorize "Trusted Advisor needs Business plan" and mark ALL checks as unavailable on Developer. The 7 core checks are FREE for everyone. Cost Optimization and Performance checks require Business/Enterprise.

---

## MCQ 4 — Cost Allocation Tags

**Scenario:** A company applies tags like `Project=Phoenix` and `Department=Engineering` to all their AWS resources. At the end of the quarter, the Finance team tries to filter costs by project in **AWS Cost Explorer** but the tag filters don't appear. The tags are correctly applied on all resources. What steps are needed to fix this? (Select TWO)

**A.** Delete and reapply all tags — Cost Explorer only recognizes tags created after it's enabled

**B.** Activate the tags as **Cost Allocation Tags** in the Billing and Cost Management console

**C.** Enable **Cost Explorer** in the Billing and Cost Management console if not already enabled

**D.** Create an AWS Config rule to validate all tags are correct

**E.** Apply the tags at the AWS Organizations master account level instead

---

**✅ Correct Answers: B and C**

**Explanation:**

- **B is correct** because applying tags to resources is NOT enough. You must separately **activate them as Cost Allocation Tags** in Billing → Cost Allocation Tags. Until activated, they won't appear in Cost Explorer as filter dimensions.

- **C is correct** because AWS Cost Explorer must be **enabled** before it starts collecting data. Once enabled, it takes up to **24 hours** for data to appear.

- **A is wrong** — You don't need to delete and reapply. Cost Explorer shows data going forward from when tags were activated. Existing historical data with those tags will appear after activation, but you cannot retroactively alter old billing data.

- **D is wrong** — AWS Config validates compliance, not Cost Explorer visibility.

- **E is wrong** — Tags applied at Organization level are SCPs, not cost allocation tags.

> 🪤 **Exam Trap:** Simply tagging resources is NEVER enough for Cost Explorer filtering. The two-step process is: (1) Apply tags, (2) Activate as cost allocation tags in billing console. This is tested frequently.

---

## MCQ 5 — Stopinator Architecture

**Scenario:** A company wants to automatically stop all EC2 instances and RDS databases tagged `Environment=Development` every weekday at **8 PM** and restart them at **8 AM**. They want the solution to be **serverless, highly available, and have minimal operational overhead**. Which architecture best meets these requirements?

**A.** Launch a dedicated EC2 t2.micro instance running a cron job script that calls AWS CLI commands to stop/start instances based on tags

**B.** Use AWS Lambda functions triggered by Amazon EventBridge scheduled rules using cron expressions, with the Lambda functions using Boto3 to filter instances by tag and stop/start them

**C.** Use AWS Systems Manager Maintenance Windows to run an Automation document on a schedule

**D.** Use Amazon EC2 Auto Scaling scheduled actions to stop and start instances

**E.** Use AWS Batch with a scheduled job to stop and start instances

---

**✅ Correct Answers: B and C**

**Explanation:**

- **B is correct** — This is the **canonical serverless stopinator pattern**. Lambda + EventBridge (CloudWatch Events) is serverless, highly available, costs nearly nothing, and requires zero server management. Cron expression: `cron(0 20 ? * MON-FRI *)` for 8 PM weekdays.

- **C is correct** — AWS Systems Manager Maintenance Windows with Automation documents can also stop/start tagged instances on a schedule, and it's fully managed and serverless.

- **A is wrong** — Running an EC2 instance 24/7 just to stop other instances defeats the cost-saving purpose and adds operational overhead. It's also not serverless.

- **D is wrong** — Auto Scaling scheduled actions adjust the desired count of instances but are not designed for stopping/starting existing tagged instances across different groups.

- **E is wrong** — AWS Batch is designed for compute-intensive batch processing jobs, not resource lifecycle management.

> 🪤 **Exam Trap:** Option A is tempting because it's familiar, but it's the LEAST efficient and most costly approach. Always choose Lambda + EventBridge for scheduled serverless automation.

---

## MCQ 6 — Tag Case Sensitivity

**Scenario:** A company's IAM policy requires the tag `costcenter` with value `115` at EC2 instance creation. A developer creates an instance using the AWS Console and enters `CostCenter = 115`. The creation succeeds, but the Finance team later finds the instance isn't appearing in cost reports filtered by `costcenter`. What is the root cause and what are TWO consequences?

**A.** IAM allowed the creation because `CostCenter` and `costcenter` are treated as equivalent by IAM

**B.** The IAM policy condition was bypassed because the developer used the Console instead of the CLI

**C.** AWS tags are case-sensitive, so `CostCenter` and `costcenter` are treated as different tags, causing the IAM condition to fail silently if not set to Deny

**D.** The instance won't appear in Cost Explorer reports filtered by `costcenter` because it has `CostCenter` instead

**E.** The instance will automatically be terminated by AWS Config for non-compliance

---

**✅ Correct Answers: C and D**

**Explanation:**

- **C is correct** — AWS tags are fully case-sensitive. `CostCenter` and `costcenter` are completely different tags. If the IAM policy used `Allow` with the condition, the resource could still be created without the correct tag (the allow condition simply didn't match, but no explicit Deny existed).

- **D is correct** — Since the instance has `CostCenter=115` but the cost allocation tag activated is `costcenter`, the instance won't appear in filtered Cost Explorer views.

- **A is wrong** — IAM does NOT treat tag keys case-insensitively. They are strictly case-sensitive.

- **B is wrong** — Console vs CLI makes no difference to IAM policy evaluation.

- **E is wrong** — AWS Config identifies non-compliance but does NOT automatically terminate instances. That would require a Lambda remediation action configured separately.

> 🪤 **Exam Trap:** This is one of the sneakiest tag traps. Developers often capitalize naturally (CostCenter) while policies use lowercase (costcenter). Always establish a documented tagging standard and enforce it via documentation and tooling.

---

## MCQ 7 — AWS Config vs IAM Policy

**Scenario:** A financial services company wants to ensure that every new S3 bucket has the tag `DataClassification` set to either `Public`, `Internal`, or `Confidential`. Any bucket created without this tag should be **automatically remediated** by adding the tag `DataClassification=Unclassified`. Which combination achieves this? (Select TWO)

**A.** Create an IAM policy denying `s3:CreateBucket` unless `aws:RequestTag/DataClassification` is present

**B.** Create an AWS Config `required-tags` rule to detect non-compliant buckets

**C.** Use AWS Config with an automatic remediation action using an **SSM Automation document** to apply the default tag

**D.** Create a CloudWatch Events rule to detect S3 bucket creation and trigger a Lambda function to check and apply tags

**E.** Use AWS Budgets to alert when untagged buckets are found

---

**✅ Correct Answers: B and C**

**Explanation:**

- **B is correct** — AWS Config `required-tags` rule continuously monitors S3 buckets and marks those without `DataClassification` as non-compliant.

- **C is correct** — AWS Config supports **automatic remediation** using SSM Automation documents. You can configure it to automatically run a remediation action (like applying a default tag) when a resource is found non-compliant.

- **A is wrong** — The scenario asks for remediation (auto-tagging), not prevention. Also, if you deny creation, no bucket gets created at all — but the requirement is to allow creation and then auto-tag.

- **D is correct in theory** but B+C is the more native/integrated AWS solution. EventBridge + Lambda works but is more custom-built.

- **E is wrong** — AWS Budgets is for cost management, not compliance tagging.

> 🪤 **Exam Trap:** The scenario says "automatically remediated" — this is AWS Config + SSM Automation. Don't confuse with IAM which prevents but doesn't remediate.

---

## MCQ 8 — Reserved Instance Cost Optimization

**Scenario:** A company has been running a production web application on 10 x `r5.2xlarge` EC2 instances in `us-east-1` continuously for 2 years with no plans to change. Their current monthly EC2 bill is $15,000. A solutions architect reviews the setup. Which TWO recommendations would provide the GREATEST cost reduction?

**A.** Switch to Spot Instances for all 10 production instances

**B.** Purchase 1-year or 3-year **Reserved Instances** (Standard or Convertible) for the r5.2xlarge instances

**C.** Enable AWS Trusted Advisor to get cost optimization recommendations

**D.** Use AWS Compute Savings Plans which apply to any EC2 instance regardless of type or region

**E.** Enable Detailed Monitoring on the instances to better track utilization

---

**✅ Correct Answers: B and D**

**Explanation:**

- **B is correct** — Reserved Instances provide up to **72% savings** vs On-Demand for committed usage. For a steady-state production workload running continuously, RIs are the #1 cost optimization. 3-year All Upfront provides the maximum discount.

- **D is correct** — **Compute Savings Plans** provide up to **66% savings** and are MORE flexible than Standard RIs — they apply automatically across any EC2 instance type, size, OS, region, and even Lambda/Fargate. For a long-running workload, this is excellent.

- **A is wrong** — Spot Instances can be **interrupted with 2-minute notice**. They are NOT suitable for production workloads requiring continuous availability.

- **C is wrong** — Trusted Advisor gives recommendations but doesn't itself reduce cost. It's a tool, not a solution.

- **E is wrong** — Detailed Monitoring increases CloudWatch metrics granularity (1-minute vs 5-minute) but has NO direct cost reduction effect and actually costs more.

> 🪤 **Exam Trap:** Students often choose Spot Instances because they offer the biggest discount (up to 90%). But Spot is for **fault-tolerant, interruptible** workloads. Production = Reserved Instances or Savings Plans.

---

## MCQ 9 — Finding and Eliminating Waste

**Scenario:** A company's AWS bill increased by 40% last month. The infrastructure team suspects there are idle EC2 instances, underused EBS volumes, and forgotten Elastic IP addresses. Which THREE services/features would most effectively help identify and eliminate this waste?

**A.** AWS Trusted Advisor — checks for idle EC2 instances, underutilized EBS volumes, and unused Elastic IPs

**B.** Amazon CloudWatch — CPU Utilization metrics to identify long-running low-utilization instances

**C.** AWS Cost Explorer — to identify which services and resources are contributing most to the cost increase

**D.** AWS CloudTrail — to see who created the instances that are now idle

**E.** Amazon Inspector — to scan instances for security vulnerabilities causing the high costs

---

**✅ Correct Answers: A, B, and C**

**Explanation:**

- **A is correct** — Trusted Advisor directly checks for:
  - EC2 instances with CPU < 10% for 14 days (idle)
  - EBS volumes with low read/write operations
  - Elastic IPs not associated with running instances

- **B is correct** — CloudWatch CPU Utilization metrics can identify instances with consistently low utilization. You can create a CloudWatch dashboard or use Trusted Advisor which leverages this data.

- **C is correct** — Cost Explorer shows spending by service, region, or resource, making it easy to spot which specific services caused the 40% spike.

- **D is wrong** — CloudTrail tracks API calls (who did what) not resource utilization. It can tell you who created an instance but not if it's idle.

- **E is wrong** — Amazon Inspector is a **security vulnerability assessment** service. It has nothing to do with cost optimization.

> 🪤 **Exam Trap:** Don't confuse Amazon Inspector (security scanning) with Trusted Advisor (best practice recommendations). Both are frequently offered as distractors in cost-related questions.

---

## MCQ 10 — Tag Limits and Characteristics

**Scenario:** A DevOps engineer is building an automated tagging solution using the Resource Groups Tagging API. They need to apply the following tags to a single EC2 instance: 15 custom business tags, 10 technical tags, 20 compliance tags, and they notice the instance already has 3 `aws:` prefixed tags from CloudFormation. They also want to add 5 more tags next month. Will they hit any limits and what applies?

**A.** They will exceed the tag limit because the total including aws: tags would be 53, exceeding the 50-tag limit

**B.** They will NOT exceed the limit — the 50-tag limit only counts user-defined tags. AWS-reserved `aws:` tags do not count toward the limit

**C.** They cannot modify the `aws:` prefixed tags but can add up to 50 of their own tags (45 now + 5 next month = 50 total)

**D.** The Resource Groups Tagging API can override the 50-tag limit for enterprise customers

**E.** They should remove some compliance tags to stay within limits since compliance tags count double

---

**✅ Correct Answers: B and C**

**Explanation:**

- **B is correct** — The **50-tag limit per resource applies only to user-defined tags**. AWS-reserved tags (with `aws:` prefix) do NOT count toward this limit. So the 3 CloudFormation tags are separate.

- **C is correct** — 15 + 10 + 20 = 45 user-defined tags now, + 5 next month = 50 total user-defined tags. They will hit exactly 50, which is the maximum. Any additional tag beyond 50 will fail.

- **A is wrong** — The aws: prefixed tags don't count toward the 50-tag limit.

- **D is wrong** — There is no enterprise override for tag limits. The limit is hard at 50 user-defined tags.

- **E is wrong** — There is no concept of tags counting "double." All user-defined tags count equally as 1 toward the limit.

> 🪤 **Exam Trap:** The most common mistake is adding aws: tags to the count. Remember: aws: tags are AWS-managed and DON'T count toward your 50-tag limit.

---

## MCQ 11 — Multi-Account Cost Management

**Scenario:** A company uses AWS Organizations with a master account and 12 member accounts across Dev, Test, and Prod. The CFO wants a **single consolidated view** of all costs across all accounts, wants to receive **alerts when total spend across all accounts exceeds $50,000/month**, and wants costs **broken down by account and by the `Project` tag**. Which THREE services/features are needed?

**A.** Enable **Consolidated Billing** through AWS Organizations

**B.** Create an **AWS Budget** in the master account for $50,000 with SNS alerting

**C.** Activate the `Project` tag as a **Cost Allocation Tag** in the master account's billing console

**D.** Create separate AWS Budgets in each of the 12 member accounts and aggregate manually

**E.** Use Amazon QuickSight to visualize costs across accounts

---

**✅ Correct Answers: A, B, and C**

**Explanation:**

- **A is correct** — **Consolidated Billing** in AWS Organizations combines all member account charges into a single master account bill, enabling a unified view. It also provides **volume discounts** when combined usage hits tier thresholds.

- **B is correct** — Creating an AWS Budget in the **master account** covers ALL linked accounts' costs. Setting a $50,000 threshold with SNS notification achieves the alert requirement. Budgets in master account = organization-wide visibility.

- **C is correct** — Cost Allocation Tags must be activated in the **master (management) account's** billing console to appear in Cost Explorer for all accounts. Once activated, the `Project` tag dimension will be available for filtering across all accounts.

- **D is wrong** — Creating separate budgets per account doesn't give a consolidated view. It would also be 12x the management overhead and couldn't alert on combined total.

- **E is wrong** — QuickSight is a BI visualization tool. While it can display cost data, it's not required for this use case and adds unnecessary complexity. AWS Cost Explorer handles this natively.

> 🪤 **Exam Trap:** Cost Allocation Tag activation must happen in the MASTER/MANAGEMENT account. Doing it in a member account won't make it available org-wide in Cost Explorer.

---

## MCQ 12 — Serverless vs EC2 Cost Model

**Scenario:** A company has a Lambda function that processes image uploads. The function runs for an average of **800ms** per invocation and is invoked **2 million times per month**. They are comparing this to running a dedicated EC2 t3.medium instance (running 24/7) that could handle the same workload. Which statements are TRUE about the Lambda approach? (Select TWO)

**A.** Lambda will always be more expensive than EC2 for high-volume workloads

**B.** Lambda charges only for actual compute time (rounded up to 1ms), so you pay for 800ms × 2,000,000 invocations, not 24/7 server time

**C.** Lambda includes 1 million free requests and 400,000 GB-seconds of compute per month in the free tier

**D.** Lambda functions cannot be triggered more than 1,000 times concurrently without a limit increase

**E.** With Lambda, you eliminate costs associated with OS patching, server management, and idle compute time

---

**✅ Correct Answers: B, C, and E**

**Explanation:**

- **B is correct** — Lambda's pricing model: **$0.20 per 1M requests + $0.0000166667 per GB-second**. You pay ONLY for actual execution time. For 2M invocations at 800ms, you pay for 1.6M seconds of compute, not 744 hours of EC2 time.

- **C is correct** — Lambda Free Tier: **1 million requests/month** and **400,000 GB-seconds/month** — permanent free tier (not just 12 months). This significantly reduces Lambda costs for moderate workloads.

- **E is correct** — Lambda is fully managed. No EC2 = no patching, no AMI management, no paying for idle time between image uploads. This is a core cloud benefit.

- **A is wrong** — Lambda is often CHEAPER than EC2 for intermittent/event-driven workloads. For continuous high-throughput workloads, EC2 with Reserved Instances might be cheaper, but "always more expensive" is incorrect.

- **D is wrong** — Lambda has a default concurrency limit of **1,000 concurrent executions per region**, but this can be **increased by requesting a limit increase** from AWS Support. It's a soft limit.

> 🪤 **Exam Trap:** The Lambda 1,000 concurrent execution limit is a SOFT limit that can be raised. Don't treat it as a hard architectural constraint.

---

## MCQ 13 — AWS Config Remediation

**Scenario:** A company uses AWS Config with the `required-tags` rule requiring `Environment` tag on all EC2 instances. Config identifies 15 instances as non-compliant. The operations team wants to **automatically add** `Environment=Unknown` to all non-compliant instances without manual intervention. What should they configure? (Select TWO)

**A.** Configure AWS Config rule with **Automatic Remediation** using an SSM Automation document

**B.** Create an AWS Lambda function triggered by Config compliance change events via EventBridge to add the missing tag

**C.** Use AWS CloudTrail to automatically tag non-compliant instances

**D.** Create an AWS Budgets alert that triggers tagging via SNS

**E.** Use AWS Trusted Advisor to auto-remediate tagging issues

---

**✅ Correct Answers: A and B**

**Explanation:**

- **A is correct** — AWS Config supports **Automatic Remediation** natively. You can associate an SSM Automation document (like `AWS-AddTagsToResource`) with a Config rule. When a resource becomes non-compliant, Config automatically triggers the SSM document to remediate.

- **B is correct** — When a Config rule changes a resource to non-compliant, it emits an **Amazon EventBridge event**. You can create an EventBridge rule to trigger a Lambda function that calls `ec2:CreateTags` to add the missing tag. This is a flexible, custom remediation approach.

- **C is wrong** — CloudTrail is a **logging/auditing** service that records API calls. It has no capability to add tags or remediate compliance issues.

- **D is wrong** — AWS Budgets is for cost management and cannot trigger resource configuration changes.

- **E is wrong** — AWS Trusted Advisor provides **recommendations** but has no automatic remediation capability. You must act on its recommendations manually or build automation around them.

> 🪤 **Exam Trap:** Students confuse CloudTrail (who did what/when) with remediation services. CloudTrail RECORDS actions — it doesn't TAKE actions.

---

## MCQ 14 — Cost Explorer vs Budgets

**Scenario:** A CTO asks for the following: (1) Know which AWS service cost the most in the last 3 months, (2) See a graph of daily costs for the last 30 days broken down by region, (3) Get an email when monthly costs exceed $10,000, (4) See the predicted cost for the rest of this month. Which services handle EACH requirement correctly?

**A.** All four requirements can be met by AWS Cost Explorer alone

**B.** Requirements 1, 2, and 4 use Cost Explorer; Requirement 3 uses AWS Budgets

**C.** Requirements 1, 2, and 4 use AWS Budgets; Requirement 3 uses CloudWatch

**D.** All four requirements need AWS Trusted Advisor

**E.** Requirements 1 and 2 use Cost Explorer; Requirements 3 and 4 use AWS Budgets

---

**✅ Correct Answer: B**

**Explanation:**

**Mapping each requirement:**

| Requirement | Correct Service | Reason |
|---|---|---|
| 1 - Which service cost most in last 3 months | Cost Explorer | Group by Service, view historical |
| 2 - Daily cost graph by region for 30 days | Cost Explorer | Daily granularity, group by Region |
| 3 - Email when costs exceed $10,000 | AWS Budgets | Alert/notification on threshold |
| 4 - Predicted cost for rest of month | Cost Explorer | Has forecasting feature |

- **A is technically partially true** because Cost Explorer does have a forecast feature, but for email alerts specifically, you need **AWS Budgets** (or CloudWatch Billing Alarm). Cost Explorer itself doesn't send alerts.

- **E is partially correct** — Budgets CAN forecast, but Cost Explorer is the primary visualization and forecasting tool.

> 🪤 **Exam Trap:** Cost Explorer does NOT send alerts or notifications. AWS Budgets DOES send alerts. For "notify me when cost exceeds X" — the answer is always Budgets or CloudWatch Billing Alarm.

---

## MCQ 15 — Trusted Advisor Checks Detail

**Scenario:** A company's security audit requires checking the following items using AWS Trusted Advisor on their **Business support plan account**: (1) S3 bucket public access permissions, (2) IAM access key rotation, (3) Underutilized EC2 instances, (4) CloudFront content delivery optimization, (5) VPC flow logs enabled. Which of these checks does Trusted Advisor currently provide?

**A.** Only checks 1 and 3 because Trusted Advisor focuses on cost and basic security

**B.** Checks 1, 2, 3, and 4 — all are available on Business plan, but VPC flow logs is not a Trusted Advisor check

**C.** All 5 checks are available on Business plan

**D.** Only checks 1, 2, and 3 — CloudFront optimization and VPC flow logs are not Trusted Advisor checks

**E.** None of these — Trusted Advisor only checks for S3 and EC2 issues

---

**✅ Correct Answer: B**

**Explanation:**

Trusted Advisor checks on Business plan include:

- **S3 bucket public access permissions** ✅ — Security check (also core)
- **IAM access key rotation** ✅ — Security check (Business plan)
- **Underutilized EC2 instances** ✅ — Cost Optimization check (Business plan)
- **CloudFront content delivery optimization** ✅ — Performance check (Business plan)
- **VPC flow logs enabled** ❌ — This is NOT a Trusted Advisor check. VPC Flow Logs compliance is handled by AWS Config (vpc-flow-logs-enabled managed rule)

- **D is wrong** — CloudFront optimization IS a Trusted Advisor Performance check on Business plan.
- **C is wrong** — VPC flow logs is NOT a Trusted Advisor check.

> 🪤 **Exam Trap:** Not everything is a Trusted Advisor check. VPC Flow Logs, GuardDuty enablement, and similar configuration compliance checks are typically AWS Config managed rules, not Trusted Advisor checks.

---

## MCQ 16 — Tag Propagation in Auto Scaling

**Scenario:** A company uses an Auto Scaling Group (ASG) to launch EC2 instances. They apply tags `Environment=Production` and `Project=Alpha` to the ASG. New instances are launched but the Finance team reports that the new instances don't have the `Project=Alpha` tag and aren't appearing in cost reports. What are the TWO likely causes?

**A.** Auto Scaling Groups cannot pass tags to EC2 instances in any configuration

**B.** The `Project=Alpha` tag was added to the ASG **after** existing instances were launched, and tag propagation only applies to NEW instances launched after the tag was added

**C.** The **"Tag New Instances During Launch"** option was not enabled for the `Project=Alpha` tag in the ASG configuration

**D.** EC2 instances in an ASG require a separate IAM permission to receive tags

**E.** Tags cannot be propagated from ASG to instances if the ASG spans multiple Availability Zones

---

**✅ Correct Answers: B and C**

**Explanation:**

- **B is correct** — When you add a new tag to an existing ASG, it does NOT retroactively apply to already-running instances. The tag only propagates to **newly launched** instances after the tag was added. Existing instances need to be manually tagged or recycled.

- **C is correct** — Each tag in an ASG has a **"Tag New Instances During Launch"** checkbox. If this is unchecked for `Project=Alpha`, instances launched from this ASG will NOT inherit that tag.

- **A is wrong** — ASGs absolutely CAN propagate tags to EC2 instances. This is a standard feature.

- **D is wrong** — There is no separate IAM permission required for tag propagation from ASG to instances.

- **E is wrong** — Multi-AZ deployment has no effect on tag propagation behavior.

> 🪤 **Exam Trap:** Tag propagation in ASG has TWO requirements: (1) The "propagate at launch" option must be enabled, AND (2) it only applies to NEW instances. Existing instances must be manually tagged.

---

## MCQ 17 — Right-Sizing Instances

**Scenario:** AWS Trusted Advisor reports that 8 out of your 20 EC2 m5.4xlarge instances have had average CPU utilization below **5%** for the past 30 days, and memory utilization below 10%. The company wants to reduce costs while maintaining the same workload. Which TWO actions represent the BEST right-sizing approach?

**A.** Immediately terminate all 8 instances to save costs

**B.** Use **CloudWatch detailed metrics** and **AWS Compute Optimizer** to get specific downsizing recommendations (e.g., move to m5.large or m5.xlarge)

**C.** Switch all 8 instances to **T3 burstable instances** if the workload has occasional spikes but is generally low utilization

**D.** Move all instances to Spot Instances immediately since they're not heavily used

**E.** Increase the instances to m5.8xlarge to get better performance per dollar

---

**✅ Correct Answers: B and C**

**Explanation:**

- **B is correct** — **AWS Compute Optimizer** analyzes CloudWatch metrics and uses machine learning to recommend optimal instance types. It specifically looks at CPU, memory, network, and disk utilization patterns. This is the proper data-driven approach to right-sizing.

- **C is correct** — **T3 instances** (burstable) are perfect for workloads with low average utilization but occasional spikes. They accumulate CPU credits during low-usage periods and use them during bursts. For workloads at 5% average CPU, T3 instances can provide **significant cost savings** (T3 instances cost ~30-40% less than M5 equivalents).

- **A is wrong** — You should never terminate instances based on utilization alone without understanding WHY they have low CPU. The workload might be memory or I/O bound, or critical processes might run briefly. Analyze first.

- **D is wrong** — Spot Instances can be interrupted. For general workloads where you don't know the application's fault tolerance, recommending Spot without analysis is risky.

- **E is wrong** — Scaling UP when utilization is already low makes absolutely no sense and would increase costs significantly.

> 🪤 **Exam Trap:** AWS Compute Optimizer is often forgotten. It's the purpose-built service for right-sizing recommendations using ML. Don't just use Trusted Advisor — Compute Optimizer gives more specific and accurate recommendations.

---

## MCQ 18 — SCP and Tagging in Organizations

**Scenario:** A company with 50 AWS accounts under AWS Organizations wants to enforce that NO EC2 instances can be launched in ANY account unless they have the tags `CostCenter`, `Environment`, and `Owner`. The enforcement should work even if a member account administrator tries to modify their own IAM policies to bypass it. Which solution achieves this?

**A.** Apply IAM policies in each of the 50 accounts requiring the tags at launch

**B.** Use AWS Config `required-tags` rules deployed via AWS Organizations to all accounts

**C.** Create a **Service Control Policy (SCP)** at the Organization root level that denies `ec2:RunInstances` unless all three tags are present with `aws:RequestTag` conditions

**D.** Use AWS Control Tower guardrails to enforce tagging

**E.** Create a Lambda function that monitors CloudTrail and terminates untagged instances across all accounts

---

**✅ Correct Answers: C and D**

**Explanation:**

- **C is correct** — **SCPs at the Organization root level** apply to ALL accounts and ALL IAM entities (including account administrators and root users, except the master account root). An SCP with Deny on `ec2:RunInstances` unless tags are present cannot be overridden by member account admins. This is exactly what SCPs are designed for.

```json
{
  "Effect": "Deny",
  "Action": "ec2:RunInstances",
  "Resource": "arn:aws:ec2:*:*:instance/*",
  "Condition": {
    "Null": {
      "aws:RequestTag/CostCenter": "true",
      "aws:RequestTag/Environment": "true",
      "aws:RequestTag/Owner": "true"
    }
  }
}
```

- **D is correct** — **AWS Control Tower guardrails** include mandatory tagging rules that can be enforced across all managed accounts. Control Tower uses SCPs under the hood for preventive guardrails.

- **A is wrong** — IAM policies in individual accounts CAN be modified by account administrators. This doesn't provide centralized, tamper-proof enforcement.

- **B is wrong** — Config rules detect but don't prevent. Account admins could also disable Config in their accounts (though Organizations Config deployment helps with consistency).

- **E is wrong** — This is reactive, not preventive. There's a window between instance launch and Lambda termination where the instance is running untagged.

> 🪤 **Exam Trap:** The key phrase "even if a member account administrator tries to bypass it" signals that you need **SCPs** — not IAM policies (which can be modified at the account level). SCPs override all IAM policies in member accounts.

---

## MCQ 19 — Cost and Usage Report

**Scenario:** A large enterprise wants the MOST granular and comprehensive AWS cost data possible. They want to: analyze costs at the resource level (individual EC2 instance IDs), integrate the data with their existing data warehouse (Amazon Redshift), run SQL queries on cost data, and keep 3 years of historical data. Which service is BEST suited and what additional services are needed? (Select THREE)

**A.** AWS Cost Explorer — provides resource-level cost data with 13 months history

**B.** **AWS Cost and Usage Report (CUR)** — provides the most granular, resource-level billing data

**C.** Configure CUR to **deliver data to an S3 bucket** in Parquet or CSV format

**D.** Use **Amazon Athena** to query CUR data directly from S3 using SQL, then connect to Redshift

**E.** Use AWS Budgets API to pull 3 years of historical cost data into Redshift

---

**✅ Correct Answers: B, C, and D**

**Explanation:**

- **B is correct** — **AWS Cost and Usage Report (CUR)** is the most comprehensive billing dataset in AWS. It provides hourly/daily/monthly data at the resource level (including individual EC2 instance IDs, EBS volume IDs, etc.), which Cost Explorer cannot match in granularity.

- **C is correct** — CUR delivers data to an **S3 bucket** in CSV or Parquet format. Parquet is more efficient for analytical queries. From S3, data can be retained as long as needed (3 years is easily achievable with S3 lifecycle policies).

- **D is correct** — **Amazon Athena** can query CUR data directly from S3 using standard SQL. AWS even provides a **CUR integration with Athena** using a CloudFormation template. Athena results can be visualized or loaded into Redshift Spectrum.

- **A is wrong** — Cost Explorer only keeps **13 months** of historical data and doesn't offer resource-level granularity for all resources. It doesn't integrate with Redshift directly.

- **E is wrong** — AWS Budgets API provides budget status data, not historical cost data. It cannot provide 3 years of granular resource-level cost history.

> 🪤 **Exam Trap:** Cost Explorer = easy UI, 13 months, service-level granularity. CUR = complex, unlimited retention, resource-level granularity. When you see "most granular" or "resource ID level" — the answer is CUR, not Cost Explorer.

---

## MCQ 20 — Comprehensive Scenario

**Scenario:** A company has the following requirements:
1. Shut down ALL `Environment=Dev` instances every night at 10 PM automatically
2. Alert the DevOps team when monthly EC2 costs exceed $8,000
3. Identify which EC2 instances have been idle (< 10% CPU) for 2+ weeks
4. Prevent any new EC2 instance from launching without a `Project` tag
5. Generate a weekly report showing cost breakdown by department using the `Department` tag

Which of the following correctly maps EACH requirement to the RIGHT AWS service?

**A.**
1→Lambda+EventBridge, 2→CloudWatch Billing Alarm, 3→Trusted Advisor, 4→IAM Policy, 5→Cost Explorer

**B.**
1→CloudWatch Events only, 2→AWS Budgets, 3→Cost Explorer, 4→AWS Config, 5→Trusted Advisor

**C.**
1→Lambda+EventBridge, 2→AWS Budgets, 3→Trusted Advisor, 4→AWS Config, 5→Cost Explorer

**D.**
1→EC2 Auto Scaling, 2→AWS Budgets, 3→Trusted Advisor, 4→IAM Policy, 5→Cost Explorer

**E.**
1→Lambda+EventBridge, 2→AWS Budgets, 3→Trusted Advisor, 4→IAM Policy, 5→Cost Explorer

---

**✅ Correct Answers: A and E**

**Explanation:**

Let's evaluate each requirement:

| Req | Best Service | Why |
|---|---|---|
| 1. Nightly shutdown by tag | Lambda + EventBridge (CloudWatch Events) | Serverless scheduled automation based on tags |
| 2. Alert when EC2 > $8,000 | **AWS Budgets** (preferred) OR **CloudWatch Billing Alarm** | Both are valid; Budgets is more feature-rich |
| 3. Identify idle instances | **AWS Trusted Advisor** | Has specific check: EC2 instances with < 10% CPU for 14 days |
| 4. Prevent launch without tag | **IAM Policy** with `aws:RequestTag` condition | PREVENTS — not detects. Config detects only |
| 5. Cost by Department tag | **Cost Explorer** with activated cost allocation tags | Group by tag dimension |

- **A is correct** — Uses CloudWatch Billing Alarm for #2 (valid alternative to Budgets)
- **E is correct** — Uses AWS Budgets for #2 (preferred/more feature-rich)

- **B is wrong** — Maps #4 to AWS Config (Config DETECTS, doesn't PREVENT) and #3 to Cost Explorer (wrong — Trusted Advisor identifies idle instances)

- **C is wrong** — Maps #4 to AWS Config. This is the key error — Config cannot PREVENT resource creation.

- **D is wrong** — Maps #1 to EC2 Auto Scaling (Auto Scaling doesn't stop instances based on tags on a schedule).

> 🪤 **Final Master Exam Trap:** The most tested confusion in this entire module:
> - **PREVENT** tag enforcement = **IAM Policy**
> - **DETECT** tag compliance = **AWS Config**
> - **ALERT** on cost = **AWS Budgets** or **CloudWatch Billing Alarm (us-east-1 only)**
> - **ANALYZE** cost = **Cost Explorer**
> - **IDENTIFY** idle resources = **Trusted Advisor**
> - **SCHEDULE** automation = **Lambda + EventBridge**

---

## 📊 Quick Score Summary Table

| MCQ | Topic Tested | Key Trap |
|---|---|---|
| 1 | Tag enforcement: IAM vs Config | Config doesn't prevent |
| 2 | CloudWatch billing alarm region | Must be us-east-1 |
| 3 | Trusted Advisor tiers | Core vs full checks |
| 4 | Cost allocation tag activation | Must activate in billing console |
| 5 | Serverless stopinator | Lambda+EventBridge not EC2 |
| 6 | Tag case sensitivity | CostCenter ≠ costcenter |
| 7 | Config remediation | Config+SSM for auto-remediation |
| 8 | RI vs Spot for production | Spot = not for production |
| 9 | Waste identification tools | Inspector ≠ cost tool |
| 10 | Tag limits | aws: tags don't count toward 50 |
| 11 | Multi-account consolidated billing | Tags activated in master account |
| 12 | Lambda pricing model | 1M free requests/month |
| 13 | Auto-remediation options | CloudTrail can't remediate |
| 14 | Explorer vs Budgets | Explorer doesn't alert |
| 15 | Trusted Advisor checks scope | VPC flow logs = Config rule |
| 16 | ASG tag propagation | Existing instances not retroactively tagged |
| 17 | Right-sizing tools | Compute Optimizer for ML recommendations |
| 18 | SCP vs IAM for enforcement | SCP bypasses account admin |
| 19 | CUR vs Cost Explorer | CUR = most granular |
| 20 | Full service mapping | Prevent=IAM, Detect=Config, Alert=Budgets |

---

> 💡 **Pro Tip for SAA Exam:** When you see any question about cost management, immediately categorize it: Is it about PREVENTING, DETECTING, ALERTING, ANALYZING, or AUTOMATING? Each has a dedicated AWS service. Map the requirement to the right category first, then pick the service.
