# Module 9 — Monitoring & Security

# 🟦 Topic 1: Amazon CloudWatch — In Depth

## What is CloudWatch?

Think of CloudWatch as the **"nervous system"** of your AWS infrastructure. Just like your body's nervous system collects signals from every organ and alerts your brain when something is wrong, CloudWatch collects signals (metrics, logs, events) from every AWS service and alerts you when something needs attention.

---

## The 3 Pillars of CloudWatch

| Pillar | What it does | Analogy |
|---|---|---|
| **Monitoring** | Tracks metrics, fires alarms | A doctor monitoring your vitals |
| **Logs** | Collects & searches log files | A security camera recording footage |
| **Events** | Reacts to changes in real-time | A motion sensor triggering an alarm |

---

## Pillar 1 — CloudWatch Monitoring (Metrics & Alarms)

### What is a Metric?
A metric is a **time-ordered set of data points**. Think of it like a graph over time — every 5 minutes (or 1 minute), AWS sends a data point to CloudWatch.

**Examples of standard metrics (free, auto-collected):**
- EC2 → CPU Utilization, Network In/Out, Disk Read/Write
- S3 → BucketSizeBytes, NumberOfObjects
- RDS → DatabaseConnections, FreeStorageSpace
- Lambda → Invocations, Errors, Duration, Throttles

> ⚠️ **Critical exam fact:** EC2 does NOT send memory utilization or disk space metrics by default. These are inside the OS — invisible to the hypervisor. You need the **CloudWatch Agent** installed on the instance to collect these.

### Metric Anatomy — 3 things that uniquely identify a metric:

```
Namespace   →  AWS/EC2          (which service)
Metric Name →  CPUUtilization   (what is being measured)
Dimension   →  InstanceId: i-0abc123  (which specific resource)
```

### Standard vs Custom Metrics

| | Standard Metrics | Custom Metrics |
|---|---|---|
| Cost | Free | Paid |
| Who creates | AWS automatically | You (via Agent, CLI, or API) |
| Examples | CPU, Network, Disk | Memory usage, active users, order count |
| Namespace | AWS/EC2, AWS/RDS etc. | Your own (e.g., MyApp/WebServer) |

### CloudWatch Alarms — 3 States

```
OK                →  Metric is within threshold ✅
ALARM             →  Metric crossed the threshold 🔴
INSUFFICIENT_DATA →  Not enough data yet (alarm just created, or metric missing) ⚪
```

> ⚠️ **Exam trap:** ALARM does not mean something is broken. It just means a threshold was crossed. You define what that threshold means.

### How Alarms Work — The "N consecutive periods" Rule

Imagine your CPU threshold is 70%, and you set **3 consecutive periods** before alarm triggers:

```
Period 1: CPU = 80%  → over threshold (1/3)
Period 2: CPU = 85%  → over threshold (2/3)
Period 3: CPU = 90%  → over threshold (3/3) → 🔴 ALARM FIRES
```

But if:
```
Period 1: CPU = 80%  → over (1/3)
Period 2: CPU = 40%  → under → counter resets
Period 3: CPU = 90%  → over (1/3) → no alarm yet
```

This prevents false alarms from momentary spikes.

### What can an alarm DO when it fires?

1. **Send SNS notification** → email, SMS, or trigger a Lambda
2. **Send to SQS** → create a work item for a system to process
3. **Trigger Auto Scaling** → add or remove EC2 instances
4. **EC2 Action** → stop, terminate, reboot, or recover an instance

---

## Basic vs Detailed Monitoring

| | Basic Monitoring | Detailed Monitoring |
|---|---|---|
| Frequency | Every **5 minutes** | Every **1 minute** |
| Cost | **Free** (included) | **Paid** |
| How to enable | Default | Enable per instance |

> ⚠️ **Exam tip:** If a question says "near real-time" or "1-minute granularity" — the answer involves **detailed monitoring**.

---

## 🎯 Scenario-Based Questions (Think Like the Exam)

**Scenario 1:**
> Your e-commerce app runs on EC2. During a flash sale, you notice the servers slow down but your CloudWatch CPU alarm never fires. Why?

**Answer:** CloudWatch basic monitoring reports every 5 minutes. The spike may have lasted only 2–3 minutes and was missed. Solution: enable **detailed monitoring** (1-min intervals) + lower the alarm evaluation period.

---

**Scenario 2:**
> Your manager asks: "Is memory utilization being monitored on our EC2 instances?" Your team confirms CloudWatch is set up. Is memory being monitored?

**Answer:** NO. Memory utilization is not a standard EC2 metric — it's inside the OS. You must install the **CloudWatch Agent** on each instance and configure it to collect memory metrics as a **custom metric**.

---

**Scenario 3:**
> A company wants to be alerted if their AWS bill exceeds $500 in a month. What must they do?

**Answer:**
1. Enable **billing alerts** in account preferences (not on by default)
2. Create a **CloudWatch billing alarm** on the `EstimatedCharges` metric
3. Set an **SNS notification** to email the finance team

---

**Scenario 4:**
> You have a critical EC2 instance. If it fails, you want it to automatically restart. How?

**Answer:** Create a CloudWatch Alarm on the **StatusCheckFailed_System** metric, and set the alarm action to **EC2 Recover**. This automatically recovers the instance on the same host or moves it to a new host.

---

## 🔑 Key Facts to Memorize for SAA

- Basic monitoring = **5 min**, free | Detailed = **1 min**, paid
- Memory & disk metrics on EC2 = **NOT** available by default → need **CloudWatch Agent**
- Alarm states: **OK / ALARM / INSUFFICIENT_DATA**
- Alarm actions: SNS, SQS, Auto Scaling, EC2 actions
- Metrics expire after **15 months** of no new data
- Billing alarms need to be **manually enabled** in account settings
- Custom metrics are published via **CLI, API, or CloudWatch Agent**

---
# 🟦 Topic 2: Amazon CloudWatch Events (EventBridge)


## What Problem Does CloudWatch Events Solve?

In Topic 1, you learned that CloudWatch **reacts to metric thresholds** — like "alert me when CPU > 70%."

But what if you don't care about a metric value — you care about **something happening**?

> "The moment a new EC2 instance launches → automatically run a configuration script on it."
> "Every night at midnight → trigger a Lambda to back up my database."
> "The moment an IAM user is created → notify the security team."

**This is exactly what CloudWatch Events is for** — reacting to *events* (state changes), not just metric values.

---

## The Core Concept — 3 Building Blocks

```
EVENT  →  RULE  →  TARGET
```

Think of it like an **if-then automation system:**

| Block | What it is | Real-world analogy |
|---|---|---|
| **Event** | Something that happened in AWS | "Motion detected" |
| **Rule** | The filter — which events do you care about? | "Only motion in the backyard" |
| **Target** | What to DO when the rule matches | "Turn on the floodlight" |

---

## Block 1 — Events

An event is a **JSON message** that AWS automatically generates whenever something changes in your environment.

**Examples of events:**
- EC2 instance state changes (pending → running → stopped → terminated)
- Auto Scaling launches or terminates an instance
- An S3 object is uploaded
- An IAM user is created or deleted
- A CloudTrail API call is made
- A scheduled time is reached (cron-based)

Every event looks like this in JSON:
```json
{
  "source": "aws.ec2",
  "detail-type": "EC2 Instance State-change Notification",
  "detail": {
    "instance-id": "i-0abc12345",
    "state": "running"
  }
}
```

> ⚠️ **Exam insight:** You can also create **custom events** from your own applications and publish them — not just AWS service events.

---

## Block 2 — Rules

A rule **matches** incoming events and decides which target to send them to.

**Two types of rules:**

### Type 1 — Event-based Rules
Triggered when something happens:
```
"When any EC2 instance enters the 'running' state → run SSM script"
"When any IAM user is created → send SNS alert to security team"
"When any S3 object is deleted → trigger Lambda for audit"
```

### Type 2 — Schedule-based Rules (Cron)
Triggered at a specific time:
```
"Every day at 2 AM → trigger Lambda to create RDS snapshot"
"Every Monday at 9 AM → send weekly cost report"
"Every 5 minutes → check if a service is healthy"
```

