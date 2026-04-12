# Module 5 — Containers & Serverless

## 🔷 TOPIC 1: Serverless Computing — The Big Picture

### What Problem Does It Solve?

In **traditional deployment**, you must:
- Provision EC2 instances
- Patch the OS
- Install runtimes
- Configure auto-scaling and load balancers
- Monitor servers 24/7

In **serverless deployment**, AWS handles ALL of that. You only care about:
- Writing your application code
- Monitoring your application behavior

### Key Mental Model
> Think of serverless like a taxi vs. owning a car. You pay only when you ride, and you don't maintain the engine.

---

## 🔷 TOPIC 2: AWS Lambda

### What Is Lambda?
Lambda is a **serverless compute service** that runs your code in response to events, without you provisioning or managing servers.

### How It Works (The 4-Step Flow)

<img width="1258" height="356" alt="image" src="https://github.com/user-attachments/assets/b25fa176-5ae5-479a-9f9a-a873b97bc235" />

---

### Lambda Triggers — What Can Invoke Lambda?

| Trigger Type | Example |
|---|---|
| AWS Services | S3 file upload, DynamoDB stream, SNS message |
| HTTP Endpoints | API Gateway REST call |
| Schedule | CloudWatch Events (cron-style) |
| Mobile Apps | In-app activity |

### Key Product Features (Memorize These for SAA)

| Feature | What It Means |
|---|---|
| Bring your own code | Python, Node.js, Java, Go, Ruby, .NET supported |
| Automated administration | No OS patching, no server management |
| Built-in fault tolerance | Runs across multiple AZs automatically |
| Automatic scaling | Scales from 1 to thousands of concurrent executions instantly |
| Orchestrate multiple functions | Use Step Functions to chain them |
| Integrated security model | Uses IAM roles for permissions |
| Pay per use | Charged per request + duration |
| Flexible resource model | You choose memory (128MB–10GB); CPU scales proportionally |

### Lambda Limits — CRITICAL for SAA Exam

| Limit Type | Value |
|---|---|
| Max memory per function | 10 GB (note: the slide says 3GB but AWS has since updated to 10GB — always check current docs) |
| Max execution duration | **15 minutes** |
| Max deployment package (zip) | 50 MB zipped, 250 MB unzipped |
| Max /tmp storage | 10 GB |
| Concurrency default limit | 1,000 per region (can be increased) |

> ⚠️ **Exam Trap**: If a function runs longer than 15 minutes, Lambda is the **wrong choice**. Use EC2, ECS, or AWS Batch instead.

### Lambda Layers — What and Why?

A **Lambda Layer** is a ZIP archive containing libraries, custom runtimes, or dependencies that your function needs but shouldn't be bundled inside the deployment package.

**Benefits:**
- Keeps deployment package small (faster cold starts)
- Avoids dependency conflicts
- Reusable across multiple Lambda functions
- Shareable with other AWS accounts

**Real-world analogy:** Think of layers like shared libraries in Linux. Instead of every app bundling libc, they all reference one shared copy.

### Steps to Deploy Lambda (SAA Scenario)

```
Step 1: Write your handler function (e.g., lambda_handler in Python)
Step 2: Create the Lambda function in AWS console or CLI
Step 3: Attach an IAM Execution Role (so Lambda can access other AWS services)
Step 4: Upload your code (zip or container image)
Step 5: Configure trigger and test invocation
Step 6: Monitor with CloudWatch Logs and Metrics
```

###  Scenario: EC2 Start/Stop Automation

**Business problem:** A company pays for EC2 instances running 24/7 but only uses them during business hours.

**Solution with Lambda:**
- CloudWatch Events triggers Lambda at 8 AM → Lambda starts EC2 instances
- CloudWatch Events triggers Lambda at 8 PM → Lambda stops EC2 instances
- The Lambda function uses an IAM role to call EC2 start/stop APIs
- Cost savings: ~65% reduction in compute costs

**SAA exam question style:** *"A solutions architect wants to reduce EC2 costs by stopping non-production instances overnight. What is the MOST cost-effective serverless approach?"*
→ **Answer: AWS Lambda triggered by Amazon CloudWatch Events (EventBridge)**

---

## 🔷 TOPIC 3: APIs and REST

### What Is an API?

An **Application Programming Interface (API)** is a contract that defines how software components communicate. It:
- Provides programmatic access to functionality
- Supports multiple programming languages
- Operates on a request/response model

### What Is REST?

**REST (Representational State Transfer)** is an architectural style for designing networked applications. It uses HTTP as its communication protocol.

### The 6 REST Design Principles (SAA May Test These)

| Principle | Explanation |
|---|---|
| Uniform Interface | Consistent resource identification using URLs |
| Stateless | Each request contains all the info needed; server stores no session state |
| Client-Server Architecture | Client and server are independent; client handles UI, server handles data |
| Cacheable | Responses can be cached to improve performance |
| Layered System | Client doesn't need to know if it's talking directly to the server |
| Code on Demand (optional) | Server can send executable code to clients (e.g., JavaScript) |

### REST Request Components

```
Endpoint:  https://api.example.com/orders/123
Method:    GET / POST / PUT / DELETE
Headers:   Content-Type: application/json
           Authorization: Bearer <token>
Body:      {"item": "coffee", "quantity": 2}  ← used in POST/PUT
```

### HTTP Methods — CRUD Mapping

| HTTP Method | CRUD Operation | Use Case |
|---|---|---|
| GET | Read | Retrieve a resource |
| POST | Create | Submit new data |
| PUT | Update | Replace existing resource |
| DELETE | Delete | Remove a resource |
| PATCH | Partial Update | Modify part of a resource |

### HTTP Status Codes — Must Know for SAA

| Range | Meaning | Common Examples |
|---|---|---|
| 100s | Informational | 100 Continue |
| 200s | Success | 200 OK, 201 Created |
| 300s | Redirection | 301 Moved Permanently |
| 400s | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| 500s | Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

> ⚠️ **Exam Tip**: A **4xx error** means the CLIENT sent a bad request. A **5xx error** means the SERVER failed. This matters when troubleshooting API Gateway issues.

---

## 🔷 TOPIC 4: Amazon API Gateway

### What Is API Gateway?

Amazon API Gateway is a **fully managed service** that allows you to create, publish, maintain, monitor, and secure APIs at any scale.

Think of it as the "front door" to your backend services.

### What API Gateway Handles for You

- **Scaling** — Automatically scales to handle millions of requests per second
- **Access Control** — Authentication via IAM, Cognito, Lambda authorizers
- **Monitoring** — Built-in CloudWatch integration
- **Throttling** — Protects backend from traffic spikes
- **Caching** — Reduces backend calls and latency

### API Gateway Benefits (SAA Favorite)

| Benefit | Detail |
|---|---|
| Low cost and efficient | Pay per API call + data transfer |
| Performance at any scale | Handles millions of API calls |
| Easy monitoring | CloudWatch metrics and access logs |
| Streamlined development | SDK generation, API versioning, staging |
| Flexible security | IAM roles, Cognito user pools, API keys, Lambda authorizers |
| Run without servers | Integrates with Lambda for fully serverless backend |
| RESTful endpoints for existing services | Expose legacy on-prem services as REST APIs |

### API Gateway Architecture

```
Client Apps
    ↓
Amazon API Gateway (REST/HTTP/WebSocket API)
    ↓              ↓              ↓
AWS Lambda    Amazon EC2    Amazon DynamoDB
(Serverless)  (Traditional) (Database)
```

###  Scenario: Serverless Web Application

