# AWS SAA Deep Dive: Module 4 — Scaling & Name Resolution

---

## 🔷 PART 1: ELASTIC LOAD BALANCING (ELB)

### What is ELB?

ELB is a **managed service** that automatically distributes incoming traffic across multiple targets (EC2, containers, Lambda, IP addresses) in one or more Availability Zones. AWS manages the underlying infrastructure — you just configure it.

### The Three Load Balancer Types

---

### 1. Application Load Balancer (ALB) — Layer 7

ALB operates at the **HTTP/HTTPS application layer**. It can inspect the content of requests and make intelligent routing decisions.

**Key capabilities:**
- **Content-based routing** — route based on URL path (`/orders` → Orders service, `/images` → Images service), hostname, HTTP headers, query strings, source IP
- **Supports WebSocket and HTTP/2**
- **Host-based routing** — `api.example.com` goes to one target group, `app.example.com` to another
- **Lambda as a target** — ALB can forward requests to Lambda functions
- **User authentication** — integrates with Cognito or OIDC providers
- **Sticky sessions** using cookies (duration-based or application-based)
- **Fixed response actions** — return a fixed HTTP response without hitting backend
- **Redirect actions** — HTTP → HTTPS redirect at the load balancer level
- **WAF integration** — attach AWS WAF to ALB for web application firewall protection

**ALB components:**
- **Load Balancer** — the entry point with a DNS name
- **Listener** — checks for connection requests on a port/protocol; up to **50 listeners** per ALB
- **Rules** — each listener has rules with conditions and actions; evaluated in priority order; last rule is default
- **Target Group** — a logical grouping of targets (EC2, ECS, Lambda, IPs); each target group has its own **health check configuration**
- A single target can be registered in **multiple target groups**

**Scenario:** Your e-commerce app has separate microservices. You want `/cart` going to Cart service and `/payment` going to Payment service — both behind the same ALB. You use **path-based routing rules** on a single listener.

**Exam Trap ⚠️:** ALB does NOT support static IP addresses natively. If you need a static IP with ALB, put **AWS Global Accelerator** in front of it. NLB natively supports static/Elastic IPs.

---

### 2. Network Load Balancer (NLB) — Layer 4

NLB operates at the **TCP/UDP/TLS transport layer**. It makes routing decisions based on IP and port — it cannot inspect HTTP headers.

**Key capabilities:**
- **Extreme performance** — handles millions of requests per second with ultra-low latency
- **Static IP per AZ** — you can assign one Elastic IP per AZ (crucial for firewall whitelisting)
- **Preserves source IP** — backend sees the actual client IP (ALB by default replaces with its own IP; you need X-Forwarded-For header)
- **TLS termination** — can offload TLS from backends
- **UDP support** — useful for DNS, gaming, IoT
- **No security group on NLB** — NLB itself doesn't have a security group; security groups on targets must allow traffic from client IPs (or 0.0.0.0/0 if varied)
- **Cross-zone load balancing** — disabled by default on NLB (enabled by default on ALB)

**Scenario:** You run a financial trading platform requiring sub-millisecond latency and your firewall team needs to whitelist specific IPs. Use NLB with Elastic IPs.

**Exam Trap ⚠️:** NLB does NOT support path-based or host-based routing. If the question mentions HTTP routing by path/header, the answer is ALB.

---

### 3. Classic Load Balancer (CLB) — Layers 4 & 7

**Previous generation** — operates at both Layer 4 and Layer 7 but with limited Layer 7 features compared to ALB.

- Used for EC2-Classic network (retired in 2022)
- Supports HTTP, HTTPS, TCP, SSL
- No path-based routing, no host-based routing, no Lambda targets
- **Exam Trap ⚠️:** CLB is the answer only for legacy EC2-Classic scenarios or when the question explicitly mentions it. AWS recommends migrating to ALB or NLB.

---

### ELB Features — Deep Dive

**High Availability:** ELB automatically distributes across multiple AZs. Always configure ELB across **at least 2 AZs** for HA.

**Health Checks:**
- ELB sends periodic requests to targets to assess health
- Unhealthy targets are removed from rotation
- Health check configuration: protocol, path (for HTTP), port, healthy/unhealthy threshold, timeout, interval
- **Exam Trap ⚠️:** If instances fail health checks, ELB stops sending traffic but does NOT terminate them — only Auto Scaling terminates instances.

**Cross-Zone Load Balancing:**
- **ALB:** Enabled by default (no charge for cross-AZ data transfer)
- **NLB & GLB:** Disabled by default (charges apply when enabled)
- Without cross-zone: each load balancer node distributes only to registered targets in its AZ
- With cross-zone: each node distributes evenly across all registered targets in all AZs

**Connection Draining / Deregistration Delay:**
- When a target is deregistered or unhealthy, ELB stops sending new requests
- Existing in-flight requests are given time to complete (default 300 seconds, range 1–3600)
- Setting to 0 disables it (immediate termination of connections)

**TLS/SSL Termination:**
- ELB can decrypt HTTPS traffic and forward as HTTP to backends (offloads CPU from instances)
- Uses **ACM (AWS Certificate Manager)** certificates
- **Exam Trap ⚠️:** For end-to-end encryption (client → ELB → backend), configure HTTPS on both listener and target group

**Access Logs:**
- ALB and NLB can log request details to S3
- Disabled by default; must be enabled manually