**Cron expression format:**
```
cron(Minutes  Hours  DayOfMonth  Month  DayOfWeek  Year)
cron(0        2      *           *      MON-FRI     *)   → 2 AM, Mon-Fri
```

---

## Block 3 — Targets

A rule can route to **one or more targets simultaneously** (parallel processing):

| Target | Use case |
|---|---|
| **AWS Lambda** | Run custom code |
| **SNS** | Send notification |
| **SQS** | Queue a work item |
| **EC2 Run Command (SSM)** | Run a script on an instance |
| **Step Functions** | Trigger a workflow |
| **Kinesis** | Stream event data |
| **ECS Task** | Run a container |
| **CodePipeline** | Trigger a CI/CD pipeline |

> ⚠️ **Key exam fact:** One rule → multiple targets, all processed **in parallel**. Different teams/systems can independently react to the same event.

---

## CloudWatch Events vs EventBridge — What's the Difference?

> ⚠️ **Important for the exam:** AWS renamed CloudWatch Events to **Amazon EventBridge** in 2020. They are the **same underlying service**. The SAA exam may use either name.

| | CloudWatch Events | Amazon EventBridge |
|---|---|---|
| Name | Old name | New name |
| Function | Identical | Identical + more features |
| Extra features in EventBridge | — | Third-party SaaS event sources (e.g., Zendesk, Datadog), Schema Registry, Event Buses |
| Exam context | Both names appear | Prefer EventBridge in new questions |

Think of EventBridge as **CloudWatch Events, but grown up.**

---

## 📊 Comparison: CloudWatch Monitoring vs CloudWatch Events

This is where most students get confused. Here's the clearest way to think about it:

| | CloudWatch Monitoring (Topic 1) | CloudWatch Events (Topic 2) |
|---|---|---|
| **Trigger** | A metric VALUE crosses a threshold | Something HAPPENS (a state change) |
| **Question it answers** | "Is my CPU too high?" | "Did something just change?" |
| **Timing** | Delayed (5 min or 1 min intervals) | Near real-time (seconds) |
| **Example** | CPU > 80% for 3 periods → alert | EC2 instance launched → run script |
| **Input** | Numeric metric data | JSON event data |
| **Use case** | Performance monitoring | Automation & event-driven architecture |

**Simple memory trick:**
- CloudWatch Monitoring = **"How is it performing?"** (numbers over time)
- CloudWatch Events = **"What just happened?"** (instant reaction)

---

## 🎯 Scenario-Based Questions

**Scenario 1 — Auto-configuration of new servers:**
> A company has an Auto Scaling group that launches new EC2 instances during peak hours. Every new instance must have a security agent installed automatically. How do you achieve this without manual steps?

**Answer:**
1. Create a **CloudWatch Events rule** that triggers when EC2 instance state = `running`
2. Set the **target** to **AWS Systems Manager Run Command**
3. SSM Run Command automatically executes the security agent installation script on the new instance

This is exactly the example from your slides — zero human intervention needed.

---

**Scenario 2 — Nightly database backup:**
> A financial company needs their RDS database snapshot taken every night at 11 PM UTC, automatically. What's the simplest approach?

**Answer:**
1. Create a **scheduled CloudWatch Events rule** with cron: `cron(0 23 * * ? *)`
2. Set target to **AWS Lambda**
3. Lambda calls the RDS API to create a snapshot

---

**Scenario 3 — Security monitoring:**
> Your security team wants to be notified within seconds whenever someone creates a new IAM user in the AWS account — before any damage can be done.

**Answer:**
1. **CloudTrail** captures the `CreateUser` API call as an event
2. **CloudWatch Events rule** matches on `source: aws.iam` + `eventName: CreateUser`
3. Target = **SNS** → sends email/SMS to the security team instantly

> Notice how CloudTrail + CloudWatch Events work together here — this is a favourite SAA exam combo.

---

**Scenario 4 — Multi-team reaction:**
> When a production EC2 instance is terminated unexpectedly, you want to: (a) alert the DevOps team via email, (b) log the event to a database, and (c) trigger a Lambda to spin up a replacement.

**Answer:**
One **CloudWatch Events rule** with **3 targets in parallel:**
- Target 1 → **SNS** (email to DevOps)
- Target 2 → **Kinesis** (stream event to database)
- Target 3 → **Lambda** (auto-recovery logic)

All three fire **simultaneously** — this is the power of parallel targets.

---

**Scenario 5 — Cost vs speed tradeoff (classic SAA trap):**
> A solutions architect needs to trigger an action when CPU utilization stays above 90% for 10 minutes. Should they use CloudWatch Monitoring alarms or CloudWatch Events?

**Answer:** **CloudWatch Monitoring alarm** — because the trigger is based on a *metric value sustained over time*, not an instantaneous state change. CloudWatch Events reacts to things that *happen*, not sustained metric conditions.

---

## 🔑 Key Facts to Memorize for SAA

- CloudWatch Events = **EventBridge** (same thing, new name)
- Three components: **Event → Rule → Target**
- Two rule types: **event-based** (something happens) vs **schedule-based** (cron)
- One rule can trigger **multiple targets in parallel**
- Events are delivered in **near real-time** (seconds)
- CloudWatch Events reacts to **state changes**; CloudWatch Monitoring reacts to **metric thresholds**
- Common targets: Lambda, SNS, SQS, SSM Run Command, Step Functions
- You can publish **custom events** from your own applications

---

## 🧠 Running Comparison Table (Updated After Topic 2)

| Feature | CloudWatch Monitoring | CloudWatch Events |
|---|---|---|
| What triggers it | Metric threshold crossed | State change / scheduled time |
| Speed | 5 min (basic) / 1 min (detailed) | Near real-time |
| Output | Alarm state change | Triggers a target |
| Best for | Performance alerts | Automation & orchestration |
| Works with | SNS, SQS, Auto Scaling | Lambda, SNS, SQS, SSM, Step Functions, and more |
| Cron support | No | Yes |

---
# 🟦 Topic 3: Amazon CloudWatch Logs — In Depth

## What Problem Does CloudWatch Logs Solve?

In a traditional data center, logs live on the server's hard drive. When the server dies — logs are gone forever. In AWS, EC2 instances are disposable — they can be terminated at any time by Auto Scaling.

**CloudWatch Logs solves 3 problems:**

1. **Persistence** — logs survive even after the instance is terminated
2. **Centralization** — aggregate logs from 100 instances into one place
3. **Intelligence** — search, filter, and alert on log content in real time

---

## The Architecture — 5 Key Concepts

```
EC2 Instance                    CloudWatch Logs
┌─────────────────┐            ┌──────────────────────────────┐
│  App log file   │            │  Log Group: /app/webserver   │
│  /var/log/httpd │──Agent──▶  │  ├── Log Stream: i-0abc111   │
│                 │            │  ├── Log Stream: i-0abc222   │
│  System log     │            │  └── Log Stream: i-0abc333   │
│  /var/log/syslog│──Agent──▶  │                              │
└─────────────────┘            │  Metric Filter → Custom      │
                               │  Metric → Alarm → SNS        │
                               └──────────────────────────────┘
```

### Concept 1 — CloudWatch Agent
- A software agent you install on EC2 instances (or on-premises servers)
- Watches specified log files and streams them to CloudWatch Logs
- Also collects system-level metrics (memory, disk) that standard monitoring misses
- **Without the agent, CloudWatch Logs gets nothing from EC2**

> ⚠️ **Exam trap:** CloudWatch Logs does NOT automatically collect logs from EC2. You must install and configure the agent. This is one of the most tested facts.

### Concept 2 — Log Groups
- A **log group** is a container for logs of the same type
- Example: all Apache access logs from all your web servers → one log group called `/app/webserver/access`
- You set **retention policy** at the log group level (1 day to 10 years, or never expire)
- By default, logs **never expire** — this costs money if you're not careful

> ⚠️ **Exam tip:** If a question asks about controlling log storage costs → set a **retention policy** on the log group.