**Architecture:**
1. **Amazon S3** — Hosts static content (HTML, CSS, JavaScript)
2. **Amazon Cognito** — Handles user registration and authentication
3. **Amazon API Gateway** — Receives REST API calls from the client
4. **AWS Lambda** — Processes business logic (triggered by API Gateway)
5. **Amazon DynamoDB** — Stores application data (NoSQL)

**SAA exam question style:** *"A company wants to build a web app with no server management. Users should authenticate securely and data should be stored in a managed database. What architecture should be used?"*
→ **Answer: S3 (static hosting) + Cognito (auth) + API Gateway + Lambda + DynamoDB**

### Types of APIs in API Gateway

| API Type | Use Case |
|---|---|
| REST API | Standard web APIs, full API Gateway features |
| HTTP API | Lower cost, lower latency, simpler use cases |
| WebSocket API | Real-time bidirectional communication (chat apps, live dashboards) |

---

## 🔷 TOPIC 5: Containers on AWS — Complete Guide

### What Is a Container?

A container is a **standardized unit of software** that packages an application and ALL its dependencies (code, runtime, libraries, system tools, config) into a single portable unit.

- **Runs on any infrastructure** that supports containers
- **Isolated from other containers** (OS-level virtualization)
- **Lightweight** compared to VMs (shares the host OS kernel)

### Containers vs. Virtual Machines

| Feature | Virtual Machine | Container |
|---|---|---|
| Startup time | Minutes | Seconds |
| Size | GBs | MBs |
| OS | Full guest OS per VM | Shares host OS kernel |
| Isolation | Hardware-level | Process-level |
| Portability | Good | Excellent |

### Benefits of Containers

| Benefit | What It Means |
|---|---|
| Environment consistency | "Works on my machine" problem eliminated — same container everywhere |
| Operational efficiency | Higher density, better resource utilization |
| Developer productivity | Faster build-test-deploy cycles |
| Version control | Container images are versioned and immutable |

### What Is Docker?

**Docker** is the most popular platform for building, running, and managing containers.

A Docker container includes:
- Your application code
- Runtime (e.g., Python 3.11, Node.js 18)
- System libraries
- Configuration files

```
Dockerfile → Build → Docker Image → Run → Container
```

### Container Management Challenges

| Challenge | Description |
|---|---|
| Container sprawl | Hundreds of containers become unmanageable without orchestration |
| Version differences | Different teams running different versions causes inconsistency |
| Ownership | Who is responsible for which container? |
| Bin packing | How to optimally place containers on hosts to maximize resource use |
| Zombie containers | Unused, abandoned containers consuming resources |
| Disappearing containers | Short-lived containers make debugging difficult |

This is exactly WHY orchestration services like Amazon ECS and Kubernetes exist.

---

## 🔷 TOPIC 6: Amazon ECS — Elastic Container Service

### What Is ECS?

Amazon ECS is a **highly scalable, high-performance container orchestration service** that supports Docker containers. It manages where and how your containers run.

### Key ECS Concepts

| Concept | Description |
|---|---|
| **Task Definition** | Blueprint for your container (image, CPU, memory, networking, IAM role) |
| **Task** | A running instance of a Task Definition (one or more containers) |
| **Service** | Ensures a specified number of tasks are always running (like Auto Scaling for containers) |
| **Cluster** | Logical grouping of compute resources (EC2 instances or Fargate) |
| **Container Agent** | Software running on EC2 instances that registers them with ECS |

### ECS Benefits

- Container management service (scheduling, placement, scaling)
- Flexible scheduling (place containers based on resource needs)
- Integrated with AWS services (IAM, VPC, ELB, CloudWatch, ECR)
- Security (IAM task roles, VPC network isolation)
- Performance at scale (proven at Amazon.com scale)
- Task definitions (declarative container configuration)

### 📌 ECS Scenario

**Problem:** A company runs a microservices application with 20 different services. They want to deploy each service in Docker containers, ensure high availability, and scale each service independently.

**Solution:** Amazon ECS with an Application Load Balancer
- Each microservice = 1 ECS Service
- ECS maintains desired task count (auto-restarts failed containers)
- ALB routes traffic to the right service
- Auto Scaling adjusts task count based on CPU/memory metrics

---

## 🔷 TOPIC 7: AWS Fargate — Serverless Containers

### What Is Fargate?

AWS Fargate is a **serverless compute engine for containers**. It works with both ECS and EKS. You run containers without managing the underlying EC2 instances.

> Fargate is to containers what Lambda is to functions — you just provide the code/image, AWS handles the infrastructure.

### Fargate vs. ECS on EC2 — When to Use Which

| Use Fargate When... | Use ECS on EC2 When... |
|---|---|
| You don't want to manage EC2 instances | You need Spot Instances or Reserved Instances for cost savings |
| Quick start for new containerized app | Running Windows containers |
| Porting a monolithic app to containers | Needing granular compliance/control over the host |
| Highly variable/unpredictable workloads | Requiring specialized hardware (GPU, high memory) |

### How to Use Fargate (4 Steps)

```
1. Build container image (push to ECR)
2. Specify CPU and memory requirements in Task Definition
3. Define IAM roles and VPC networking policies
4. Launch — Fargate provisions and manages infrastructure
```

### Fargate Pricing Model
- **Resource-based**: Pay for vCPU and memory used per second
- No charge for idle EC2 capacity
- More expensive per hour than EC2, but no waste

### 📌 Fargate Scenario

**Problem:** A startup wants to run their web app in containers but has no DevOps team to manage servers. Workload spikes unpredictably during marketing campaigns.

**Solution:** AWS Fargate on ECS
- No EC2 instances to manage
- Fargate automatically scales during traffic spikes
- Pay only for resources consumed during actual usage
- Team focuses entirely on application development

---

## 🔷 TOPIC 8: Amazon EKS — Elastic Kubernetes Service

### What Is Kubernetes?

**Kubernetes (K8s)** is open-source container orchestration software (originally from Google). It automates deployment, scaling, and management of containerized applications.

Use Kubernetes to:
- Manage clusters of compute instances
- Run containers with automated deployment, maintenance, and scaling

### Kubernetes Key Advantages

| Advantage | Detail |
|---|---|
| Run at scale | Manages thousands of containers across hundreds of nodes |
| Seamlessly move applications | Portable across cloud providers and on-premises |
| Run anywhere | AWS, Azure, GCP, on-premises, hybrid |
| Add new functionality | Massive ecosystem of plugins and operators |

### What Is Amazon EKS?

**Amazon EKS (Elastic Kubernetes Service)** is a managed Kubernetes service. AWS manages the Kubernetes control plane (master nodes). You manage the worker nodes.

### How EKS Works (4 Steps)

```
1. Provision EKS cluster (AWS manages control plane across 3 AZs)
2. Deploy worker nodes (EC2 instances or Fargate)
3. Connect using kubectl (Kubernetes CLI)
4. Deploy and run container workloads
```

### ECS vs. EKS — When to Use Which

| Choose ECS When... | Choose EKS When... |
|---|---|
| AWS-native, simpler setup | Need Kubernetes compatibility |
| Smaller teams, less K8s expertise | Already using Kubernetes on-premises |
| Tighter AWS service integration | Multi-cloud strategy |
| Lambda + container hybrid | Need K8s ecosystem tools (Helm, Istio) |

### 📌 EKS Scenario

**Problem:** A company is migrating from on-premises Kubernetes clusters to AWS. They have existing Kubernetes manifests and don't want to rewrite them.

**Solution:** Amazon EKS
- Existing K8s YAML manifests work without modification
- AWS manages control plane availability
- Workers run on EC2 (or Fargate for serverless nodes)
- Lift-and-shift of Kubernetes workloads to cloud