**Sticky Sessions (Session Affinity):**
- ALB: Uses cookies — either **duration-based** (LB-generated) or **application-based** (app sets the cookie)
- NLB: Uses source IP
- **Exam Trap ⚠️:** Sticky sessions can cause uneven load distribution; not recommended for stateless apps

---

### Gateway Load Balancer (GLB) — Bonus

Not in the slides but important for SAA:
- Layer 3 (network layer)
- Used for deploying, scaling, and managing **third-party virtual network appliances** (firewalls, IDS/IPS, deep packet inspection)
- Uses **GENEVE protocol on port 6081**
- Creates a **bump-in-the-wire** transparent architecture

---

## 🔷 PART 2: AMAZON EC2 AUTO SCALING

### Why Auto Scaling?

Traditional on-premises: buy hardware for peak demand → hardware sits idle 70% of the time, then is overwhelmed during peaks. Auto Scaling solves this by elastically adjusting capacity.

**Scale out** = add instances (horizontal scaling)
**Scale up** = larger instance type (vertical scaling — requires downtime)

Auto Scaling does **scale out/in**, not scale up/down.

---

### The Three Components of EC2 Auto Scaling

---

### Component 1: Launch Configuration / Launch Template

This defines **what to launch** — the blueprint for new instances.

**Launch Configuration (older, being deprecated):**
- Immutable — cannot be modified after creation
- Only one active at a time per Auto Scaling group
- Specify: AMI, instance type, security group, key pair, storage, IAM role, user data, spot price

**Launch Template (recommended, newer):**
- **Versioned** — can have multiple versions; ASG can use specific version or `$Latest` or `$Default`
- Supports **multiple instance types** and **mixed instances policy** (combine On-Demand + Spot)
- Supports **T2/T3 unlimited** CPU credit specification
- More features: placement groups, capacity reservations, dedicated hosts, elastic inference, tags
- Can inherit from another template (base + overrides)

**Exam Trap ⚠️:** Launch Configurations CANNOT be edited. To update, you create a new one and update the ASG. Existing instances are NOT automatically replaced — you must manually terminate them or use instance refresh. Launch Templates are preferred for new deployments.

---

### Component 2: Auto Scaling Group (ASG)

The ASG is the **logical group** that manages the EC2 instances.

**Key settings:**
- **Minimum capacity** — ASG will never go below this (floor)
- **Maximum capacity** — ASG will never go above this (ceiling)
- **Desired capacity** — the number of instances ASG tries to maintain; if not set, defaults to minimum
- **Subnets/AZs** — specify which AZs/subnets to launch in; ASG distributes evenly across AZs

**ASG and ELB integration:**
- ASG can register new instances automatically with a target group
- ASG uses ELB health checks (not just EC2 health checks) when integrated with ELB
- **Exam Trap ⚠️:** By default, ASG uses **EC2 health checks only**. You must explicitly configure it to also use **ELB health checks**. Without ELB health check enabled in ASG, an instance that the ELB marks unhealthy (failed HTTP health check) will NOT be replaced by ASG.

**Instance Refresh:**
- Gradually replace running instances with new ones (e.g., after changing launch template)
- Specify minimum healthy percentage during refresh
- Useful for rolling updates

**Warm Pools:**
- Pre-initialize instances so they're ready to serve traffic quickly
- Reduces scale-out latency

---

### Component 3: Scaling Policies

**1. Manual Scaling**
- Manually set desired capacity
- `aws autoscaling set-desired-capacity --auto-scaling-group-name my-asg --desired-capacity 5`

**2. Scheduled Scaling**
- Scale based on known, predictable patterns
- Example: Every Monday at 8 AM set min=10, every Friday at 8 PM set min=2
- Useful for batch jobs, business hours patterns
- **Exam Trap ⚠️:** Scheduled scaling doesn't respond to actual load — if traffic is unexpectedly high, scheduled scaling won't add more instances. Combine with dynamic scaling.

**3. Dynamic Scaling (reactive)**

*Simple Scaling:*
- Single CloudWatch alarm triggers a single scaling action
- After scaling, waits for **cooldown period** before next action (default 300 seconds)
- During cooldown, no additional scaling activities happen
- Old and less flexible; generally replaced by step/target tracking

*Step Scaling:*
- Multiple alarms or one alarm with multiple steps
- Responds proportionally to alarm breach magnitude
- Uses **instance warmup period** instead of cooldown
- During warmup, newly launched instances are not counted toward ASG metrics
- Does NOT wait for warmup to complete before potentially scaling again
- More responsive than simple scaling

*Target Tracking Scaling:*
- Most recommended for most use cases
- You specify a **target value** for a metric (e.g., keep average CPU at 50%)
- ASG automatically creates and manages CloudWatch alarms
- Scales out/in automatically to maintain the target
- Built-in **scale-in cooldown** (default 300s) and **scale-out cooldown** (default 0s — scales out immediately)
- **Exam Trap ⚠️:** Target tracking cannot scale to zero. If you need to completely scale down, use scheduled scaling or manual override.

**4. Predictive Scaling**
- Uses ML to analyze historical load data (last 14 days)
- Proactively scales BEFORE anticipated load increase
- Works with target tracking
- Modes: **Forecast only** (view predictions, no action) or **Forecast and scale** (auto scale)
- Best for applications with **recurring, cyclical patterns**