### Concept 3 — Log Streams
- A **log stream** is the sequence of log events from a **single source** (one EC2 instance, one Lambda function)
- Multiple log streams live inside one log group
- Think: Log Group = the filing cabinet | Log Stream = one folder per server

### Concept 4 — Metric Filters
This is the most powerful and most tested concept in CloudWatch Logs.

**The idea:** Scan log data for a pattern → every match increments a custom metric → use that metric like any other CloudWatch metric (alarms, dashboards)

**The flow:**
```
Log Group
    ↓
Metric Filter (pattern: "ERROR" or "[status=4*]")
    ↓
Custom CloudWatch Metric (e.g., "ErrorCount")
    ↓
CloudWatch Alarm (ErrorCount > 10 in 5 minutes)
    ↓
SNS → Email/SMS to on-call engineer
```

**Filter pattern examples:**

| Pattern | What it matches |
|---|---|
| `ERROR` | Any log line containing the word ERROR |
| `"404"` | Lines containing exactly 404 |
| `[ip, user, username, timestamp, request=*html*, status=4*, bytes]` | Apache logs with HTML requests that returned 4xx errors |
| `{ $.level = "ERROR" }` | JSON logs where the level field = ERROR |

> ⚠️ **Critical exam facts about filters:**
> - Filters are **case-sensitive**
> - If multiple terms in a filter, **ALL must appear** in the same log line
> - Filters apply **going forward** — they don't backfill historical data

### Concept 5 — CloudWatch Logs Insights
- A built-in **query engine** for your log data
- Uses its own simple but powerful query language (NOT SQL)
- Great for ad-hoc investigation: "Show me all 500 errors from the last 3 hours"
- Can create **visualizations** (bar charts, time series) from query results

**Example query:**
```
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

> ⚠️ **Exam distinction:** Logs Insights = interactive ad-hoc queries. Metric Filters = ongoing automated monitoring. Both live in CloudWatch Logs but serve different purposes.

---

## The 3-Phase Log Analysis Process (From Your Slides)

```
Phase 1: CONFIGURE          Phase 2: COLLECT           Phase 3: ANALYZE
─────────────────           ────────────────           ────────────────
Decide what to log          Agent streams logs          Query with Logs Insights
Choose log format           from instances to           Create metric filters
Set retention policy        CloudWatch Logs             Build dashboards
                            Logs survive instance       Set alarms on patterns
                            termination                 Investigate incidents
```

---

## Real Apache Log Format — The Exam Will Test This

Your slides include this real Apache log example:
```
127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
```

Breaking it down:

| Field | Value | Meaning |
|---|---|---|
| `%h` | 127.0.0.1 | Client IP address |
| `%l` | `-` | Client identity (usually unknown) |
| `%u` | frank | Authenticated username |
| `%t` | [10/Oct/2000:13:55:36] | Timestamp |
| `%r` | GET /apache_pb.gif HTTP/1.0 | Request line |
| `%s` | 200 | HTTP status code |
| `%b` | 2326 | Response size in bytes |

**Why this matters for the exam:** Understanding log formats helps you write correct metric filter patterns. A filter for 404 errors would look for `status=404` or `%s=404` in the Apache log structure.

---

## How CloudWatch Logs Connects to Topics 1 & 2

```
┌─────────────────────────────────────────────────────────┐
│                  CloudWatch Ecosystem                    │
│                                                          │
│  CloudWatch Logs                                         │
│       │                                                  │
│       │── Metric Filter ──▶ Custom Metric               │
│                                    │                     │
│                                    ▼                     │
│                            CloudWatch Alarm (Topic 1)   │
│                                    │                     │
│                                    ▼                     │
│                             SNS / Auto Scaling           │
│                                                          │
│  CloudWatch Events (Topic 2)                             │
│       │                                                  │
│       └── Can trigger Lambda to PROCESS log data        │
└─────────────────────────────────────────────────────────┘
```

---

## 🎯 Scenario-Based Questions

**Scenario 1 — The Disappearing Logs Problem:**
> A company runs a web app on a fleet of EC2 instances managed by Auto Scaling. During an incident last week, instances were terminated before engineers could check the logs. How do you prevent losing logs in the future?

**Answer:**
- Install the **CloudWatch Agent** on every EC2 instance (bake it into the AMI or use SSM)
- Configure the agent to stream `/var/log/httpd/access_log` and `/var/log/httpd/error_log` to a **CloudWatch Log Group**
- Set a **retention policy** of 90 days on the log group
- Now logs persist in CloudWatch even after instances are terminated

> **Bonus:** Use a **CloudWatch Events rule** (Topic 2!) to automatically install the agent on every new instance launched — combining all three topics.

---

**Scenario 2 — Real-time Error Detection:**
> A company's e-commerce site runs on EC2. The SLA requires that if more than 50 HTTP 500 errors occur within 5 minutes, the on-call engineer must be paged immediately. How do you implement this?

**Answer — step by step:**

```
Step 1: Install CloudWatch Agent on EC2 instances
        → streams Apache error logs to log group "/ecommerce/apache/error"

Step 2: Create a Metric Filter on that log group
        → pattern: [ip, user, username, timestamp, request, status=5*, bytes]
        → filter name: "HTTP500Errors"
        → metric name: "HTTP500Count"

Step 3: Create a CloudWatch Alarm
        → metric: HTTP500Count
        → threshold: > 50
        → evaluation period: 1 period of 5 minutes
        → alarm state → action: publish to SNS topic

Step 4: SNS topic
        → subscriber: PagerDuty webhook or engineer's phone (SMS)
```

All of this is fully automated — zero manual checking required.

---

**Scenario 3 — Cost Control:**
> A startup is receiving a surprise bill from CloudWatch Logs. Their logs are growing rapidly and they never set an expiry. What should they do?

**Answer:**
- Set a **log retention policy** on each log group (e.g., 30 days)
- CloudWatch Logs will automatically delete log data older than the retention period
- Also consider **exporting old logs to S3** (much cheaper storage) using export tasks
- Use **S3 Lifecycle policies** to move S3 logs to Glacier after 90 days for even cheaper archival

> ⚠️ **Exam tip:** S3 is much cheaper than CloudWatch Logs for long-term log storage. For logs older than your active investigation window, export to S3.

---

**Scenario 4 — Security Investigation (Combines Topics 1, 2, 3):**
> A hospital's security team suspects someone is trying to brute-force SSH into their EC2 instances. They want automated detection and response. Design a solution.

**Answer — full architecture:**

```
EC2 Instance
    │
    │ CloudWatch Agent streams /var/log/secure (auth logs)
    ▼
CloudWatch Log Group: "/hospital/ec2/auth"
    │
    │ Metric Filter: pattern = "Failed password"
    ▼
Custom Metric: "SSHFailedAttempts"
    │
    │ CloudWatch Alarm: SSHFailedAttempts > 10 in 2 minutes
    ▼
SNS Topic
    ├──▶ Email to Security Team
    └──▶ Lambda Function
              │
              └──▶ Calls EC2 API to update Security Group
                   and BLOCK the offending IP address