---

## 🔷 TOPIC 9: Amazon ECR — Elastic Container Registry

### What Is ECR?

Amazon ECR is a **fully managed Docker container registry** — essentially, it's like DockerHub but private, secure, and integrated with AWS.

### ECR Features

| Feature | Detail |
|---|---|
| ECS/EKS Integration | Pull images directly during task/pod launch |
| Docker support | Standard Docker CLI push/pull |
| High availability | Images stored in S3 (durable, redundant) |
| Team collaboration | Share images across accounts |
| Access control | IAM-based permissions per repository |
| Encryption at rest | Images encrypted using AWS KMS |
| Third-party integrations | Works with Jenkins, GitHub Actions, etc. |

### ECR Workflow

```
Developer writes code
    → docker build -t my-app .
    → docker tag my-app <account>.dkr.ecr.<region>.amazonaws.com/my-app
    → docker push → ECR Repository
    → ECS/EKS pulls image from ECR during deployment
```

---

## 🔷 TOPIC 10: AWS Step Functions

### What Is Step Functions?

AWS Step Functions is a **serverless workflow orchestration service** that lets you coordinate multiple AWS services into visual, auditable state machines.

### Core Concepts

| Concept | Description |
|---|---|
| **State Machine** | The overall workflow definition |
| **State** | Each step in the workflow |
| **Task State** | A state that performs work (calls Lambda, ECS, etc.) |
| **Choice State** | Conditional branching (if/else logic) |
| **Parallel State** | Execute multiple branches simultaneously |
| **Wait State** | Pause execution for a defined time |

### How Step Functions Works

```
Step 1: Define workflow steps using Amazon States Language (JSON)
Step 2: Each step calls an AWS service action (Lambda, ECS, SNS, etc.)
Step 3: Output of Step N becomes input to Step N+1
Step 4: Run workflow — can run up to 1 year (max timeout)
Step 5: Swap out individual steps without rewriting entire workflow
```

### Step Functions Benefits

- **Build distributed applications** without writing coordination code
- **Write less code** — branching, retries, error handling built-in
- **Visual workflow** — see execution path in real-time
- **Automatic retries** with exponential backoff
- **Audit trail** — every state transition logged

### 📌 Step Functions Scenario

**Problem:** An e-commerce company's order processing involves: validate payment → check inventory → update database → send confirmation email → update analytics. Each step is a separate Lambda function.

**Without Step Functions:** Each Lambda manually invokes the next, complex error handling code.

**With Step Functions:**
```
[Start]
  → Validate Payment (Lambda)
  → Choice: Payment Approved?
      YES → Check Inventory (Lambda)
           → Update Order DB (DynamoDB)
           → Send Email (SNS → Lambda)
           → Update Analytics (Lambda)
           → [Success]
      NO  → Send Decline Email (SNS)
           → [Failed]
```

- If any step fails → automatic retry → then error path
- Complete audit log of every order
- Easy to add new steps without rewriting everything

### Step Functions Express vs. Standard Workflows

| Feature | Standard | Express |
|---|---|---|
| Max duration | 1 year | 5 minutes |
| Execution model | Exactly-once | At-least-once |
| Use case | Long-running, auditable | High-volume, short-duration |
| Pricing | Per state transition | Per execution duration |

---

## 🔷 TOPIC 11: Key Architectural Patterns (SAA Exam Favorites)

### Pattern 1: Event-Driven Serverless

```
S3 Upload → triggers → Lambda → processes file → stores result in DynamoDB
                                               → sends notification via SNS
```

### Pattern 2: Serverless API Backend

```
Mobile/Web Client
    → API Gateway (REST API)
    → Lambda (business logic)
    → DynamoDB (data storage)
    → Cognito (authentication)
```

### Pattern 3: Containerized Microservices

```
Internet → ALB → ECS Service A (Container)
               → ECS Service B (Container)
               → ECS Service C (Container)
Each service scales independently, communicates via internal ALB or Service Discovery
```

### Pattern 4: Serverless Container Workflow

```
EventBridge (trigger) → Step Functions → ECS Fargate Task (heavy processing)
                                       → Lambda (lightweight notification)
                                       → SNS (send results)
```

---

## 🔷 TOPIC 12: Comparison Tables for SAA Exam

### Lambda vs. ECS/Fargate vs. EC2

| Factor | Lambda | Fargate | ECS on EC2 | EC2 |
|---|---|---|---|---|
| Server management | None | None | Some | Full |
| Max runtime | 15 min | No limit | No limit | No limit |
| Scaling | Automatic | Automatic | Configurable | Manual/Auto |
| State | Stateless | Stateful possible | Stateful possible | Stateful |
| Cost model | Per 100ms | Per second | Per EC2 hour | Per EC2 hour |
| Best for | Short event-driven tasks | Containerized apps | Cost-optimized containers | Full control |

### ECS vs. EKS

| Factor | ECS | EKS |
|---|---|---|
| Orchestrator | AWS proprietary | Kubernetes (open-source) |
| Complexity | Simpler | More complex |
| Learning curve | Lower | Higher |
| AWS integration | Deeper | Good |
| Portability | AWS only | Multi-cloud |
| Control plane cost | Free | $0.10/hour per cluster |

---

## 🔷 SAA Exam Cheat Sheet — Module 5

### Quick-Fire Facts

- ✅ Lambda max runtime = **15 minutes**
- ✅ Lambda scales to **thousands of concurrent executions**
- ✅ Lambda pays per **request + duration (per 1ms)**
- ✅ Lambda Layers = **shared dependencies as ZIP archives**
- ✅ REST uses **HTTP** as transport protocol
- ✅ REST is **stateless** — no session stored on server
- ✅ API Gateway = **fully managed API management** (scales automatically)
- ✅ Containers = **OS-level virtualization** (lighter than VMs)
- ✅ Docker = **platform for building and running containers**
- ✅ ECS = **AWS-native container orchestration**
- ✅ Fargate = **serverless container runtime** (no EC2 management)
- ✅ EKS = **managed Kubernetes on AWS**
- ✅ ECR = **private Docker container registry on AWS**
- ✅ Step Functions = **serverless workflow orchestration** (up to 1 year)
- ✅ Step Functions Express = **high-volume, max 5 min** workflows

### Common Exam Scenarios & Answers

| Scenario | Best Answer |
|---|---|
| Run code in response to S3 upload, no server management | AWS Lambda |
| Task runs for 20 minutes, no server management | ECS Fargate (Lambda max = 15 min) |
| Need Kubernetes compatibility on AWS | Amazon EKS |
| Store and manage Docker images | Amazon ECR |
| Chain multiple Lambda functions with error handling | AWS Step Functions |
| Create REST API for Lambda backend | Amazon API Gateway |
| Containerized app, no cluster management, variable load | AWS Fargate |
| Need Spot Instances for containers to reduce costs | ECS on EC2 |
| Authenticate users for a serverless web app | Amazon Cognito + API Gateway |

---

## 🔷 Scenario Practice Questions

**Q1:** A company wants to process images uploaded to S3. Processing takes 3 minutes per image. They want zero server management. What should they use?

**Answer:** AWS Lambda (3 min < 15 min limit) triggered by S3 event notification.

---

**Q2:** A data pipeline runs complex ML batch jobs that take 6 hours. They want to use containers with no server management. What service?

**Answer:** AWS Fargate on ECS (no server management, no time limit like Lambda has).

---

**Q3:** A startup wants to expose their monolithic on-premises application as a RESTful API to mobile clients. The backend is in a private VPC. What should they use?