---

### Termination Policies (Scale-In Order)

When scaling in, ASG uses termination policies to decide which instance to remove:

| Policy | Description |
|---|---|
| **Default** | Complex multi-step algorithm (see below) |
| OldestInstance | Terminates longest-running instance |
| NewestInstance | Terminates shortest-running instance |
| OldestLaunchConfiguration | Terminates instance using oldest launch config |
| OldestLaunchTemplate | Terminates instance using oldest launch template |
| ClosestToNextInstanceHour | Terminates instance closest to billing hour (cost optimization) |
| AllocationStrategy | For mixed instance policies (Spot optimization) |

**Default termination policy algorithm:**
1. Select AZ with most instances (balance AZs)
2. Among those, select instance with oldest launch config/template
3. If tied, select instance closest to next billing hour
4. If still tied, select randomly

**Exam Trap ⚠️:** The default policy is NOT simply "oldest instance." It's a multi-step process that first balances AZs.

---

### Avoiding Thrashing

**Thrashing** = rapidly and repeatedly scaling out then in, wasting money and causing instability.

**Three mechanisms to prevent thrashing:**

1. **Alarm sustain period** — CloudWatch alarm only triggers after metric breaches threshold for a sustained period (e.g., CPU > 80% for **10 consecutive minutes**, not just a spike)

2. **Cooldown period** (Simple scaling only):
   - After a scaling activity, suspend further scaling for cooldown duration
   - Default: 300 seconds
   - ASG rejects scaling requests during cooldown
   - **Exam Trap ⚠️:** Cooldown applies to the entire ASG for simple scaling. For step scaling, use warmup period instead.

3. **Instance warmup period** (Step scaling):
   - New instance is excluded from ASG metrics until warmup completes
   - Prevents the newly added instance from immediately triggering scale-in because adding it "diluted" the average metric
   - **Exam Trap ⚠️:** With step scaling, if CPU stays high during warmup, ASG CAN add more instances (unlike cooldown which blocks all activity). This is a key difference from simple scaling cooldown.

---

### Lifecycle Hooks

Lifecycle hooks allow you to perform **custom actions** during scale-out or scale-in events, pausing instance state transitions.

**Scale-Out lifecycle:**
1. Instance launches (Pending state)
2. Lifecycle hook fires → instance enters **Pending:Wait**
3. You perform actions: install software, configure, run tests
4. You send `complete-lifecycle-action` (continue or abandon)
5. Instance enters **InService**

**Scale-In lifecycle:**
1. Instance marked for termination (Terminating state)
2. Lifecycle hook fires → instance enters **Terminating:Wait**
3. You perform actions: download logs, graceful shutdown, deregister from service discovery
4. Complete the action
5. Instance **Terminated**

**Exam Trap ⚠️:** If your lifecycle action doesn't complete within the timeout (default 3600 seconds, max 7200), ASG takes the default action (CONTINUE for scale-out, ABANDON for scale-in). Use SNS/SQS/EventBridge + Lambda to automate lifecycle hook actions.

---

### Steady-State Group

Setting min = max = desired = 1 creates a **single-instance HA pattern**:
- If the instance fails health check, ASG terminates and launches a replacement
- Common use case: NAT instance (though NAT Gateway is preferred now), bastion host
- **Limitation:** There is brief downtime during replacement
- For zero-downtime, use multiple instances (min ≥ 2)

---

### Instance Health

**EC2 status checks (default):**
- System status (AWS infrastructure)
- Instance status (OS/software)

**ELB health checks (optional, recommended when behind ELB):**
- Checks application-layer health (can the app respond to HTTP requests?)
- Must be explicitly enabled on ASG when using ELB

**Manual health change:**
```bash
aws autoscaling set-instance-health \
  --instance-id i-1234567890abcdef0 \
  --health-status Unhealthy
```

This forces ASG to replace the instance on next health check evaluation.

---

## 🔷 PART 3: AMAZON ROUTE 53

### What is Route 53?

Route 53 is AWS's **highly available and scalable DNS web service**. It performs three main functions:

1. **Domain Registration** — register/transfer domain names (e.g., example.com)
2. **DNS Resolution** — translate domain names to IP addresses
3. **Health Checking** — monitor health of endpoints and route traffic accordingly

The name "Route 53" refers to TCP/UDP port 53 (DNS port).

---

### Key Concepts

**Hosted Zone:**
- Container for DNS records for a domain
- **Public Hosted Zone:** Routes internet traffic (publicly accessible)
- **Private Hosted Zone:** Routes traffic within a VPC (internal DNS resolution)
- Cost: $0.50/month per hosted zone

**Record Types:**

| Record | Purpose | Example |
|---|---|---|
| **A** | Domain → IPv4 address | example.com → 93.184.216.34 |
| **AAAA** | Domain → IPv6 address | example.com → 2606:2800:... |
| **CNAME** | Domain → another domain | www.example.com → example.com |
| **MX** | Mail server records | Priority + mail server |
| **NS** | Name server records | Identifies authoritative name servers |
| **SOA** | Start of Authority | Zone metadata |
| **TXT** | Text records | SPF, DKIM, domain verification |
| **PTR** | Reverse DNS | IP → domain name |
| **SRV** | Service location | Host, port for services |
| **CAA** | Certificate Authority Authorization | Which CAs can issue certs |