```

This is a **fully automated security response system** using CloudWatch Logs + Monitoring + Events/Lambda together.

---

**Scenario 5 — Classic SAA Trap Question:**
> A developer sets up a CloudWatch Logs metric filter to count the word "ERROR" in application logs. An hour later, they check and the metric shows 0 even though there are clearly errors in the logs. What went wrong?

**Answer:** Metric filters only apply to **new log events going forward** — they do not backfill historical data. The errors that existed before the filter was created are not counted. This is a very commonly tested gotcha.

---

**Scenario 6 — Logs Insights vs Metric Filters:**
> A DevOps engineer needs to: (a) investigate what caused last night's outage by searching through logs, and (b) set up ongoing monitoring to alert if it happens again. Which CloudWatch Logs feature handles each task?

**Answer:**
- (a) **CloudWatch Logs Insights** — for the one-time, ad-hoc investigation of historical logs
- (b) **Metric Filter + CloudWatch Alarm** — for the ongoing automated monitoring going forward

Both are part of CloudWatch Logs but serve completely different purposes.

---

## 🧠 Full Running Comparison Table (After Topic 3)

| Feature | CW Monitoring | CW Events | CW Logs |
|---|---|---|---|
| **Data type** | Numbers (metrics) | JSON events | Raw text (log lines) |
| **Trigger** | Metric threshold | State change / cron | Log pattern match |
| **Speed** | 1–5 min intervals | Near real-time | Near real-time (streaming) |
| **Best for** | Performance alerts | Automation | Debugging & compliance |
| **Requires agent?** | No (standard metrics) | No | Yes (for EC2 logs) |
| **Retention** | 15 months (metrics) | N/A | Configurable (forever by default) |
| **Query tool** | CloudWatch console | N/A | Logs Insights |
| **Alarm support** | Native | No | Via Metric Filters |
| **Cost tip** | Custom metrics = paid | Free (within limits) | Export old logs to S3 |

---

## 🔑 Key Facts to Memorize for SAA

- CloudWatch Agent is **required** to collect logs from EC2 and on-premises servers
- CloudWatch Agent also collects **memory and disk** metrics (not available by default)
- **Log Group** = container for logs of same type | **Log Stream** = one source's logs
- Log retention is **never expire by default** — always set a retention policy
- **Metric Filters** = ongoing automated pattern matching → custom metric → alarm
- Metric filters only apply to **new log events** — no backfilling
- Filters are **case-sensitive** and require **all terms** to match in the same line
- **Logs Insights** = ad-hoc investigation with query language
- For long-term storage → **export to S3** (much cheaper)
- CloudWatch Logs integrates with **CloudTrail** (next topic!) for security auditing

---

# 🟦 Topic 4: AWS CloudTrail — In Depth




## The "Aha" Moment — Why CloudTrail Exists

Imagine you come into work Monday morning and find:
- An S3 bucket that was public-facing is now deleted
- A security group was modified to allow all inbound traffic
- An IAM user you don't recognize has Administrator access

**CloudWatch Logs** can tell you your app threw errors.
**CloudWatch Monitoring** can tell you traffic spiked.

But neither can tell you:
> **"Who did this? When? From where? Using which tool?"**

**This is exactly what AWS CloudTrail was built to answer.**

---

## What is CloudTrail — The Core Idea

Every action in AWS — clicking a button in the Console, running an AWS CLI command, or an application calling the AWS API — is ultimately an **API call**.

CloudTrail **records every single one of these API calls** and stores them as log entries.

```
You click "Launch Instance" in Console
         ↓
AWS API call: ec2:RunInstances
         ↓
CloudTrail records: WHO, WHAT, WHEN, WHERE, SUCCESS/FAIL
         ↓
Log stored in S3 bucket
```

> **Simple mental model:** CloudTrail is like a **CCTV camera for your entire AWS account** — recording every action taken by every person or service, at every moment.

---

## What Exactly Does CloudTrail Record?

Every CloudTrail log entry answers these questions:

| Question | CloudTrail Field | Example |
|---|---|---|
| **Who?** | `userIdentity` | IAM user "john", Root account, Lambda role |
| **What?** | `eventName` | `RunInstances`, `DeleteBucket`, `CreateUser` |
| **When?** | `eventTime` | `2024-03-15T14:32:01Z` |
| **Where from?** | `sourceIPAddress` | `203.0.113.42` (suspicious IP?) |
| **Which service?** | `eventSource` | `ec2.amazonaws.com`, `s3.amazonaws.com` |
| **Which region?** | `awsRegion` | `us-east-1` |
| **Via what tool?** | `userAgent` | Console, CLI, SDK |
| **Did it succeed?** | `responseElements` | `Success` or `Failure` + error message |

---

## Real CloudTrail Log Entry — Dissected

From your slides, here's a real CloudTrail JSON entry:

```json
{
  "eventVersion": "1.01",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAyyyyyyyyyyyyyyyy",
    "arn": "arn:aws:iam::123456789012:user/tests3user",
    "accountId": "123456789012",
    "userName": "tests3user"       ← WHO took the action
  },
  "eventTime": "2018-09-23T22:41:38Z",     ← WHEN
  "eventSource": "signin.amazonaws.com",    ← WHICH service
  "eventName": "ConsoleLogin",             ← WHAT action
  "awsRegion": "us-east-1",               ← WHERE (region)
  "sourceIPAddress": "54.240.217.10",      ← FROM where (IP)
  "userAgent": "Mozilla/5.0..Firefox/31.0" ← HOW (browser/tool)
}
```

**Success response:**
```json
"responseElements": {
  "ConsoleLogin": "Success"    ← login worked
},
"additionalEventData": {
  "MFAUsed": "No"              ← ⚠️ no MFA! security risk
}
```

**Failure response:**
```json
"errorMessage": "Failed authentication",
"responseElements": {
  "ConsoleLogin": "Failure"    ← login failed
}
```

> ⚠️ **Exam insight:** The `MFAUsed: No` field is gold for security audits. You can query CloudTrail to find all console logins where MFA was NOT used and flag those users.

---

## Two Modes of CloudTrail

### Mode 1 — Event History (Default, Always On)
- **Free**, always enabled, no configuration needed
- Shows last **90 days** of activity
- Only captures **management events** (create, modify, delete actions)
- Only shows **write** actions by default (CreateInstance, DeleteBucket, etc.)
- **Cannot** be turned off
- Accessible directly from the CloudTrail console

### Mode 2 — CloudTrail Trail (You Configure This)
- **Paid** (S3 storage costs)
- Stores logs **beyond 90 days** — indefinitely
- Captures **all event types** including data events
- Logs stored in **S3 bucket** you specify
- Can apply to **one region or all regions**
- Supports **log file encryption** (SSE-KMS) and **integrity validation**

> ⚠️ **Exam tip:** For compliance, auditing, or security — always configure a **Trail**. Event History alone is not sufficient for enterprise requirements.

---

## Three Types of Events CloudTrail Can Log

### Type 1 — Management Events (Control Plane)
- Actions performed ON AWS resources
- Examples: creating an EC2 instance, modifying a security group, deleting an S3 bucket
- **Logged by default** in Event History
- Also called **"control plane operations"**

### Type 2 — Data Events (Data Plane)
- Actions performed ON THE DATA inside resources
- Examples: S3 object-level operations (GetObject, PutObject, DeleteObject), Lambda function invocations
- **NOT logged by default** — must explicitly enable
- **High volume** — can generate millions of events
- **Extra cost** to enable

> ⚠️ **Exam scenario:** A company needs to track who accessed a specific S3 object containing sensitive data. → Enable **data events** for that S3 bucket in CloudTrail.

### Type 3 — Insight Events
- Detects **unusual API activity** automatically
- Examples: sudden spike in IAM CreateUser calls, burst of EC2 TerminateInstances
- Uses ML to establish a baseline and flag anomalies
- **Extra cost** to enable

---

## Configuring a CloudTrail Trail — Step by Step

From your slides, the configuration steps are:

```
Step 1: Choose S3 bucket
        → new or existing bucket to store log files
        → logs are delivered within ~15 minutes of the API call

Step 2: Define what to log
        → Management events (read-only, write-only, or all)
        → Data events (S3, Lambda — optional, costs more)
        → Insight events (optional, costs more)

Step 3: Create SNS topic (optional)
        → Get notified when new log files are delivered to S3

Step 4: Forward to CloudWatch Logs (optional)
        → Enables real-time alerting on CloudTrail events
        → Combines the power of CloudTrail + CloudWatch together

Step 5: Enable log file encryption
        → Uses SSE-KMS to encrypt logs at rest
        → Required for compliance (HIPAA, PCI-DSS, etc.)

Step 6: Enable log file integrity validation
        → CloudTrail creates a hash of each log file
        → Lets you detect if logs were tampered with
        → Critical for forensics and compliance

Step 7: Add tags
        → Organize and manage the trail
```

---

## CloudTrail + CloudWatch Logs — The Power Combo

This is heavily tested on the SAA exam. When you forward CloudTrail logs to CloudWatch Logs:

```
API Call Made
    ↓