**Answer:** Amazon API Gateway (creates REST API) → VPC Link → Private EC2/ECS in VPC.

---

**Q4:** A financial company has a multi-step transaction workflow: validate → process → notify → audit. Each step can fail and must retry. Full audit trail required. What service?

**Answer:** AWS Step Functions (Standard Workflow) — built-in retry, error handling, and execution history.

---

**Q5:** A company is moving from on-premises Kubernetes to AWS and wants to reuse existing Helm charts and kubectl commands.

**Answer:** Amazon EKS — fully compatible with upstream Kubernetes tooling.

---

This covers all major topics from Module 5 in depth. The key themes for SAA are: **when to use Lambda vs. containers vs. EC2**, **how API Gateway enables serverless APIs**, **ECS vs. EKS vs. Fargate trade-offs**, and **Step Functions for workflow orchestration**. Master these patterns and you'll handle the compute/serverless section of the SAA exam confidently.
# 20 AWS SAA Certification-Level MCQs — Containers & Serverless

### Exam Rules Applied:
- ⚠️ = Exam Trap present
- 🔵 = Single correct answer
- 🟢 = Multiple correct answers (select stated number)
- Full explanation after each question

---

## ❓ QUESTION 1
🔵 **Single Answer**

A company has a Lambda function that processes customer orders. During peak sales events, the function is invoked thousands of times per second. The team notices that some invocations are failing with a **TooManyRequestsException** error. What is the MOST likely cause?

- **A.** The Lambda function has exceeded its 15-minute timeout limit
- **B.** The Lambda function has exceeded the regional concurrency limit
- **C.** The Lambda deployment package is too large
- **D.** The Lambda function ran out of /tmp storage space

---

### ✅ Answer: **B**

### 📖 Explanation:

**Why B is correct:**
- `TooManyRequestsException` (HTTP 429) is specifically the error thrown when Lambda hits its **concurrency limit**
- Default regional concurrency limit = **1,000 concurrent executions**
- During massive traffic spikes, if 1,000 functions are already running, new invocations get throttled
- **Fix:** Request a concurrency limit increase via AWS Support, or use **Reserved Concurrency** and **Provisioned Concurrency**

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | Timeout errors produce a different error — `Task timed out after X seconds` |
| C | Deployment package size causes deployment failure, not runtime failure |
| D | /tmp exhaustion causes a storage error, not TooManyRequests |

⚠️ **Exam Trap:** Many students confuse timeout errors with throttling errors. The keyword `TooManyRequestsException` always points to **concurrency/throttling**, not duration.

---

## ❓ QUESTION 2
🟢 **Multiple Answer — Select TWO**

A solutions architect is designing a system where a Lambda function needs to connect to an **Amazon RDS database inside a private VPC** and also call a **public AWS S3 API endpoint**. Which TWO configurations are required?

- **A.** Configure the Lambda function to run inside the VPC by specifying VPC, subnets, and security groups
- **B.** Configure the Lambda function with a public IP address
- **C.** Create a VPC Endpoint for S3 (Gateway Endpoint) or ensure the VPC has a NAT Gateway for internet access
- **D.** Attach an Internet Gateway directly to the Lambda function
- **E.** Deploy Lambda in the same Availability Zone as the RDS instance

---

### ✅ Answer: **A and C**

### 📖 Explanation:

**Why A is correct:**
- By default, Lambda runs in an AWS-managed VPC and **cannot access resources in your private VPC**
- To access RDS in a private VPC, you must configure Lambda with:
  - Your **VPC ID**
  - **Private subnets** (where ENIs will be created)
  - **Security groups** (to allow traffic to RDS port 3306/5432)

**Why C is correct:**
- Once Lambda is inside your VPC, it **loses internet access** by default
- To call public AWS APIs (like S3), you need either:
  - **NAT Gateway** in a public subnet + Lambda in private subnet (costs money)
  - **VPC Gateway Endpoint for S3** (free, routes S3 traffic through private AWS network)

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| B | Lambda functions don't have public IPs — they use ENIs in your VPC |
| D | Internet Gateway attaches to VPC, not individual functions |
| E | Lambda doesn't need to be in the same AZ as RDS — it connects via security group rules |

⚠️ **Exam Trap:** This is one of the most tested Lambda networking scenarios. Remember: **Lambda in VPC = loses internet by default = needs NAT or VPC Endpoints**.

---

## ❓ QUESTION 3
🔵 **Single Answer**

A company's data processing job takes **25 minutes** to complete. The job needs to process files from S3 and write results to DynamoDB. The team wants a **fully serverless solution with no server management**. What should the solutions architect recommend?

- **A.** AWS Lambda with a 25-minute timeout configuration
- **B.** AWS Fargate on Amazon ECS triggered by an S3 event
- **C.** AWS Lambda chained with Step Functions to extend execution time
- **D.** Amazon EC2 Spot Instances with Auto Scaling

---

### ✅ Answer: **B**

### 📖 Explanation:

**Why B is correct:**
- Lambda has a **hard maximum timeout of 15 minutes** — this cannot be increased regardless of configuration
- AWS Fargate runs containers with **no time limit** and **no server management**
- Architecture: S3 Event → EventBridge → ECS Task (Fargate) → DynamoDB
- Fargate is fully serverless (no EC2 instances to manage)

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | Lambda's maximum timeout is 15 minutes — you CANNOT set it to 25 minutes. This is a hard limit |
| C | Step Functions chaining doesn't extend a single Lambda's runtime — each Lambda still has its own 15-min limit |
| D | EC2 Spot Instances require server management and are not serverless |

⚠️ **Exam Trap:** This is the #1 Lambda trap on SAA. Students try to increase Lambda timeout beyond 15 minutes. **It is impossible.** Any job > 15 minutes = use Fargate, ECS, Glue, or Batch.

---

## ❓ QUESTION 4
🟢 **Multiple Answer — Select THREE**

A company wants to use **AWS Fargate** instead of **ECS on EC2**. Which THREE scenarios justify choosing Fargate over ECS with EC2 launch type?

- **A.** The workload is unpredictable and scales from zero to high traffic multiple times per day
- **B.** The company needs to use Reserved Instances to minimize container costs
- **C.** The development team has no DevOps expertise and wants zero server management
- **D.** The company needs to run Windows Server containers
- **E.** The company is migrating a monolithic application to containers for the first time
- **F.** The workload requires GPU-accelerated computing

---

### ✅ Answer: **A, C, and E**

### 📖 Explanation:

**Why A is correct:**
- Fargate is ideal for **variable/unpredictable workloads** — you only pay for resources used
- No need to pre-provision EC2 capacity for peak load
- Scales to zero (no idle costs)

**Why C is correct:**
- Fargate eliminates all EC2 fleet management — no patching, no capacity planning
- Perfect for teams without dedicated infrastructure management

**Why E is correct:**
- When porting a monolithic app to containers, Fargate provides the simplest migration path
- No need to learn EC2 cluster management while simultaneously learning containers

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| B | Fargate does NOT support Reserved Instances — use ECS on EC2 for Reserved/Spot Instance discounts |
| D | Windows containers are NOT supported on Fargate — requires ECS on EC2 with Windows AMI |
| F | Fargate does NOT support GPU instances — requires ECS on EC2 with GPU-enabled EC2 instances |

⚠️ **Exam Trap:** Windows containers and GPU workloads are classic "sounds like Fargate" traps. These require **EC2 launch type**, not Fargate.

---

## ❓ QUESTION 5
🔵 **Single Answer**

A mobile gaming company uses **API Gateway + Lambda**. Their API receives **50,000 requests per second** during game launch events. Users report seeing **502 Bad Gateway errors** during peak periods. What is the MOST likely cause?