**ALIAS Record (AWS-specific):**
- Routes traffic to AWS resources (ELB, CloudFront, S3, API Gateway, another Route 53 record)
- Works like CNAME but CAN be used at the **zone apex** (root domain like example.com)
- **No additional charge** for queries to AWS resources
- **Exam Trap ⚠️:** CNAME CANNOT be used at the zone apex (root domain). If the question asks how to point `example.com` (not `www.example.com`) to an ELB, the answer is **ALIAS record**, not CNAME.
- ALIAS automatically follows IP changes of the target resource

**TTL (Time To Live):**
- How long DNS resolvers cache the record
- Higher TTL = fewer queries to Route 53 (cheaper) but slower propagation of changes
- Lower TTL = more queries (costly) but faster changes
- **Exam Trap ⚠️:** Before changing DNS records, lower TTL first (allow old TTL to expire), make change, then raise TTL back. This minimizes propagation issues.

---

### The 7 Routing Policies — Deep Dive

---

**1. Simple Routing Policy**
- Single record → one or more IP values
- If multiple IPs, Route 53 returns all values in random order; client picks one
- **No health checks** (health checks not supported with simple routing for single records)
- Use case: Single resource serving all traffic, no failover needed
- **Exam Trap ⚠️:** Simple routing can return multiple values but cannot do weighted distribution or health-check-based failover. If you need health checks, use another policy.

---

**2. Weighted Routing Policy**
- Distribute traffic across multiple resources based on weights
- Weight value: 0 to 255
- Traffic % = (individual weight) / (sum of all weights)
- Setting a record's weight to 0 stops traffic to that resource
- If ALL records have weight 0, Route 53 returns all records equally

**Use cases:**
- **Blue/Green deployment** — gradually shift traffic from old (blue) system to new (green) system: start 90/10, then 70/30, then 0/100
- **A/B testing** — test new version with small percentage of users
- **Load distribution** across regions

**Exam Trap ⚠️:** Weighted routing with health checks — if a weighted record fails its health check, Route 53 removes it from responses and distributes traffic among healthy records based on their relative weights.

---