CloudTrail captures it
    ↓
Delivered to CloudWatch Logs (real-time)
    ↓
Metric Filter scans for pattern
(e.g., "ConsoleLogin" + "Failure")
    ↓
Custom Metric: "FailedLogins"
    ↓
CloudWatch Alarm: FailedLogins > 5 in 10 minutes
    ↓
SNS → Security Team alerted immediately
```

> This is the standard architecture for **real-time security monitoring** of your AWS account.

---

## What CloudTrail CANNOT Do

> ⚠️ **Critical exam fact — very commonly tested:**

CloudTrail records **AWS API calls only**. It does NOT track:

| What happens | Tracked by CloudTrail? |
|---|---|
| Someone SSH's into EC2 and runs `sudo rm -rf /` | ❌ No |
| Someone manually shuts down an EC2 via SSH (`sudo shutdown`) | ❌ No |
| Someone edits a file on the EC2 filesystem | ❌ No |
| Someone calls `ec2:TerminateInstances` via CLI | ✅ Yes |
| Someone deletes an S3 bucket via Console | ✅ Yes |
| A Lambda function calls the RDS API | ✅ Yes |

**The rule:** If it goes through the **AWS API** → CloudTrail sees it. If it happens **inside the OS** → CloudTrail is blind to it.

For OS-level activity monitoring → use **CloudWatch Logs** (with the agent) or **AWS Systems Manager Session Manager**.

---

## CloudTrail Best Practices (SAA Loves These)

| Best Practice | Why |
|---|---|
| Enable trail in **all regions** | Attackers may operate in regions you don't use |
| Store logs in a **separate, dedicated S3 bucket** | Harder for an attacker to cover tracks |
| Enable **MFA Delete** on the log S3 bucket | Prevent logs from being deleted |
| Enable **log file integrity validation** | Detect tampering for forensics |
| Enable **SSE-KMS encryption** | Protect sensitive log content |
| Forward to **CloudWatch Logs** | Enable real-time alerting |
| Use a **separate AWS account** for log storage | Even if main account is compromised, logs survive |

---

## 🎯 Scenario-Based Questions

**Scenario 1 — The Classic "Who Did This?" Investigation:**
> On Monday morning, a Solutions Architect discovers that a production RDS database was deleted over the weekend. No tickets were filed for this change. How do you find out who deleted it?

**Answer:**
1. Go to **CloudTrail Event History** in the AWS Console
2. Filter by **Event name:** `DeleteDBInstance`
3. The log entry will show:
   - **userIdentity** → which IAM user or role did it
   - **eventTime** → exactly when it happened
   - **sourceIPAddress** → from which IP address
   - **userAgent** → Console, CLI, or SDK

If it happened more than 90 days ago → check the **CloudTrail Trail logs in S3**.

---

**Scenario 2 — Detecting Compromised Credentials:**
> Your security team suspects an IAM access key has been stolen. They want to detect if the stolen key is being used from unusual IP addresses. What do you set up?

**Answer:**
```
CloudTrail Trail (all regions)
    ↓ forward to
CloudWatch Logs
    ↓
Metric Filter: filter on sourceIPAddress NOT in approved IP ranges
    ↓
Custom Metric: "SuspiciousAPIActivity"
    ↓
CloudWatch Alarm → SNS → Security team paged immediately
```

Additionally, use **CloudTrail Insights** to automatically detect unusual spikes in API call patterns.

---

**Scenario 3 — Compliance Audit:**
> A financial company must prove to auditors that no one has modified their production security groups in the last 6 months. How do you provide this evidence?

**Answer:**
1. CloudTrail **Trail** must have been configured (not just Event History — that's only 90 days)
2. Query the S3 bucket containing CloudTrail logs
3. Filter for `eventName: AuthorizeSecurityGroupIngress` or `RevokeSecurityGroupEgress` etc.
4. If no such events exist → proven that security groups were not modified
5. For easier querying → use **Amazon Athena** to run SQL on the S3 log files (Topic 5!)

---

**Scenario 4 — S3 Data Access Tracking:**
> A healthcare company stores patient records in S3. HIPAA requires them to log every time a patient record is accessed (read). How do you implement this?

**Answer:**
- Enable **CloudTrail data events** for the specific S3 bucket
- Select **Read** events (GetObject)
- Every access is now logged with: who accessed it, when, from where
- Store logs in a separate, encrypted, MFA-protected S3 bucket

> ⚠️ Remember: data events are NOT on by default and cost extra — but for compliance, they're essential.

---

**Scenario 5 — Multi-Account Organization:**
> A large enterprise has 50 AWS accounts across different departments. The security team needs centralized visibility into all API activity across all accounts. What do you recommend?

**Answer:**
- Use **AWS Organizations** with a dedicated **Security/Audit account**
- Create an **Organization Trail** in CloudTrail
- This automatically applies the trail to **all accounts** in the organization
- All logs flow to a **central S3 bucket** in the Security account
- Even if a developer account is compromised, the attacker cannot delete the logs (different account)

---

**Scenario 6 — The Tricky Elimination Question:**
> A company wants to monitor their AWS account for security threats. They want to know: (a) when an EC2 instance type is changed, (b) when someone logs into the console without MFA, (c) when memory utilization on EC2 exceeds 90%. Which service handles each?

**Answer:**

| Requirement | Service | Why |
|---|---|---|
| (a) EC2 instance type changed | **CloudTrail** | It's an API action (ModifyInstanceAttribute) |
| (b) Console login without MFA | **CloudTrail** | Console login = API call, MFAUsed field available |
| (c) Memory > 90% on EC2 | **CloudWatch Monitoring** + **Agent** | Metric threshold, not an API call |

> This type of multi-part scenario is very common in SAA exams. Know which service owns which domain.

---

## 🧠 Full Running Comparison Table (After Topic 4)

| Feature | CW Monitoring | CW Events | CW Logs | CloudTrail |
|---|---|---|---|---|
| **Data type** | Numbers | JSON events | Raw text | API call records |
| **Question** | How is it performing? | What just happened? | What did it say? | Who did what to AWS? |
| **Scope** | Resource metrics | AWS state changes | Application/system logs | AWS API activity |
| **Inside EC2?** | No (needs agent) | No | Yes (needs agent) | No |
| **Default retention** | 15 months | N/A | Never expires | 90 days free |
| **Long-term storage** | N/A | N/A | Export to S3 | S3 via Trail |
| **Real-time alerts** | Yes (alarms) | Yes (rules) | Yes (metric filters) | Via CW Logs integration |
| **Compliance use** | Performance SLAs | Automation | App audit logs | API audit trail |
| **Key limitation** | No app-level detail | No metric data | No AWS API visibility | No OS-level activity |

---

## 🔑 Key Facts to Memorize for SAA

- CloudTrail records **every AWS API call** — Console, CLI, SDK, and service-to-service
- Default **Event History = 90 days, free, management events only**
- **Trail = your configuration** for long-term storage beyond 90 days in S3
- Three event types: **Management** (default), **Data** (extra cost), **Insights** (ML anomaly detection)
- CloudTrail **cannot** see inside EC2 instances — only API-level actions
- For **real-time alerting** on CloudTrail events → forward to **CloudWatch Logs** + metric filter + alarm
- Enable **log integrity validation** to detect tampering (critical for forensics)
- Enable trail in **all regions** — attackers use regions you don't monitor
- For **SQL querying of CloudTrail logs** → use **Amazon Athena** (Topic 5 — next!)
- Separate **S3 bucket + separate AWS account** for logs = best security practice

---

# 🔎 TOPIC 5: Amazon Athena

## The Problem Athena Solves

By now you know CloudTrail stores logs in S3 as raw JSON files. But imagine:

- Your company has been running for 2 years
- CloudTrail has generated **50 million log entries** across hundreds of JSON files in S3
- An auditor asks: *"Show me every action taken by user 'john' between March 1–15 that resulted in a failure"*

**Without Athena:** You'd have to download files, write custom scripts, parse JSON manually — hours of work.

**With Athena:** You write one SQL query. Results in seconds.

```sql
SELECT eventTime, eventName, errorMessage
FROM cloudtrail_logs
WHERE userIdentity.userName = 'john'
  AND errorCode IS NOT NULL
  AND eventTime BETWEEN '2024-03-01' AND '2024-03-15'