- **A.** API Gateway has reached its regional throttle limit of 10,000 RPS
- **B.** Lambda concurrency limit has been reached, causing Lambda to reject requests, which API Gateway reports as 502
- **C.** The SSL certificate on API Gateway has expired
- **D.** The Lambda function's memory setting is too low causing out-of-memory errors

---

### ✅ Answer: **B**

### 📖 Explanation:

**Why B is correct:**
- When Lambda is throttled (concurrency exceeded), it returns a **429 error** to API Gateway
- API Gateway translates Lambda throttling errors into **502 Bad Gateway** responses to the client
- This is the most common cause of 502 errors in API Gateway + Lambda architectures

**The error chain:**
```
Client → API Gateway → Lambda (throttled = 429)
                    ← API Gateway converts to 502 Bad Gateway
Client ← 502 Bad Gateway
```

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | API Gateway account-level default is 10,000 RPS per region, but 502 indicates a backend failure, not API Gateway throttling (which returns 429) |
| C | SSL certificate expiry would affect all requests, not just peak periods; would show SSL handshake errors |
| D | OOM errors cause Lambda to return an error payload, but API Gateway shows this as 502 — however, the scenario specifies PEAK periods pointing to concurrency, not memory |

⚠️ **Exam Trap:** Students confuse **429** (throttling at API Gateway level) with **502** (backend/Lambda failure). Know that Lambda throttling appears as **502** to the API Gateway client.

---

## ❓ QUESTION 6
🟢 **Multiple Answer — Select TWO**

A company runs a **containerized microservices application** on Amazon ECS. They want to ensure that sensitive database credentials are NOT hardcoded in the container image or Task Definition. Which TWO approaches should the solutions architect recommend?

- **A.** Store credentials in AWS Secrets Manager and reference them in the ECS Task Definition
- **B.** Embed credentials directly in the Dockerfile as environment variables
- **C.** Store credentials in AWS Systems Manager Parameter Store (SecureString) and inject them as environment variables in the Task Definition
- **D.** Store credentials in an S3 bucket and download them at container startup using a shell script
- **E.** Pass credentials as command-line arguments when running the ECS task

---

### ✅ Answer: **A and C**

### 📖 Explanation:

**Why A is correct:**
- **AWS Secrets Manager** is purpose-built for storing sensitive credentials
- ECS natively integrates with Secrets Manager — you reference the secret ARN in the Task Definition
- Secret is injected as an environment variable at runtime, never stored in the image
- Secrets Manager also supports **automatic rotation** of credentials

**Why C is correct:**
- **SSM Parameter Store with SecureString** (encrypted with KMS) is a cost-effective alternative
- ECS Task Definition supports direct reference to SSM SecureString parameters
- No credentials in code, Dockerfile, or ECR image

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| B | Credentials in Dockerfile get baked into the image — visible in ECR and to anyone with image access. Major security violation |
| D | Downloading from S3 requires S3 credentials, and the script itself could expose credentials in logs |
| E | Command-line arguments are visible in process lists and CloudTrail logs |

⚠️ **Exam Trap:** The SAA exam frequently tests secrets management. **Never hardcode credentials anywhere**. The correct pattern is always Secrets Manager or SSM Parameter Store with IAM role-based access.

---

## ❓ QUESTION 7
🔵 **Single Answer**

A company uses **AWS Step Functions** to orchestrate an order processing workflow with 8 steps. The workflow runs approximately **2 minutes** and processes **10 million orders per day**. The solutions architect wants to **minimize cost** while maintaining reliability. Which Step Functions workflow type should be used?

- **A.** Standard Workflow — because it provides exactly-once execution semantics
- **B.** Express Workflow (Synchronous) — because it's optimized for high-volume, short-duration workloads
- **C.** Standard Workflow — because it supports workflows longer than 5 minutes
- **D.** Express Workflow (Asynchronous) — because it processes the highest volume at lowest cost

---

### ✅ Answer: **D**

### 📖 Explanation:

**Workflow Type Comparison:**

| Feature | Standard | Express Sync | Express Async |
|---|---|---|---|
| Max duration | 1 year | 5 minutes | 5 minutes |
| Execution model | Exactly-once | At-least-once | At-least-once |
| Pricing | Per state transition | Per execution duration | Per execution duration |
| Volume | Lower | High | **Highest** |
| Use case | Long, auditable | Short, need response | **High-volume, event processing** |

**Why D is correct:**
- 2 minutes < 5 minute Express limit ✓
- 10 million orders/day = very high volume → Express pricing is **significantly cheaper**
- Asynchronous Express is best for **event-driven, high-throughput** pipelines
- Does NOT need to wait for completion response (fire and forget per order)

**Why A and C are wrong:**
- Standard Workflows are billed per **state transition** — at 10M orders/day with 8 steps = 80M state transitions/day → expensive

**Why B is wrong:**
- Synchronous Express waits for a response — adds unnecessary latency overhead for a background order pipeline

⚠️ **Exam Trap:** Students default to Standard Workflow thinking "more features = better." For **high-volume + short-duration**, Express Async is almost always the cost-optimal answer.

---

## ❓ QUESTION 8
🔵 **Single Answer**

A developer is building a REST API using **API Gateway and Lambda**. During testing, they notice that **identical GET requests** are hitting Lambda every time, causing unnecessary database queries and high costs. What is the MOST cost-effective solution?

- **A.** Increase Lambda memory to speed up processing
- **B.** Enable API Gateway caching on the stage and configure a TTL
- **C.** Use Lambda Provisioned Concurrency to pre-warm functions
- **D.** Switch from REST API to HTTP API in API Gateway

---

### ✅ Answer: **B**

### 📖 Explanation:

**Why B is correct:**
- **API Gateway Stage Caching** stores Lambda responses for a configurable TTL (0–3600 seconds)
- Identical GET requests with the same parameters return cached responses without invoking Lambda
- Dramatically reduces Lambda invocations and downstream database queries
- Cost reduction: Fewer Lambda executions + fewer DB queries
- Cache can be invalidated on demand when data changes

**How it works:**
```
Client → GET /products
API Gateway Cache HIT? → Return cached response (no Lambda invoked)
API Gateway Cache MISS? → Invoke Lambda → Store response → Return to client
```

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | More Lambda memory makes execution faster, but doesn't reduce the NUMBER of invocations |
| C | Provisioned Concurrency eliminates cold starts (latency), not unnecessary invocations (cost) |
| D | HTTP API is cheaper per request but doesn't have built-in response caching like REST API |

⚠️ **Exam Trap:** Provisioned Concurrency and caching are frequently confused. **Provisioned Concurrency = warm containers = lower latency. API Gateway Caching = fewer Lambda calls = lower cost.**

---

## ❓ QUESTION 9
🟢 **Multiple Answer — Select TWO**

A company is migrating to AWS and wants to run their **existing on-premises Kubernetes workloads** on AWS with **minimal code changes**. They currently use **Helm charts, kubectl, and custom Kubernetes operators**. Which TWO services should the solutions architect recommend?

- **A.** Amazon ECS with Fargate launch type
- **B.** Amazon EKS with EC2 node groups
- **C.** Amazon EKS with Fargate profiles
- **D.** AWS Lambda with container image support
- **E.** Amazon ECS with EC2 launch type

---

### ✅ Answer: **B and C**

### 📖 Explanation:

**Why B is correct:**
- Amazon EKS is **fully compatible with upstream Kubernetes**
- Existing Helm charts, kubectl commands, and custom operators work without modification
- EC2 node groups give full control over the worker nodes (needed for specialized operators that require node-level access)