**3. Latency-Based Routing (LBR)**
- Routes traffic to the AWS Region with the **lowest network latency** for the user
- NOT geographic distance — it's actual measured network latency
- Route 53 maintains latency data between user locations and AWS regions
- **Exam Trap ⚠️:** A user in India might be routed to US East if that region has lower latency than the closer Singapore region (though unlikely, it's possible). Latency ≠ proximity.
- Each record in a latency policy is associated with a specific AWS Region
- Can combine with health checks for failover

---

**4. Failover Routing Policy**
- **Active-Passive failover**
- One record designated as **Primary**, another as **Secondary** (failover)
- Route 53 monitors primary with health check
- If primary is unhealthy → traffic automatically routed to secondary
- Secondary can be another EC2, an S3 static website, another region
- Requires **health check on the primary record**

**Scenario:** Your production ELB is primary. An S3 static website with a "maintenance" page is secondary. If the ELB fails its health check, users see the maintenance page automatically.

**Exam Trap ⚠️:** Failover routing requires health checks. Without health checks, Route 53 doesn't know the primary is down and won't failover. This is the most common exam trap for this policy.

---

**5. Geolocation Routing Policy**
- Routes traffic based on **where the DNS query originates** (user's location)
- Location specificity: continent → country → US state
- More specific locations take precedence
- Must create a **default record** for users who don't match any location rule
- Use cases: Content localization, legal/regulatory compliance, language-specific content

**Example:** Users from Germany → German-language server; users from US → English server; all others → default server.

**Exam Trap ⚠️:** If there's no matching geolocation record AND no default record → Route 53 returns a "no answer" response. Always create a default record. Also, geolocation is based on DNS query origin, NOT the actual user's IP location (they can differ if user uses a remote DNS resolver like 8.8.8.8).

---

**6. Geoproximity Routing Policy**
- Routes traffic based on **geographic location of users AND resources**
- Uses **bias** to expand or shrink the geographic area served by a resource
- Positive bias (1–99): Expand area → resource serves more users
- Negative bias (-1 to -99): Shrink area → resource serves fewer users
- Requires **Traffic Flow** (Route 53 visual editor)
- Can route to AWS resources (specify region) or non-AWS resources (specify lat/long)

**Difference from Geolocation:**
- Geolocation: Route based on user's exact country/continent
- Geoproximity: Route based on physical distance, with bias adjustments

**Exam Trap ⚠️:** Geoproximity requires Route 53 Traffic Flow (additional cost). Questions about shifting traffic between two regions with a visual percentage control → Geoproximity with bias.

---

**7. Multivalue Answer Routing Policy**
- Returns multiple IP values for a single DNS name (up to 8 healthy records)
- **Supports health checks** (unlike simple routing with multiple values)
- Route 53 only returns healthy records
- NOT a substitute for a load balancer — clients still pick randomly from returned values
- Use case: Simple client-side load balancing with health checking

**Exam Trap ⚠️:** Multivalue answer is often confused with simple routing (multiple values). The key difference: multivalue supports health checks and only returns healthy records. Simple routing returns all values regardless of health.

---

### Route 53 Health Checks

**Types:**
1. **Endpoint health checks** — monitor HTTP/HTTPS/TCP endpoint (IP or domain)
2. **Calculated health checks** — combine results of other health checks using AND/OR/NOT logic
3. **CloudWatch alarm health checks** — based on state of a CloudWatch alarm

**Health checker locations:** Route 53 uses health checkers from multiple locations globally. For a resource to be "healthy," a threshold of checkers must report it healthy (default 18%).

**Exam Trap ⚠️:** Route 53 health checkers originate from the **public internet**. If your instances are in a private subnet (no public IP), health checkers can't reach them directly. Solution: Use CloudWatch alarm-based health checks, or put a public-facing proxy/ELB in front.

**Health check intervals:**
- Standard: 30 seconds
- Fast: 10 seconds (higher cost)

---

### Associating DNS with ELB

**Default ELB DNS:** `my-lb-123456789.us-east-1.elb.amazonaws.com` (changes when ELB is recreated)

**Custom domain options:**
1. **ALIAS record** (recommended): `example.com` ALIAS → ELB DNS name (free, works at zone apex)
2. **CNAME record**: `www.example.com` CNAME → ELB DNS name (NOT at zone apex, charged per query)

**Exam Trap ⚠️:** Never point an A record directly to an ELB IP address. ELB IPs change. Use ALIAS or CNAME to the ELB DNS name.

---

### Route 53 + ELB Architecture Pattern

```
User → Route 53 (DNS resolution) → ELB → Auto Scaling Group (EC2 instances)
```

This is the standard three-tier pattern tested heavily on the SAA exam.

---

## 🔷 PART 4: CRITICAL ARCHITECTURE PATTERNS & EXAM SCENARIOS

### Pattern 1: Multi-Region Active-Active with Latency Routing

```
Users → Route 53 (Latency) → ELB in us-east-1 → ASG
                           → ELB in ap-southeast-1 → ASG
```
- Route 53 routes each user to lowest-latency region
- Both regions active simultaneously
- Add health checks to detect regional failures and failover

### Pattern 2: Multi-Region Active-Passive with Failover Routing

```
Users → Route 53 (Failover) → Primary ELB in us-east-1 (health check)
                            → Secondary ELB in us-west-2
```
- Primary serves all traffic
- If primary health check fails, Route 53 automatically routes to secondary
- Secondary can be warm standby or pilot light

### Pattern 3: Blue/Green Deployment

```
Users → Route 53 (Weighted) → Old ELB (weight 90)
                            → New ELB (weight 10)
```
- Gradually shift weight from old to new
- If issues detected, set new weight to 0 (instant rollback)

### Pattern 4: Canary Testing

Same as blue/green but with very low weight on new version (e.g., 5%) to test with real traffic before full rollout.

---

## 🔷 ADDITIONAL SAA EXAM TOPICS FROM THIS MODULE

### ELB + SSL/TLS Deep Dive

**SSL offloading:** HTTPS → ELB (decrypts) → HTTP → backend (reduces backend CPU)

**SSL passthrough:** HTTPS → ELB → HTTPS → backend (end-to-end encryption; ELB doesn't decrypt; requires TCP listener on NLB)

**Server Name Indication (SNI):**
- Allows multiple SSL certificates on a single ALB listener
- Client indicates which hostname it's connecting to in the SSL handshake
- ALB uses correct certificate based on hostname
- **Supported by ALB and NLB, NOT CLB**
- **Exam Trap ⚠️:** CLB supports only one SSL certificate per load balancer. If you need multiple SSL certificates on one load balancer, use ALB with SNI.

### ASG + Spot Instances

**Mixed instances policy:**
- Combine On-Demand and Spot instances in the same ASG
- Specify On-Demand base capacity + percentage
- Use multiple Spot instance types/sizes for capacity optimization
- **Spot interruption:** ASG automatically replaces interrupted Spot instances

**Spot instance termination notice:**
- 2-minute warning before Spot termination
- Use lifecycle hooks to gracefully handle termination
- **Exam Trap ⚠️:** For production workloads, always mix On-Demand base with Spot for cost savings without sacrificing availability. Never run production entirely on Spot.

### ASG Scaling Limits

- **Minimum group size:** 0
- **Maximum group size:** 5000 (default soft limit, can be raised)
- Can scale to 0 (useful for dev environments during off-hours)

### ELB Security

**Security Groups on ALB:**
- ALB has its own security group
- EC2 instances' security groups should allow traffic from ALB's security group (not 0.0.0.0/0)
- Best practice: EC2 SG inbound rule = source is ALB SG (not CIDR)

**NLB and Security Groups:**
- NLB itself does NOT have a security group
- Backend instance SGs must allow traffic from client IPs OR from NLB's private IPs
- Enable "client IP preservation" to see actual client IPs at backend

---

## 🔷 20 EXAM-STYLE MCQs

---

**Question 1** *(Scenario + Exam Trap)*

A company is running a web application behind an ALB. The security team requires that the application servers never be directly accessible from the internet and that traffic only flows through the ALB. What is the MOST secure configuration?

A. Configure EC2 security groups to allow inbound traffic only from the ALB's IP addresses
B. Configure EC2 security groups to allow inbound traffic only from the ALB's security group ID
C. Place EC2 instances in a public subnet and configure NACLs to block direct internet access
D. Use an NLB instead of ALB since NLB provides better security isolation

**Answer: B**

**Explanation:** Referencing the ALB's security group ID in the EC2 security group inbound rule is the best practice. ALB IPs change dynamically, so using IP addresses (A) will break. NACLs (C) are stateless and harder to manage precisely. NLB doesn't have a security group to reference (D).

---

**Question 2** *(Multiple Correct)*

A company needs to ensure their Auto Scaling group replaces unhealthy instances. Which of the following health check sources can be configured for an ASG? (Select TWO)

A. AWS CloudTrail health checks
B. Amazon EC2 instance status checks
C. AWS Config compliance checks
D. Elastic Load Balancer health checks
E. AWS Systems Manager health checks

**Answer: B, D**

**Explanation:** ASG natively supports EC2 health checks (default) and ELB health checks (optional, must be enabled). CloudTrail is audit logging. Config checks compliance. Systems Manager doesn't feed health status to ASG.

---

**Question 3** *(Scenario + Exam Trap)*

A solutions architect is setting up a new Auto Scaling group with a step scaling policy. An instance takes 4 minutes to fully initialize. During peak load, the system is adding one instance every 2 minutes but the average CPU keeps rising. What should be configured to prevent this from triggering additional scale-out actions before instances are ready?

A. Set the cooldown period to 240 seconds
B. Set the instance warmup period to 240 seconds
C. Set the alarm evaluation period to 240 seconds
D. Set the health check grace period to 240 seconds

**Answer: B**

**Explanation:** For step scaling, use the **instance warmup period** (not cooldown). During warmup, the new instance's metrics are not counted in the ASG aggregate. Cooldown is for simple scaling. Health check grace period prevents health checks from failing before instance initializes but doesn't prevent additional scaling.

**Exam Trap ⚠️:** Cooldown is for simple scaling; warmup is for step and target tracking scaling.

---

**Question 4** *(Scenario)*

A company runs a website that stores user session data in EC2 instance memory. They want to add an ALB to distribute traffic and add more instances during peak times. Users are complaining that they keep getting logged out when requests hit different instances. What is the BEST solution?

A. Enable cross-zone load balancing
B. Enable sticky sessions on the ALB target group
C. Migrate session data to an ElastiCache cluster
D. Use a Network Load Balancer instead

**Answer: C**

**Explanation:** While sticky sessions (B) would fix the immediate symptom, storing session data in ElastiCache is the architecturally correct (and exam-preferred) solution. It makes the application truly stateless, allowing any instance to serve any request. Sticky sessions create uneven load distribution and break when instances are replaced. This is a key cloud best practice: **externalize session state**.

---

**Question 5** *(Exam Trap)*

A company has a domain `example.com` hosted in Route 53. They want to point the root domain (`example.com`) to their Application Load Balancer. What type of DNS record should they create?

A. A record pointing to the ALB's IP address
B. CNAME record pointing to the ALB DNS name
C. ALIAS record pointing to the ALB DNS name
D. AAAA record pointing to the ALB IPv6 address

**Answer: C**

**Explanation:** CNAME cannot be used at the zone apex (root domain) — this is a fundamental DNS limitation. A records shouldn't point to ALB IPs (they change). ALIAS records work at the zone apex and automatically follow DNS changes of the ALB. This is a very common SAA exam question.

---

**Question 6** *(Multiple Correct)*

A global company wants to optimize their Route 53 routing strategy. They need different requirements for different regions. Which Route 53 routing policies support health checks? (Select THREE)

A. Simple routing
B. Weighted routing
C. Latency-based routing
D. Failover routing
E. Multivalue answer routing

**Answer: B, C, D, E** (Actually B, C, D, E all support health checks; only Simple routing has limitations)

**Corrected Answer: B, C, D** for strict "fully support health checks for routing decisions"

**Explanation:** Weighted, Latency, Failover, Geolocation, Geoproximity, and Multivalue all support health checks. Simple routing does NOT support health checks in the traditional sense (cannot remove unhealthy endpoints from rotation). For this question select B, C, D.

---

**Question 7** *(Scenario)*

An application experiences traffic spikes every weekday at 9 AM when employees start work, and the team knows this pattern is consistent. What is the MOST cost-effective scaling strategy?

A. Target tracking scaling with CPU utilization target of 60%
B. Simple scaling triggered by CPU > 70% CloudWatch alarm
C. Scheduled scaling to increase capacity at 8:45 AM on weekdays
D. Predictive scaling analyzing historical patterns

**Answer: C**

**Explanation:** Since the pattern is **known and predictable** (every weekday at 9 AM), **scheduled scaling** is the most appropriate. It scales out proactively before the load hits. Predictive scaling (D) also works but has overhead of 14-day analysis and may not be as precise. Target tracking and simple scaling are reactive — they scale after CPU already rises. **Exam tip:** "known, predictable pattern" → scheduled or predictive scaling.

---

**Question 8** *(Exam Trap)*

A company uses a Classic Load Balancer. They need to support multiple SSL/TLS certificates on a single load balancer to serve different domains. What should they do?

A. Configure SNI on the Classic Load Balancer
B. Upload multiple certificates to the Classic Load Balancer
C. Migrate to an Application Load Balancer and configure SNI
D. Use multiple Classic Load Balancers, one per domain

**Answer: C**

**Explanation:** CLB does NOT support SNI and can only have ONE SSL certificate. ALB and NLB support SNI allowing multiple certificates. The correct solution is migrating to ALB. D works but is not cost-effective or architecturally sound.

---

**Question 9** *(Multiple Correct)*

A solutions architect is designing a resilient, multi-region architecture. Which combination of Route 53 policies would allow traffic to be routed to the lowest-latency healthy region, with automatic failover to another region if that region becomes unhealthy? (Select TWO)

A. Simple routing
B. Latency-based routing
C. Weighted routing
D. Health checks on latency records
E. Geolocation routing

**Answer: B, D**

**Explanation:** Latency-based routing routes to the lowest latency region. Adding health checks to each latency record means if that region's health check fails, Route 53 removes it from responses and routes to the next-best latency option. This combination provides both performance optimization and automatic failover.

---

**Question 10** *(Scenario)*

An e-commerce application is launching a new checkout service. The team wants to route only 5% of production traffic to the new service while the remaining 95% goes to the old service. If the new service has problems, they want instant rollback. What Route 53 configuration should they use?

A. Failover routing with the new service as secondary
B. Latency-based routing between old and new services
C. Weighted routing with weights 95 and 5, with health checks
D. Geolocation routing with specific countries going to the new service

**Answer: C**

**Explanation:** Weighted routing with 95/5 split enables canary/blue-green testing. Health checks allow Route 53 to automatically remove unhealthy records. Instant rollback is achieved by changing the new service's weight to 0. This is the canonical blue/green/canary pattern in Route 53.

---

**Question 11** *(Exam Trap)*

A solutions architect is configuring an Auto Scaling group behind an Application Load Balancer. Instances are passing EC2 health checks but the application is returning 502 errors. The ALB marks these instances as unhealthy. Will the Auto Scaling group replace these instances?

A. Yes, because ELB health checks automatically integrate with ASG
B. No, because ASG only uses EC2 health checks by default and the instances pass those
C. Yes, because the ALB will notify ASG to terminate unhealthy instances
D. No, because health check failures require manual intervention

**Answer: B**

**Explanation:** This is a critical exam trap. By default, ASG only uses **EC2 health checks** (system/instance status). Even if ELB marks an instance unhealthy, ASG will NOT replace it unless you explicitly configure the ASG to use ELB health checks. You must enable this in the ASG configuration: Health check type = ELB.

---

**Question 12** *(Scenario)*

A company has a web application behind an NLB. The backend instances need to see the real client IP addresses for logging and geo-restriction. The instances are currently seeing the NLB's private IP. What should be done?

A. Enable "Client IP Preservation" on the NLB target group
B. Use X-Forwarded-For header (NLB automatically adds it)
C. Switch to an ALB which automatically preserves client IPs
D. Configure Proxy Protocol on the NLB

**Answer: A**

**Explanation:** NLB supports **Client IP Preservation** which, when enabled, passes the client IP directly to the target. Alternatively, Proxy Protocol (D) also works but requires application support. ALB uses X-Forwarded-For header (not native IP). X-Forwarded-For is an ALB feature, not NLB.

---

**Question 13** *(Multiple Correct)*

A company wants to implement a disaster recovery solution. The primary application runs in us-east-1. Which Route 53 configuration options would provide automatic failover to us-west-2? (Select TWO)

A. Weighted routing with health checks (100/0 weights normally, switch if failure)
B. Failover routing policy with health checks on primary record
C. Simple routing returning both regional IPs
D. Latency routing with health checks enabled on both regional records
E. Geolocation routing directing all US traffic to us-east-1

**Answer: B, D**

**Explanation:** Failover routing (B) is explicitly designed for active-passive DR — primary/secondary designation with health checks. Latency routing (D) with health checks also provides automatic failover: if us-east-1 fails its health check, Route 53 removes it and routes all traffic to us-west-2. Weighted routing (A) with 100/0 won't fail over automatically — the 0-weight record won't receive traffic even if primary fails.

---

**Question 14** *(Exam Trap)*

An Auto Scaling group has a maximum capacity of 10 instances. A step scaling policy is triggered to add 5 instances when capacity is at 9 instances. What happens?

A. ASG adds 5 instances bringing total to 14
B. ASG adds 1 instance to reach the maximum of 10
C. ASG rejects the scaling action since it would exceed maximum
D. ASG adds 5 instances and temporarily overrides the maximum

**Answer: B**

**Explanation:** ASG will NEVER exceed the maximum capacity. When scaling would exceed maximum, ASG scales to the maximum (adds only enough instances to reach 10, not 14). The maximum is a hard ceiling that cannot be breached by scaling policies.

---

**Question 15** *(Scenario)*

A company is processing financial transactions. Instances in their Auto Scaling group are being terminated during scale-in, but in-flight transactions are being lost. What is the BEST solution?

A. Increase the cooldown period to 3600 seconds
B. Implement ASG lifecycle hooks on scale-in events to complete transactions before termination
C. Disable scale-in in the Auto Scaling policy
D. Use connection draining on the ALB with a 3600-second timeout

**Answer: B**

**Explanation:** **Lifecycle hooks** on scale-in events pause the termination in **Terminating:Wait** state, giving the instance time to complete in-flight transactions, drain connections, and shutdown gracefully. Connection draining (D) handles the ELB side (stops new requests) but doesn't prevent ASG from terminating instances abruptly. Lifecycle hooks are the complete solution.

---

**Question 16** *(Exam Trap)*

A Route 53 health check is configured to monitor an EC2 instance in a private subnet (no public IP). The health check keeps failing despite the instance being healthy. What is the reason?

A. Route 53 health checks don't support EC2 instances
B. Route 53 health checkers originate from the public internet and cannot reach private IPs
C. The health check interval is too short
D. Private subnet instances require VPC peering for health checks

**Answer: B**

**Explanation:** Route 53 health checkers are distributed globally on the **public internet**. They cannot reach resources in private subnets that have no public IP/internet access. **Solutions:** Use a CloudWatch metric alarm as the health check source, OR put the resource behind an ALB (which has a public IP and can be health-checked).

---

**Question 17** *(Multiple Correct)*

A company wants to deploy a new application version to 100% of their Auto Scaling group instances without downtime. Which options can accomplish this? (Select TWO)

A. Delete the existing launch configuration and create a new one — instances update automatically
B. Use ASG Instance Refresh with a minimum healthy percentage of 90%
C. Terminate all instances manually and let ASG replace them with new launch template
D. Create a new ASG with the new launch template and use weighted routing to shift traffic
E. Update the desired capacity to 0, then back to original

**Answer: B, D**

**Explanation:** Instance Refresh (B) gradually replaces instances while maintaining a minimum healthy percentage — this is the purpose-built AWS feature for rolling updates. Blue/green deployment with weighted routing (D) is another valid zero-downtime approach. Modifying launch config/template doesn't automatically replace running instances (A is false). Terminating all instances (C, E) causes downtime.

---

**Question 18** *(Scenario)*

A company has users in Europe and the US. They want European users to be served content from a Frankfurt server, US users from a Virginia server, and all other users from Singapore by default. What Route 53 routing policy should they use?

A. Latency-based routing
B. Geoproximity routing with bias
C. Geolocation routing with a default record
D. Weighted routing with three records

**Answer: C**

**Explanation:** **Geolocation routing** routes based on the user's geographic location (country/continent). They need: EU → Frankfurt, US → Virginia, default → Singapore. The **default record** handles users not matching specific geolocation rules. Latency routing routes based on network latency (not strict geography). Geoproximity uses physical proximity with bias — more complex and not needed here.

---

**Question 19** *(Exam Trap)*

An ALB is configured with a listener rule that forwards `/api/*` requests to a target group. A request comes in for `/API/users` (uppercase API). Will this match the rule?

A. Yes, ALB listener rules are case-insensitive by default
B. No, ALB listener rules for path conditions are case-sensitive by default
C. Yes, ALB normalizes all paths to lowercase
D. It depends on the backend application's configuration

**Answer: B**

**Explanation:** ALB path condition rules ARE **case-sensitive** by default. `/API/users` does NOT match `/api/*`. To handle case-insensitive matching, you need to either use multiple rules or implement case normalization at the application level. This is a common trap in URL routing questions.

---

**Question 20** *(Complex Multi-Scenario)*

A company has the following requirements:
- Web app serves global users
- Need to minimize latency for each user
- If the closest region is unhealthy, automatically route to next best region
- Want to test a new version with 10% of traffic in one region

Which combination of AWS services and Route 53 policies BEST meets ALL requirements?

A. Geolocation routing + Failover routing + Weighted routing nested policies
B. Latency-based routing with health checks on each regional record + Weighted routing within the us-east-1 region using ALB weighted target groups
C. Simple routing returning all regional IPs + Application handles routing logic
D. CloudFront distribution as origin for all traffic + Route 53 simple routing to CloudFront

**Answer: B**

**Explanation:** Breaking down the requirements:
1. **Minimize latency globally** → Latency-based routing (routes to lowest latency region)
2. **Automatic failover** → Health checks on latency records (unhealthy region removed from responses, next-best latency region serves traffic)
3. **10% canary testing in one region** → Within that region, use ALB weighted target groups (forward 90% to current, 10% to new target group) OR use nested Route 53 weighted records

This is the complete multi-requirement architecture: Route 53 Latency + health checks handles global routing and failover; ALB weighted rules or nested Route 53 weighted records handle canary within a region.

---

## 🔷 FINAL EXAM TRAP SUMMARY

| Topic | Common Trap | Correct Answer |
|---|---|---|
| ELB Types | ALB for static IP | NLB for static IP; ALB needs Global Accelerator |
| CNAME vs ALIAS | CNAME at root domain | ALIAS at root domain |
| ASG Health Checks | ELB auto-integrates | Must explicitly enable ELB health checks in ASG |
| Cooldown vs Warmup | Cooldown for step scaling | Warmup for step scaling; cooldown for simple scaling |
| Termination Policy | "Oldest instance" is default | Default is multi-step: balance AZs first |
| Route 53 Failover | No health check needed | Health check REQUIRED on primary record |
| Private Resources | Route 53 can health check them | Route 53 checkers are public; use CloudWatch alarms |
| Simple Routing | Supports health checks | Does NOT support health checks for routing |
| Geolocation default | Not required | MUST have default record or some users get no answer |
| CLB + SNI | CLB supports multiple certs | CLB = one cert; use ALB for SNI/multiple certs |
| Scale-in exceeds max | ASG can override max temporarily | ASG NEVER exceeds maximum capacity |
| Launch Config update | Running instances update | Only NEW instances use new launch config; existing unchanged |

---

Good luck on your AWS SAA exam! 🎯