ORDER BY eventTime DESC;
```

> **Simple mental model:** Athena is like putting a **SQL engine directly on top of your S3 bucket** — without moving any data anywhere.

---

## What is Amazon Athena?

Amazon Athena is a **serverless, interactive query service** that lets you analyze data stored in Amazon S3 using standard SQL.

### Key Characteristics:

| Characteristic | Detail |
|---|---|
| **Serverless** | No servers, clusters, or infrastructure to manage |
| **Standard SQL** | Uses Presto engine — familiar SQL syntax |
| **Pay per query** | Charged per TB of data scanned |
| **No ETL needed** | Query data directly in S3 as-is |
| **Parallel execution** | Queries run in parallel — results in seconds |
| **Schema-on-read** | Define schema when querying, not when storing |

---

## How Athena Works — The 3-Step Model

```
Step 1: POINT              Step 2: DEFINE             Step 3: QUERY
──────────────             ──────────────             ─────────────
Point Athena at            Define a table             Write SQL and
your S3 bucket             schema describing          run queries
containing data            the data structure         instantly
```

### Step 1 — Your Data in S3
Data can be in any of these formats:
- CSV, JSON (CloudTrail logs, VPC Flow Logs)
- Parquet, ORC (columnar — much faster and cheaper)
- Avro, TSV

### Step 2 — Define the Schema (Table Definition)
You tell Athena what the data looks like:

```sql
CREATE EXTERNAL TABLE cloudtrail_logs (
    eventTime STRING,
    eventName STRING,
    userIdentity STRUCT<userName:STRING, type:STRING>,
    sourceIPAddress STRING,
    errorCode STRING,
    errorMessage STRING
)
ROW FORMAT SERDE 'com.amazon.emr.hive.PrestoNativeJSON'
LOCATION 's3://my-cloudtrail-bucket/AWSLogs/123456789/';
```

> ⚠️ **Key concept:** The data stays in S3 — Athena just reads it. No copying, no loading, no ETL pipeline.

### Step 3 — Run SQL Queries
```sql
-- Find all failed API calls from a specific IP
SELECT eventTime, eventName, errorMessage
FROM cloudtrail_logs
WHERE sourceIPAddress = '203.0.113.42'
  AND errorCode IS NOT NULL
ORDER BY eventTime DESC
LIMIT 100;
```

---

## Athena Pricing — How It Works

> ⚠️ **Exam favourite:** Athena charges per **TB of data scanned** — not per query or per hour.

```
Standard price: $5 per TB scanned

Example:
- You run a query that scans 100 GB of data
- Cost = 0.1 TB × $5 = $0.50

Cost optimization:
- Use columnar formats (Parquet, ORC) → scan 10x LESS data
- Partition your data by date/region → skip irrelevant files
- Compress data → smaller files = less to scan
```

**Cost optimization strategies for the exam:**

| Strategy | How much it saves |
|---|---|
| Convert to Parquet/ORC | Up to 87% less data scanned |
| Partition by date | Skip months of data with WHERE clause |
| Compress data (gzip, Snappy) | 60-80% smaller files |
| Use SELECT specific columns | Don't use `SELECT *` |

---

## AWS Services That Integrate With Athena

This is directly from your slides and heavily tested:

### 1. CloudTrail Logs + Athena
- CloudTrail stores API logs as JSON in S3
- Athena can directly query these logs
- CloudTrail console even has a **"Create Athena table"** button that auto-generates the schema
- Use case: security investigations, compliance audits, finding who did what

```sql
-- Who terminated EC2 instances last week?
SELECT eventTime, userIdentity.userName, requestParameters
FROM cloudtrail_logs
WHERE eventName = 'TerminateInstances'
  AND eventTime > '2024-03-01'
ORDER BY eventTime DESC;
```

### 2. VPC Flow Logs + Athena
- VPC Flow Logs capture all IP traffic going through your VPC
- Stored in S3 or CloudWatch Logs
- Query with Athena to investigate network threats

```sql
-- Find all traffic to a suspicious IP address
SELECT srcaddr, dstaddr, action, protocol
FROM vpc_flow_logs
WHERE dstaddr = '198.51.100.55'
  AND action = 'ACCEPT'
ORDER BY start DESC;
```

### 3. Application Load Balancer (ALB) Logs + Athena
- ALB logs every request (source IP, latency, target, response code)
- Store in S3, query with Athena
- Use case: identify slow endpoints, find attacking IPs, analyze traffic patterns

```sql
-- Find the slowest API endpoints
SELECT request_url, AVG(target_processing_time) as avg_time
FROM alb_logs
GROUP BY request_url
ORDER BY avg_time DESC
LIMIT 10;
```

---

## Athena vs Other Query/Analytics Services

> ⚠️ **SAA exam loves "which service should you use" questions:**

| Service | Best For | Key Difference |
|---|---|---|
| **Athena** | Ad-hoc SQL queries on S3 data | Serverless, pay per query, no setup |
| **Redshift** | Complex analytics on large structured datasets | Provisioned data warehouse, persistent cluster |
| **EMR** | Big data processing (Spark, Hadoop) | For data engineers, complex transformations |
| **QuickSight** | Business intelligence dashboards | Visualization layer, not a query engine |
| **RDS** | Transactional databases | OLTP, not analytics |

**The SAA decision tree:**
```
Need to query data already in S3?
    → Athena (serverless, no ETL, SQL)

Need complex ongoing analytics with consistent performance?
    → Redshift (data warehouse)

Need to process/transform massive datasets?
    → EMR (Spark/Hadoop)

Need dashboards and charts for business users?
    → QuickSight (often sits on top of Athena or Redshift)
```

---

## 🎯 Athena Scenario-Based Questions

**Scenario 1 — Security Investigation at Scale:**
> A company has 3 years of CloudTrail logs in S3 (500 GB total). A security incident occurred and they need to find all API calls made by a specific compromised IAM role in the last 30 days. What's the fastest approach?

**Answer:**
1. Use **Amazon Athena** — create a table pointing to the CloudTrail S3 bucket
2. Run a SQL query filtering by `userIdentity.arn` = compromised role ARN and `eventTime` in last 30 days
3. Results returned in seconds without moving or loading any data
4. Much faster than manually parsing JSON files or writing custom scripts

**Why not CloudTrail Event History?** Only 90 days and limited filtering. Athena gives full flexibility.

---

**Scenario 2 — Cost Optimization:**
> A team runs dozens of Athena queries daily on their 10 TB CloudTrail log dataset. Their monthly Athena bill is very high. What should they do?

**Answer:**
1. **Convert logs to Parquet format** using AWS Glue — columnar format scans only needed columns
2. **Partition data** by year/month/day in S3 — queries with date filters skip irrelevant partitions
3. Result: up to **87% reduction** in data scanned = 87% lower Athena cost

---

**Scenario 3 — Network Threat Investigation:**
> A company's security team detected unusual outbound traffic from their VPC. They need to identify which EC2 instances are communicating with a known malicious IP address `198.51.100.55`. How?

**Answer:**
1. Ensure **VPC Flow Logs** are enabled and stored in S3
2. Use **Athena** to query the flow logs:
```sql
SELECT srcaddr, dstaddr, start, end, action
FROM vpc_flow_logs
WHERE dstaddr = '198.51.100.55'
ORDER BY start DESC;
```
3. The `srcaddr` column reveals which EC2 instances (private IPs) are communicating with the malicious IP

---

**Scenario 4 — The Classic "Which Service" Trap:**
> A startup wants to analyze their application logs stored in S3 to find performance bottlenecks. The analysis will be done once a month by a developer. Which service should they use — Athena or Redshift?

**Answer:** **Athena** — because:
- Ad-hoc, infrequent queries → no need for a persistent cluster
- Data is already in S3 → no loading required
- Pay only for the queries run → much cheaper for occasional use
- Redshift would be overkill and expensive for monthly ad-hoc analysis

---

# ⚙️ TOPIC 6: AWS Config

---

## The Problem AWS Config Solves

Imagine you manage 500 EC2 instances across your company. Your security policy says:
- All EBS volumes must be encrypted
- All S3 buckets must have versioning enabled
- No security groups should allow inbound `0.0.0.0/0` on port 22 (SSH)

**How do you know if someone violated these rules — right now, or 3 months ago?**

CloudTrail tells you WHO changed something.
CloudWatch tells you HOW resources are performing.

**But neither tells you:** *"Is my current configuration compliant with my policies? And what did this configuration look like 6 months ago?"*

**This is exactly what AWS Config answers.**

---

## What is AWS Config?

AWS Config is a service that:
1. **Continuously records** the configuration of every AWS resource
2. **Evaluates** those configurations against your desired rules
3. **Tracks changes** over time with a full configuration history
4. **Alerts you** when something goes out of compliance

> **Simple mental model:** AWS Config is like a **continuous health inspection** of your entire AWS infrastructure — checking every resource against your rulebook, 24/7.

---

## The 4 Core Capabilities

### Capability 1 — Resource Inventory
- Automatically discovers all AWS resources in your account
- Maintains an up-to-date inventory
- Answers: *"What resources do I currently have running?"*

### Capability 2 — Configuration History
- Records every configuration change for every resource
- Creates a **timeline** of changes
- Answers: *"What did this S3 bucket's configuration look like on March 1st?"*

```
S3 Bucket "my-data-bucket" Configuration Timeline:

Jan 1  → Created, versioning OFF, public access OFF
Feb 15 → Versioning turned ON
Mar 3  → Public access turned ON  ← ⚠️ Config flags this!
Mar 3  → Alert sent to security team
Mar 4  → Public access turned OFF ← compliance restored
```

### Capability 3 — Configuration Rules
- You define rules that represent your desired configuration
- Config continuously evaluates resources against these rules
- Non-compliant resources are flagged on a dashboard

### Capability 4 — Compliance Dashboard
- Visual overview of compliant vs non-compliant resources
- Drill down to see exactly which resources are violating which rules
- Generate compliance reports for auditors

---

## AWS Config Rules — The Heart of Config

### What is a Rule?
A rule represents your **desired configuration state**. Config evaluates every resource against every applicable rule and marks it as **COMPLIANT** or **NON_COMPLIANT**.

### Three Types of Rules:

#### Type 1 — AWS Managed Rules (pre-built)
AWS provides 150+ pre-built rules you can enable with one click:

| Rule Name | What it checks |
|---|---|
| `encrypted-volumes` | Are all EBS volumes encrypted? |
| `s3-bucket-versioning-enabled` | Do S3 buckets have versioning on? |
| `restricted-ssh` | Do security groups block SSH (port 22) from 0.0.0.0/0? |
| `iam-user-mfa-enabled` | Do all IAM users have MFA enabled? |
| `ec2-instance-no-public-ip` | Do EC2 instances have public IPs? |
| `rds-storage-encrypted` | Is RDS storage encrypted? |
| `root-account-mfa-enabled` | Does root account have MFA? |
| `approved-amis-by-id` | Are instances launched from approved AMIs only? |
| `required-tags` | Do resources have required tags? |

#### Type 2 — Custom Rules (Lambda-backed)
- Write your own compliance logic using AWS Lambda
- Lambda evaluates the resource and returns COMPLIANT or NON_COMPLIANT
- Use when AWS managed rules don't cover your specific requirement

**Example custom rule:**
> "Every EC2 instance tagged as 'production' must have instance type t3.large or larger"

#### Type 3 — Service Control Policy (SCP) Integration
- Can work alongside AWS Organizations SCPs
- For organization-wide compliance enforcement

### When Rules Are Evaluated:

| Trigger | When it runs |
|---|---|
| **Configuration change** | Every time the resource changes |
| **Periodic** | Hourly, 3-hourly, 6-hourly, 12-hourly, or daily |
| **Both** | On change AND on a schedule |

---

## AWS Config — Key Concepts Deep Dive

### Configuration Item (CI)
- A point-in-time snapshot of a resource's configuration
- Captured every time a resource changes
- Contains: resource type, ARN, relationships, configuration details, tags

### Configuration Snapshot
- A complete snapshot of ALL resource configurations at a point in time
- Can be delivered to S3 on demand or periodically
- Useful for baseline documentation

### Configuration Stream
- A real-time stream of all configuration changes
- Can be sent to SNS for real-time notifications
- Useful for triggering automated remediation

### Conformance Packs
- A collection of Config rules + remediation actions packaged together
- Pre-built packs for compliance frameworks:
  - **CIS AWS Foundations Benchmark**
  - **HIPAA**
  - **PCI-DSS**
  - **NIST 800-53**
- Deploy an entire compliance framework with one click

---

## Automated Remediation — The Most Powerful Feature

> ⚠️ **Heavily tested on SAA:** Config doesn't just DETECT non-compliance — it can automatically FIX it.

**How it works:**
```
Resource becomes NON_COMPLIANT
         ↓
Config Rule flags it
         ↓
Remediation Action triggers
         ↓
AWS Systems Manager Automation Document runs
         ↓
Resource is automatically fixed
```

**Examples of automated remediation:**

| Non-compliance detected | Automatic remediation |
|---|---|
| S3 bucket made public | Automatically set bucket to private |
| EBS volume not encrypted | Automatically encrypt the volume |
| IAM access key not rotated in 90 days | Automatically disable the key |
| Security group allows 0.0.0.0/0 SSH | Automatically remove that inbound rule |
| EC2 instance missing required tags | Automatically apply default tags |

> ⚠️ **Critical distinction:** Config DETECTS and can REMEDIATE, but it does NOT PREVENT changes. If someone makes a non-compliant change, Config catches it after the fact and can fix it — but it cannot stop the change from happening in the first place. To PREVENT → use **IAM policies** or **Service Control Policies (SCPs)**.

---

## AWS Config vs CloudTrail — The Most Tested Comparison

> ⚠️ This comparison appears in almost every SAA exam. Know it cold.

| | AWS CloudTrail | AWS Config |
|---|---|---|
| **Core question** | Who did what to AWS? | What is my resource configured as? |
| **Focus** | API activity (actions) | Resource configuration (state) |
| **Data type** | API call logs | Configuration snapshots |
| **Time dimension** | When did this API call happen? | What did this resource look like over time? |
| **Example** | "John called DeleteBucket at 3 PM" | "This S3 bucket had versioning OFF from Jan 1 to Feb 15" |
| **Compliance use** | API audit trail | Configuration compliance |
| **Can auto-remediate?** | No | Yes (via SSM Automation) |
| **Prevents changes?** | No | No |
| **Real-time alerts** | Via CloudWatch Logs | Via SNS + Config rules |

**Memory trick:**
- CloudTrail = **"Who did WHAT and WHEN"** (the detective)
- AWS Config = **"WHAT DOES IT LOOK LIKE now and over time"** (the inspector)

---

## 🎯 AWS Config Scenario-Based Questions

**Scenario 1 — Compliance Audit:**
> A company's auditor requires proof that all EBS volumes have been encrypted for the past 12 months. How do you provide this?

**Answer:**
1. Enable the **`encrypted-volumes`** AWS Config managed rule
2. Config continuously evaluates all EBS volumes against this rule
3. The **configuration history** shows the compliance state of every volume over time
4. Export the compliance report for the auditor
5. If a volume was ever non-compliant, the history shows exactly when it happened and when it was fixed

---

**Scenario 2 — Automatic Security Response:**
> A company has a strict policy that no S3 bucket should ever be made publicly accessible. Developers sometimes accidentally make buckets public. They want this fixed automatically within minutes. Design a solution.

**Answer:**
```
Developer accidentally makes S3 bucket public
         ↓
AWS Config detects change (configuration trigger)
         ↓
Rule: "s3-bucket-public-read-prohibited" → NON_COMPLIANT
         ↓