**Why C is correct:**
- EKS with Fargate profiles provides **serverless Kubernetes nodes**
- Still fully Kubernetes-compatible (same kubectl, same manifests)
- Eliminates EC2 node management while maintaining K8s compatibility
- Choose Fargate profiles for stateless workloads within the same EKS cluster

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | ECS uses AWS-proprietary orchestration — Kubernetes manifests, Helm, kubectl do NOT work with ECS |
| D | Lambda doesn't support Kubernetes tooling or orchestration |
| E | ECS with EC2 also doesn't support Kubernetes tooling |

⚠️ **Exam Trap:** ECS and EKS are both container services but they are **NOT interchangeable**. ECS = AWS-native. EKS = Kubernetes. If the question mentions kubectl, Helm, or Kubernetes — the answer is always EKS.

---

## ❓ QUESTION 10
🔵 **Single Answer**

A financial services company uses **Lambda functions** to process real-time transactions. They are experiencing **cold start latency** of 2-3 seconds that is causing poor user experience. The functions are invoked irregularly throughout the day. What is the BEST solution?

- **A.** Increase Lambda function memory allocation to 10 GB
- **B.** Enable Lambda Provisioned Concurrency for the function
- **C.** Deploy the Lambda function inside a VPC for faster networking
- **D.** Use Lambda Layers to reduce the deployment package size

---

### ✅ Answer: **B**

### 📖 Explanation:

**What is a Cold Start?**
When Lambda hasn't been invoked recently, AWS must:
1. Find compute capacity
2. Download your deployment package
3. Start the runtime (JVM, Node, Python interpreter)
4. Run initialization code

This process takes 100ms to 3+ seconds depending on runtime and package size.

**Why B is correct:**
- **Provisioned Concurrency** pre-initializes a specified number of Lambda execution environments
- These environments are kept warm and ready to respond immediately (0ms cold start)
- Ideal for latency-sensitive applications like financial transaction processing
- You pay for the provisioned concurrency even when not being used (tradeoff: cost vs. latency)

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | More memory speeds up execution time, but doesn't eliminate the cold start initialization time |
| C | Placing Lambda inside a VPC actually **increases** cold start time (ENI attachment adds latency) |
| D | Layers reduce package size which can slightly reduce cold starts, but this is not the definitive solution |

⚠️ **Exam Trap:** VPC + Lambda is a classic trap. Students think "VPC = faster networking." Wrong. **Lambda inside VPC = longer cold start** because of ENI attachment. Use VPC only when you need to access private VPC resources.

---

## ❓ QUESTION 11
🟢 **Multiple Answer — Select THREE**

A company is building a **serverless e-commerce platform**. The architecture includes API Gateway, Lambda, and DynamoDB. A security review identifies several concerns. Which THREE security best practices should the solutions architect implement?

- **A.** Assign a unique IAM execution role to each Lambda function with least-privilege permissions
- **B.** Use a single IAM role shared across all Lambda functions for simplicity
- **C.** Enable API Gateway resource policies to restrict API access by IP address or VPC
- **D.** Store API keys and database connection strings in Lambda environment variables encrypted with AWS KMS
- **E.** Enable AWS WAF on API Gateway to protect against common web exploits
- **F.** Disable CloudWatch logging for Lambda to reduce costs

---

### ✅ Answer: **A, C, and E**

### 📖 Explanation:

**Why A is correct:**
- **Principle of Least Privilege** — each Lambda function should only have permissions to the specific resources it needs
- A payment Lambda should not have access to the admin Lambda's DynamoDB table
- Separate roles = blast radius containment if one function is compromised

**Why C is correct:**
- **API Gateway Resource Policies** can restrict:
  - Specific IP ranges (e.g., corporate office only)
  - Specific VPCs (for private APIs)
  - Specific AWS accounts
- This adds a network-level security layer before the request even reaches Lambda

**Why E is correct:**
- **AWS WAF (Web Application Firewall)** on API Gateway protects against:
  - SQL injection
  - Cross-site scripting (XSS)
  - DDoS attacks
  - Bot traffic
- Essential for public-facing e-commerce APIs

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| B | Shared IAM role violates least-privilege — compromise of one function compromises all |
| D | Environment variables encrypted with KMS is better than plaintext, but **Secrets Manager** is the best practice for secrets |
| F | Disabling CloudWatch is a terrible practice — you lose all observability and debugging capability |

⚠️ **Exam Trap:** Option D is partially right (KMS-encrypted env vars is acceptable) but the question asks for best practices — Secrets Manager is superior. Watch for "acceptable" vs. "best" in SAA questions.

---

## ❓ QUESTION 12
🔵 **Single Answer**

A company runs a **multi-step data pipeline** using Step Functions and Lambda. The pipeline fails at Step 4 out of 7 steps due to a temporary DynamoDB throttling error. What happens by default in AWS Step Functions Standard Workflow?

- **A.** The entire state machine execution fails and all steps must be restarted from Step 1
- **B.** Step Functions retries the failed step based on configured retry logic, then moves to the Catch block if retries are exhausted
- **C.** Step Functions automatically skips the failed step and continues to Step 5
- **D.** Step Functions rolls back all completed steps (Steps 1-3) using compensating transactions

---

### ✅ Answer: **B**

### 📖 Explanation:

**Step Functions Error Handling — How It Works:**

```
Step 4 fails (DynamoDB throttling)
    ↓
Check Retry configuration:
  - ErrorEquals: ["DynamoDB.ThrottlingException"]
  - IntervalSeconds: 2
  - MaxAttempts: 3
  - BackoffRate: 2
    ↓
Retry 1 (after 2 seconds) → Still fails?
Retry 2 (after 4 seconds) → Still fails?
Retry 3 (after 8 seconds) → Still fails?
    ↓
Move to Catch block → Execute error handling state
    ↓
(e.g., send SNS alert, log to DynamoDB, notify operations team)
```

**Why B is correct:**
- Step Functions has built-in **Retry** (with exponential backoff) and **Catch** (error routing) mechanisms
- You configure these in the state machine definition
- Transient errors like throttling are automatically retried

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | Failure ≠ automatic restart from beginning. Step Functions preserves execution state |
| C | Step Functions does NOT skip failed states — it retries or catches |
| D | Step Functions does NOT automatically roll back completed steps — you must implement compensating logic manually in the Catch block |

⚠️ **Exam Trap:** Step Functions does NOT provide automatic rollback/saga patterns. If rollback is required, you must design compensating transactions manually in your Catch states.

---

## ❓ QUESTION 13
🟢 **Multiple Answer — Select TWO**

A company is deploying a containerized application on **Amazon ECS with EC2 launch type**. They want to ensure that their containers can access **AWS Secrets Manager** and **Amazon S3** securely without hardcoding credentials. Which TWO configurations enable this?

- **A.** Assign an IAM Instance Profile to the EC2 container instances for S3 and Secrets Manager access
- **B.** Create an IAM Task Role in the ECS Task Definition for fine-grained container-level permissions
- **C.** Hardcode AWS access keys in the Docker environment variables
- **D.** Store credentials in ECS cluster configuration
- **E.** Use the EC2 instance's root account credentials

---

### ✅ Answer: **A and B**

### 📖 Explanation:

**Two Levels of IAM in ECS on EC2:**

**Level 1 — EC2 Instance Profile (Option A):**
- Applies to the **EC2 host instance** and the ECS container agent
- Needed for: Pulling images from ECR, sending logs to CloudWatch, registering with ECS
- Permissions here apply to ALL containers on that instance

**Level 2 — ECS Task IAM Role (Option B):**
- Applies specifically to the **individual ECS Task (container)**
- Overrides instance-level permissions for application code
- Each task can have a different role with different permissions
- This is the **recommended approach** for application-level AWS service access

**Best Practice Architecture:**
```
EC2 Instance Role → ECR pull, CloudWatch logs, ECS registration
Task IAM Role → S3 access, Secrets Manager, DynamoDB (task-specific)
```

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| C | Hardcoded credentials = massive security risk, no rotation, visible in image |
| D | No such thing as "ECS cluster configuration" for credential storage |
| E | Root account credentials should NEVER be used for application access |

⚠️ **Exam Trap:** Many students only know about Instance Profiles and miss the Task Role concept. **Task Role = container-level IAM = best practice for ECS applications.**

---

## ❓ QUESTION 14
🔵 **Single Answer**

A company uses **Amazon API Gateway** to expose their microservices. During a traffic spike, downstream **Lambda functions start throttling**. The product team wants to protect the backend while still providing a response to clients. What feature should the solutions architect implement?

- **A.** Enable API Gateway caching to serve stale responses
- **B.** Configure API Gateway Usage Plans with throttling limits and enable Lambda Dead Letter Queues
- **C.** Enable API Gateway throttling at the stage/method level and configure Lambda Reserved Concurrency
- **D.** Deploy a CloudFront distribution in front of API Gateway

---

### ✅ Answer: **C**

### 📖 Explanation:

**Two-Layer Throttling Strategy:**

**Layer 1 — API Gateway Throttling:**
- Set **Rate limit** (requests per second) and **Burst limit** at stage or method level
- API Gateway returns **429 Too Many Requests** to clients that exceed limits
- Protects Lambda from being overwhelmed
- Clients can implement retry with exponential backoff

**Layer 2 — Lambda Reserved Concurrency:**
- Sets a **maximum concurrency** cap for a specific Lambda function
- Prevents one Lambda from consuming all regional concurrency
- Ensures other Lambda functions still have available concurrency
- If limit reached, returns throttle error to API Gateway

**Together these provide:**
```
Client → API Gateway (throttle at 5000 RPS)
       → Lambda (reserved concurrency = 500)
       → Backend services protected from overload
```

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | Caching serves cached responses, but doesn't control the rate of new requests hitting Lambda |
| B | DLQ is for async Lambda invocations (captures failed events) — not a throttling protection mechanism for sync API calls |
| D | CloudFront adds CDN caching for static content/GET requests, but doesn't address throttling of Lambda specifically |

⚠️ **Exam Trap:** Dead Letter Queues only work with **asynchronous Lambda invocations** (SNS, S3 events). For synchronous invocations through API Gateway, DLQ has no effect.

---

## ❓ QUESTION 15
🟢 **Multiple Answer — Select TWO**

A company stores Docker images in **Amazon ECR**. They want to ensure that images are scanned for vulnerabilities and old images are automatically deleted to reduce storage costs. Which TWO features of Amazon ECR should they enable?

- **A.** ECR Image Scanning (on push or scheduled)
- **B.** ECR Cross-Region Replication
- **C.** ECR Lifecycle Policies to automatically expire old images
- **D.** ECR Image Tag Immutability
- **E.** ECR Public Gallery publication

---

### ✅ Answer: **A and C**

### 📖 Explanation:

**Why A is correct:**
- **ECR Image Scanning** uses Clair (open-source) or Amazon Inspector for scanning
- **Scan on push** — automatically scans every image when pushed to repository
- Results show CVE (Common Vulnerability and Exposures) findings by severity: Critical, High, Medium, Low
- Can integrate with EventBridge to trigger Lambda alerts on critical findings
- Enhanced scanning (Amazon Inspector) provides continuous scanning, not just on-push

**Why C is correct:**
- **ECR Lifecycle Policies** define rules to automatically expire and delete images
- Example rules:
  - Delete untagged images older than 7 days
  - Keep only the last 10 tagged images
  - Delete images with tag prefix "dev-" after 30 days
- Runs automatically, reduces storage costs without manual intervention

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| B | Cross-Region Replication copies images to other regions for availability — doesn't scan or delete |
| D | Tag Immutability prevents tag overwriting (security feature) — doesn't scan or delete old images |
| E | Publishing to ECR Public Gallery makes images public — this is the opposite of what a security-conscious company wants |

⚠️ **Exam Trap:** Tag Immutability sounds like a cleanup feature but it's actually a security feature that **prevents** re-tagging. Lifecycle Policies are the cost-management feature.

---

## ❓ QUESTION 16
🔵 **Single Answer**

A company wants to build a **real-time bidding system** that processes auction bids. When a bid arrives, the system must: (1) validate the bid, (2) update the current highest bid, (3) notify all auction participants, (4) log the transaction. If any step fails, the previous steps should be compensated. Which architecture BEST handles this?

- **A.** Four separate Lambda functions invoked sequentially using Lambda destinations
- **B.** AWS Step Functions with States for each step, Retry configurations, and Catch blocks with compensating Lambda functions
- **C.** Amazon SQS queue feeding into a single Lambda function that handles all four steps
- **D.** API Gateway directly invoking DynamoDB, SNS, and CloudWatch Logs using service integrations

---

### ✅ Answer: **B**

### 📖 Explanation:

**Why B is correct:**

Step Functions provides:
- **Ordered execution** of all 4 steps with state tracking
- **Retry logic** per step (exponential backoff for transient failures)
- **Catch blocks** that trigger compensating transactions (e.g., if notification fails after bid is updated, revert the bid)
- **Visual audit trail** — every bid execution is traceable
- **Error isolation** — failure in Step 3 (notify) triggers Step 3's Catch, not a full crash

```
[Validate Bid]
    → Success → [Update Highest Bid]
                → Success → [Notify Participants]
                            → Success → [Log Transaction] → Done
                            → Fail → Catch → [Revert Bid Update] → Alert
                → Fail → Catch → [Return Validation Error]
    → Fail → [Return Error to Client]
```

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | Lambda Destinations handle success/failure routing but don't provide retry, state tracking, or complex branching |
| C | Single Lambda with SQS doesn't provide step-level error isolation or compensating transaction logic |
| D | Direct service integrations don't provide workflow orchestration, retry logic, or compensation |

⚠️ **Exam Trap:** "Sequential Lambda functions" sounds right but lacks the workflow management, retry, and compensation capabilities. Step Functions is specifically designed for exactly this pattern.

---

## ❓ QUESTION 17
🟢 **Multiple Answer — Select TWO**

A company's ECS containerized application needs to store **temporary session data** that is shared between multiple container instances running the same ECS service. Which TWO solutions should the solutions architect recommend?

- **A.** Store session data in Lambda /tmp storage
- **B.** Use Amazon ElastiCache (Redis) for shared in-memory session storage
- **C.** Use Amazon EFS (Elastic File System) mounted to ECS tasks for shared session storage
- **D.** Store session data in each container's local filesystem
- **E.** Use CloudFront to cache session data at edge locations

---

### ✅ Answer: **B and C**

### 📖 Explanation:

**The Problem:**
ECS runs multiple task instances for high availability. If session data is stored locally in a container, when a user's next request goes to a different container instance, the session is lost.

**Why B is correct:**
- **ElastiCache Redis** is purpose-built for shared, low-latency session storage
- All container instances connect to the same Redis cluster
- Sub-millisecond read/write performance
- Redis supports TTL (session expiration), data structures for session objects
- **Industry best practice** for distributed session management