Automated Remediation: SSM Automation Document
"AWS-DisableS3BucketPublicReadWrite" runs
         ↓
S3 bucket automatically set back to private
         ↓
SNS notification sent to security team
         (informing them of the incident + auto-fix)
```

Total time from violation to remediation: **under 5 minutes**, fully automated.

---

**Scenario 3 — Resource Inventory:**
> A company is migrating to AWS and needs to know exactly what resources exist across all their AWS accounts and regions. They have 20 accounts in AWS Organizations. What service helps?

**Answer:**
- Enable **AWS Config** with **AWS Organizations** integration
- Use **Aggregator** feature to collect configuration data from all 20 accounts into one view
- Get a complete inventory of all resources: EC2 instances, S3 buckets, RDS databases, IAM roles, VPCs, etc.
- Also see compliance status across all accounts in one dashboard

---

**Scenario 4 — The CloudTrail vs Config Trap:**
> A security team wants to know: (a) which IAM user deleted an S3 bucket yesterday, and (b) whether that S3 bucket had versioning enabled before it was deleted. Which service answers each question?

**Answer:**

| Question | Service | Why |
|---|---|---|
| (a) Who deleted the bucket? | **CloudTrail** | It's an API action — CloudTrail records who called `DeleteBucket` |
| (b) Was versioning enabled? | **AWS Config** | Config maintains configuration history — shows the bucket's settings over time |

> This is the perfect illustration of why **both services are complementary** — they answer different questions about the same event.

---

**Scenario 5 — Preventing Configuration Drift:**
> A company's production environment keeps drifting from its approved baseline — developers make ad-hoc changes that violate security policies. They want to detect AND fix drift automatically. Design a solution.

**Answer:**
```
Define desired state:
├── All EC2 instances must use approved AMIs
├── All EBS volumes must be encrypted
├── No security groups open to 0.0.0.0/0
└── All resources must have cost-center tags

Enable AWS Config with:
├── Rule: approved-amis-by-id
├── Rule: encrypted-volumes
├── Rule: restricted-ssh
└── Rule: required-tags

Each rule has automated remediation:
├── Unapproved AMI → alert + ticket (can't auto-fix running instance)
├── Unencrypted volume → auto-encrypt
├── Open security group → auto-remove offending rule
└── Missing tags → auto-apply default tags

Config Aggregator across all accounts:
└── Unified compliance dashboard for leadership
```

---

**Scenario 6 — Regulatory Compliance Pack:**
> A healthcare company needs to demonstrate HIPAA compliance for their AWS infrastructure. Setting up individual Config rules would take weeks. What's the fastest approach?

**Answer:**
- Deploy the **HIPAA Conformance Pack** in AWS Config
- This pre-built pack contains dozens of relevant Config rules covering HIPAA requirements
- All rules are deployed in minutes
- Compliance dashboard immediately shows current HIPAA compliance posture
- Generate compliance reports on demand for auditors

---

# 🧠 MASTER COMPARISON TABLE — All 6 Topics

This is your ultimate cheat sheet for the SAA exam:

| | CW Monitoring | CW Events | CW Logs | CloudTrail | Athena | AWS Config |
|---|---|---|---|---|---|---|
| **Core question** | How is it performing? | What just happened? | What did it say? | Who did what to AWS? | What does the data say? | What is it configured as? |
| **Data type** | Numeric metrics | JSON events | Raw log text | API call records | Any data in S3 | Config snapshots |
| **Trigger** | Metric threshold | State change/cron | Log pattern | API call made | User runs query | Config change/schedule |
| **Speed** | 1–5 min | Near real-time | Near real-time | ~15 min to S3 | Seconds (on demand) | Minutes |
| **Storage** | 15 months | N/A | Configurable | S3 (Trail) | S3 (source data) | S3 + Config service |
| **Alerting** | Alarms → SNS | Rules → Targets | Metric filters → Alarm | Via CW Logs | N/A | SNS + remediation |
| **Auto-remediate?** | No | Triggers Lambda | No | No | No | Yes (SSM Automation) |
| **Prevents changes?** | No | No | No | No | No | No |
| **Query/Search** | Console/CLI | N/A | Logs Insights | Event History | SQL (Presto) | Console/CLI |
| **Compliance use** | Performance SLAs | Automation | App audit logs | API audit trail | Log investigation | Config compliance |
| **Needs agent?** | For EC2 OS metrics | No | For EC2 logs | No | No | No |
| **Cost model** | Per metric/alarm | Per event | Per GB ingested | Per event recorded | Per TB scanned | Per rule evaluation |

---

# 🔑 MEGA KEY FACTS — All Topics Combined

## CloudWatch Monitoring
- Basic = **5 min free** | Detailed = **1 min paid**
- Memory/disk on EC2 → needs **CloudWatch Agent**
- Alarm states: **OK / ALARM / INSUFFICIENT_DATA**
- Metrics expire after **15 months**
- Billing alarms need **manual enablement** in account settings

## CloudWatch Events (EventBridge)
- **Event → Rule → Target** (3 building blocks)
- One rule → **multiple targets in parallel**
- Two rule types: **event-based** and **schedule-based (cron)**
- CloudWatch Events = **Amazon EventBridge** (same service)

## CloudWatch Logs
- Needs **CloudWatch Agent** on EC2
- **Log Group** = container | **Log Stream** = one source
- Default retention = **never expires** (set a policy!)
- Metric filters apply only to **new events** (no backfill)
- Filters are **case-sensitive**, all terms must match
- **Logs Insights** = ad-hoc | **Metric Filters** = ongoing

## CloudTrail
- Records **every AWS API call** (Console, CLI, SDK)
- Default **Event History = 90 days free**
- **Trail** = long-term storage in S3 (paid)
- Three event types: **Management, Data, Insights**
- Cannot see **inside EC2** (OS-level activity)
- Enable in **all regions** — best practice
- **Log integrity validation** = detect tampering

## Amazon Athena
- **Serverless SQL** on S3 data
- **Pay per TB scanned** — not per query or hour
- Use **Parquet/ORC** to reduce scan cost by up to 87%
- No ETL needed — query data directly in S3
- Best for: **CloudTrail logs, VPC Flow Logs, ALB logs**
- For ongoing analytics → **Redshift** | For big data processing → **EMR**

## AWS Config
- Continuously records **resource configurations**
- Evaluates against **Config Rules** → COMPLIANT or NON_COMPLIANT
- Can **auto-remediate** via SSM Automation
- Does **NOT prevent** changes — only detects and reports
- **Conformance Packs** = pre-built rule sets for HIPAA, PCI-DSS, etc.
- Works with **AWS Organizations** for multi-account compliance
- Use **Aggregator** for cross-account compliance dashboard

---

# 🎯 FINAL EXAM STRATEGY — Classic SAA Question Patterns

## Pattern 1 — "Who did this?" → CloudTrail
Any question asking about identifying which user/role/service performed an action → **CloudTrail**

## Pattern 2 — "Alert me when metric exceeds X" → CloudWatch Alarm
Sustained metric threshold over time periods → **CloudWatch Monitoring + Alarm**

## Pattern 3 — "Automatically do X when Y happens" → CloudWatch Events/EventBridge
Event-driven automation, react to state changes → **CloudWatch Events / EventBridge**

## Pattern 4 — "Detect pattern in application logs" → CloudWatch Logs + Metric Filter
Search log text, count occurrences, alert → **CloudWatch Logs + Metric Filter + Alarm**

## Pattern 5 — "Query log data in S3 with SQL" → Athena
Ad-hoc investigation of logs already stored in S3 → **Athena**

## Pattern 6 — "Ensure resources comply with policies" → AWS Config
Compliance checking, configuration drift, history → **AWS Config**

## Pattern 7 — "Automatically fix non-compliant resources" → AWS Config + SSM
Auto-remediation of policy violations → **AWS Config Remediation**

## Pattern 8 — "Prevent non-compliant configurations" → IAM + SCP
PREVENTION (not detection) → **IAM Policies / Service Control Policies**

---