**Why C is correct:**
- **Amazon EFS** can be mounted to multiple ECS tasks simultaneously
- Supports **ReadWriteMany** access mode (multiple tasks read/write same filesystem)
- Good for session data that needs to persist beyond container restarts
- Works with both ECS on EC2 and Fargate

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | Lambda /tmp is not relevant to ECS, and it's temporary, per-invocation, and not shared |
| D | Local container filesystem is ephemeral and NOT shared across container instances |
| E | CloudFront is a CDN for caching content delivery — not designed for application session storage |

⚠️ **Exam Trap:** Local container filesystem storage is a common mistake. Containers are **ephemeral** — local data is lost when the container stops. Always use external shared storage.

---

## ❓ QUESTION 18
🔵 **Single Answer**

A company wants to grant a **third-party SaaS vendor** read-only access to specific objects in their **S3 bucket** for data processing. The SaaS vendor uses a Lambda function in **their own AWS account** to access the data. What is the MOST secure and operationally simple approach?

- **A.** Create an IAM user in your account, generate access keys, and share them with the vendor
- **B.** Use a Resource-based S3 Bucket Policy that grants the vendor's Lambda execution role cross-account read access
- **C.** Make the S3 bucket public and provide the vendor with the bucket URL
- **D.** Copy the data to the vendor's S3 bucket nightly using a Lambda function

---

### ✅ Answer: **B**

### 📖 Explanation:

**Why B is correct:**
- **S3 Bucket Policy** (resource-based policy) can grant cross-account access to a specific IAM role (the vendor's Lambda execution role)
- No credentials to share or manage
- The vendor's Lambda assumes their own IAM role → S3 bucket policy allows that specific role's ARN
- Access is scoped to **read-only** specific objects/prefixes
- Revoke access instantly by updating the bucket policy

**Cross-Account Access Pattern:**
```
Vendor's Lambda (Account B, Role ARN: arn:aws:iam::VENDOR::role/LambdaRole)
    → Calls S3 in Account A
    → S3 Bucket Policy in Account A allows:
        Principal: "arn:aws:iam::VENDOR-ACCOUNT-ID:role/LambdaRole"
        Action: ["s3:GetObject"]
        Resource: ["arn:aws:s3:::your-bucket/shared-data/*"]
```

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | IAM user with access keys is a security anti-pattern — keys can be leaked, not easily audited, no MFA enforcement |
| C | Making S3 public exposes data to the entire internet — catastrophic security failure |
| D | Copying data creates a duplicate copy in an external account — data governance issues, extra cost, stale data risk |

⚠️ **Exam Trap:** IAM users with access keys are the "obvious" answer but are wrong for cross-account access. **Resource-based policies + IAM roles = AWS-recommended cross-account access pattern.**

---

## ❓ QUESTION 19
🟢 **Multiple Answer — Select THREE**

A solutions architect is reviewing a serverless application. The application uses API Gateway → Lambda → RDS PostgreSQL. During a load test, the team observes that as concurrent Lambda invocations increase, **database connections are exhausted**, causing errors. Which THREE solutions address this problem?

- **A.** Use Amazon RDS Proxy between Lambda and RDS to pool and reuse database connections
- **B.** Increase Lambda timeout to 15 minutes to allow connections to complete
- **C.** Store database connection objects outside the Lambda handler function for connection reuse across warm invocations
- **D.** Use Amazon Aurora Serverless v2 instead of RDS to scale connections automatically
- **E.** Switch from RDS to DynamoDB for all database operations
- **F.** Set Lambda Reserved Concurrency to limit the maximum number of concurrent invocations

---

### ✅ Answer: **A, C, and F**

### 📖 Explanation:

**The Problem:**
Each Lambda invocation creates a new database connection. RDS PostgreSQL has a connection limit based on instance size (e.g., db.t3.medium = ~170 connections). With 1,000 concurrent Lambdas = 1,000 connections attempted = RDS overwhelmed.

**Why A is correct:**
- **RDS Proxy** maintains a pool of database connections and multiplexes Lambda requests
- 1,000 Lambda invocations → RDS Proxy → manages 20-50 actual DB connections
- RDS Proxy supports IAM authentication and Secrets Manager for credentials
- **Best solution** for Lambda + RDS connection management

**Why C is correct:**
- Placing connection initialization **outside the handler** (in the global scope) allows connection reuse across **warm** Lambda invocations
- Cold start: creates connection, stores in global variable
- Subsequent warm invocations: reuses existing connection
- Reduces connection churn significantly

```python
# CORRECT — connection outside handler (reused across warm invocations)
import psycopg2
connection = psycopg2.connect(...)  # Created once per container

def lambda_handler(event, context):
    cursor = connection.cursor()  # Reuses existing connection
    ...
```

**Why F is correct:**
- Setting **Reserved Concurrency** caps the maximum simultaneous Lambda executions
- If RDS supports 100 connections, set Lambda concurrency to 80 (safety margin)
- Prevents RDS from being overwhelmed at the cost of some Lambda throttling

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| B | Increasing timeout doesn't reduce the NUMBER of concurrent connections |
| D | Aurora Serverless v2 scales compute but still has connection limits; doesn't solve the Lambda connection exhaustion pattern as elegantly as RDS Proxy |
| E | Switching to DynamoDB is a complete architecture change — valid in some cases but not a solution to the stated problem |

⚠️ **Exam Trap:** RDS Proxy is the SAA exam's preferred answer for Lambda + RDS connection exhaustion. This scenario appears very frequently. **Memorize: Lambda + RDS = always consider RDS Proxy.**

---

## ❓ QUESTION 20
🔵 **Single Answer**

A company runs a **containerized batch processing system** on Amazon ECS. The jobs are compute-intensive, run for 4-6 hours, and process large datasets. Cost is a primary concern. The workload can tolerate interruptions with checkpointing. What is the MOST cost-effective architecture?

- **A.** ECS with Fargate launch type using On-Demand pricing
- **B.** ECS with EC2 launch type using On-Demand instances
- **C.** ECS with EC2 launch type using Spot Instances with checkpointing and Spot interruption handling
- **D.** Amazon EKS with Fargate profiles for all batch jobs

---

### ✅ Answer: **C**

### 📖 Explanation:

**Why C is correct:**
- **EC2 Spot Instances** provide up to **90% discount** compared to On-Demand pricing
- The question explicitly states: "can tolerate interruptions with checkpointing"
- Checkpointing = saving job progress periodically so interrupted jobs resume from last checkpoint
- ECS on EC2 supports Spot Instances through **Spot capacity providers**
- **Spot Interruption Handling:** ECS receives a 2-minute warning before Spot termination → trigger checkpoint → graceful shutdown
- For 4-6 hour compute-intensive batch jobs, Spot savings are enormous

**Cost Comparison Example (c5.4xlarge, us-east-1):**
- On-Demand: ~$0.68/hour → 6 hours = $4.08 per job
- Spot: ~$0.20/hour → 6 hours = $1.20 per job
- **Savings: ~70%**

**Why others are wrong:**

| Option | Why Wrong |
|---|---|
| A | Fargate On-Demand is actually more expensive per hour than EC2 On-Demand for compute-intensive workloads; also doesn't support Spot pricing for the same cost savings |
| B | EC2 On-Demand is significantly more expensive than Spot for interruptible workloads |
| D | EKS Fargate doesn't support Spot Instances (Fargate pricing only) — more expensive for batch |

⚠️ **Exam Trap:** Fargate sounds like the "easier" answer but for **long-running, compute-intensive, cost-sensitive, interruptible batch workloads** — **ECS on EC2 Spot is almost always the most cost-effective answer.** Fargate Spot does exist but EC2 Spot provides greater discounts for heavy compute.

---

