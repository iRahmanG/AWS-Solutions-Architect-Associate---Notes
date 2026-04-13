
## Module 6: Computing (Database Services)

## Table of Contents
1. AWS Database Services Overview
2. SQL vs NoSQL Comparison
3. Amazon RDS (Relational Database Service)
4. Amazon Aurora
5. Amazon DynamoDB
6. AWS Database Migration Service (DMS)
7. Scenario-Based Questions & Answers
8. Exam Tips and Strategy

---

# Section 1: AWS Database Services Overview

## Key Concept: Understanding AWS Database Portfolio

AWS offers a broad array of managed database services, each purpose-built for specific use cases. The key to passing the certification is understanding **WHICH SERVICE TO CHOOSE FOR WHICH SCENARIO**.

### AWS Database Categories

<img width="1300" height="600" alt="image_f5b903f1" src="https://github.com/user-attachments/assets/ff5f0f3e-9724-4240-9369-0264dd7fc6b0" />


### Quick Reference: Database Selection

| If You Need | Product Type | Use This |
|---|---|---|
| Managed relational DB with 6 engine options | SQL | **Amazon RDS** |
| MySQL/PostgreSQL built for cloud | SQL | **Amazon Aurora** |
| Fully managed NoSQL database | NoSQL | **Amazon DynamoDB** |
| Managed graph database | NoSQL | **Amazon Neptune** |
| In-memory cache with Redis/Memcached | NoSQL Cache | **Amazon ElastiCache** |
| Data warehouse for analytics | SQL Analytics | **Amazon Redshift** |

---

### Best Use Cases Summary

| Service | Best For | Example Use Case |
|---|---|---|
| **Amazon RDS** | OLTP (Online Transaction Processing) | ERP, CRM, eCommerce transactions |
| **Amazon Aurora** | High-performance OLTP | Banks processing millions of transactions |
| **Amazon Redshift** | OLAP (Analytics) | Reporting on terabyte-scale data |
| **Amazon ElastiCache** | Real-time, low-latency | Gaming leaderboards, chat systems |
| **Amazon Neptune** | Highly connected data | Social networks, fraud detection |
| **Amazon DynamoDB** | Internet-scale, unstructured | Mobile apps, dating apps, IoT |

---

## Certification Focus Point
**The exam will test your ability to identify the RIGHT service from a scenario. Pay attention to keywords:**
- "Relational" → RDS/Aurora
- "Graph/Connected data" → Neptune
- "NoSQL/Unstructured" → DynamoDB
- "Real-time/Low-latency" → ElastiCache
- "Analytics/Reporting" → Redshift
- "Transactional" → RDS/Aurora

---

# Section 2: SQL vs NoSQL Databases

## Understanding the Fundamental Differences

### SQL Databases (Relational)

**Structure:** Data organized in rows and columns
**Schema:** Fixed schema - must define all columns before data entry
**Scaling:** Vertical scaling (increase hardware power)
**Query Language:** SQL
**ACID Compliance:** Strong - Atomicity, Consistency, Isolation, Durability

```
SQL Table Example:
┌──────────┬─────────────────┬──────────┬──────────┐
│ ISBN     │ Title           │ Author   │ Format   │
├──────────┼─────────────────┼──────────┼──────────┤
│ 9182932  │ Cloud Computing │ Wilson   │ Hardcover│
│ 3142536  │ Database Guru   │ Gomez    │ Ebook    │
└──────────┴─────────────────┴──────────┴──────────┘
```

**Pros:**
- Complex queries with JOINs
- Strong data consistency
- Well-established patterns

**Cons:**
- Harder to scale horizontally
- Less flexible for unstructured data
- Requires schema migrations

### NoSQL Databases (Non-Relational)

**Structure:** Key-value pairs, documents, graphs, or column families
**Schema:** Dynamic schema - add fields on the fly
**Scaling:** Horizontal scaling (add more servers)
**Query Language:** Document-focused queries (not traditional SQL)
**Consistency:** Eventually consistent (though some support strong consistency)

```
NoSQL Document Example (JSON):
{
    "ISBN": 9182932,
    "Title": "Cloud Computing Concepts",
    "Author": "Wilson, Joe",
    "Format": "Paperback",
    "CustomField": "Can add anytime"
}
```

**Pros:**
- Horizontal scalability
- Flexible schema
- High performance for specific access patterns

**Cons:**
- No JOINs (denormalization required)
- Eventually consistent by default
- Requires different query patterns

---

## Key Comparison Table

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Data Storage** | Rows & Columns | Key-Value, Documents |
| **Schema** | Fixed/Rigid | Dynamic/Flexible |
| **Querying** | Complex SQL queries | Collection of documents |
| **Scalability** | Vertical | Horizontal |
| **Consistency** | ACID Strong | Eventually Consistent |
| **Best For** | Structured data | Unstructured/Semi-structured |
| **Transaction Size** | Large complex transactions | Small, simple transactions |

---

## Certification Tip
**Remember:** The exam tests whether you know WHEN to use each paradigm.

**Choose SQL if:**
- Data is highly structured
- Complex joins are needed
- ACID compliance is critical (banking, healthcare)
- Data relationships are important

**Choose NoSQL if:**
- Data is unstructured/semi-structured
- High scalability needed
- Simple access patterns
- Flexible schema needed

---

# Section 3: Amazon RDS (Relational Database Service)

## What is Amazon RDS?

Amazon RDS is a **fully managed relational database service** that automates many operational tasks like patching, backup, recovery, and failover.

### Supported Database Engines (6 options)

1. **Amazon Aurora** (AWS proprietary - optimized for cloud)
2. **MySQL** (Open source)
3. **MariaDB** (MySQL fork)
4. **PostgreSQL** (Advanced open source)
5. **Oracle Database** (Commercial)
6. **Microsoft SQL Server** (Commercial)

## Key Components

### 1. DB Instances
- The basic building block of RDS
- Isolated database environment
- Can contain multiple user-created databases
- Accessible with standard database tools

### 2. DB Instance Classes
Determines CPU, memory, and network capacity:
- **Standard Classes:** General purpose, balanced compute/memory
- **Memory Optimized:** High memory for in-memory caching
- **Burstable Performance:** Low baseline with burst capability (cost-effective)

### 3. Storage Types
- **General Purpose SSD (gp2):** Balanced for most workloads
- **Provisioned IOPS SSD (io1):** For high I/O demanding applications
- **Magnetic:** Legacy, not recommended

---

## RDS Backup & Recovery

### Automated Backups

<img width="1300" height="600" alt="image_4bbaee47" src="https://github.com/user-attachments/assets/36da0888-3e4b-47f8-abc5-fa0ee4fb01b4" />

```
Timeline:
Day 1: Full backup taken
Day 2: Incremental snapshot (changes only)
Day 3: Incremental snapshot
...
├─ Stored for backup retention period (default 7 days, max 35)
├─ Automatic
└─ Transparent to application
```

### Manual Snapshots
- You can manually create snapshots anytime
- Retained until you explicitly delete them
- Useful before major changes

### AWS CLI Commands

**Create snapshot:**
```bash
aws rds create-db-snapshot --db-instance-identifier mytestdb --db-snapshot-identifier mydbsnapshot
```

**Restore from snapshot:**
```bash
aws rds restore-db-instance-from-db-snapshot --db-instance-identifier mytestdb-new --db-snapshot-identifier mydbsnapshot
```

**Copy snapshot (cross-region):**
```bash
aws rds copy-db-snapshot --source-db-snapshot-identifier mydbsnapshot --target-db-snapshot-identifier mydbsnapshot-copy --copy-tags
```

**Delete snapshot:**
```bash
aws rds delete-db-snapshot --db-snapshot-identifier mydbsnapshot
```

---

## High Availability: Multi-AZ Deployment

### What is Multi-AZ?

A failover mechanism that automatically creates a standby replica in a different Availability Zone.

### Architecture

<img width="1200" height="600" alt="image" src="https://github.com/user-attachments/assets/5090ee6e-4fa1-4ee5-871f-2f1911d77773" />


### Key Benefits of Multi-AZ

| Benefit | Description |
|---------|-------------|
| **Automatic Failover** | If primary fails, standby becomes primary automatically |
| **Zero Data Loss** | Synchronous replication ensures no data loss |
| **No Application Changes** | DNS endpoint stays the same |
| **Maintenance Windows** | Planned maintenance performed on standby first |
| **Durability** | Protection against Availability Zone failures |

### Certification Point: Failover Process
When the primary instance fails:
1. RDS detects failure
2. Standby becomes new primary
3. Applications automatically route to new primary via DNS
4. **No code changes required** (this is tested!)
5. Failover typically takes 1-2 minutes

---

## Scaling RDS

### Vertical Scaling (Scale Up)
```
Change Instance Class:
t2.micro → t2.small → t2.medium → m5.large → r5.xlarge
(Requires downtime - this is important!)
```

**When to use:** Need more CPU/memory, manageable downtime window

### Horizontal Scaling (Scale Out)
```
Primary Instance → Multiple Read Replicas
                ├─ Read Replica 1 (asynchronous)
                ├─ Read Replica 2 (asynchronous)
                └─ Read Replica 3 (asynchronous)
```

### Read Replicas

**What they are:** Asynchronously replicated copies of your database for read operations

**Key Characteristics:**
- Asynchronous replication (slight lag acceptable)
- Offload read traffic from primary
- Can be in different regions
- Can be promoted to standalone DB
- Adds slight replication lag

**Use Cases:**
- Analytics queries on production data
- Reporting without impacting primary
- Cross-region disaster recovery
- Dev/test environments

**Important:** Read replicas use **asynchronous** replication (unlike Multi-AZ which is **synchronous**)

---

## Certification Questions You'll See

**Question Type 1: Failure Scenario**
> "Your production RDS database has failed. What happens?"
> - Automatic failover to standby (if Multi-AZ enabled)
> - No code changes needed
> - DNS automatically updates

**Question Type 2: Performance Improvement**
> "You need to improve read performance. Which is best?"
> - Read replicas (for read-heavy workloads)
> - Not for write performance

**Question Type 3: Backup Strategy**
> "How long are automated backups retained?"
> - 1-35 days (default 7 days)
> - Manual snapshots retained indefinitely

---

# Section 4: Amazon Aurora

## What is Amazon Aurora?

**Aurora is AWS's flagship relational database** - a MySQL and PostgreSQL-compatible database built for the cloud with superior performance.

### Key Differentiators vs Standard RDS

| Feature | Aurora | Standard MySQL/PostgreSQL on RDS |
|---------|--------|--------------------------------|
| **Throughput** | 5x MySQL, 3x PostgreSQL | 1x (baseline) |
| **Scaling** | Automatic storage scaling | Manual management |
| **Availability** | 6 copies across 3 AZs | Single/Multi-AZ |
| **Read Replicas** | Up to 15 | Standard option |
| **Failover Time** | <30 seconds | 1-2 minutes |
| **Cost Model** | Pay-as-you-go per operation | Pay for instance + storage |

---

## Aurora Architecture

### Aurora DB Cluster Components

<img width="642" height="537" alt="image" src="https://github.com/user-attachments/assets/65266f26-2b1d-4c31-a5ad-67fa6499c25e" />


### Key Architecture Points

1. **Shared Cluster Volume**
   - Spans 3 Availability Zones
   - Stores 6 copies of data (automatic replication)
   - 10GB protection groups

2. **Primary Instance**
   - Single per cluster
   - Handles all write operations
   - Modifies cluster volume

3. **Aurora Replicas**
   - Up to 15 per cluster
   - Handle read-only operations
   - Can be promoted to primary
   - Automatic failover if primary fails

### High Availability Automatic

```
If Primary Instance Fails:
└─ Aurora automatically:
   ├─ Detects failure
   ├─ Promotes read replica to primary
   ├─ Updates DNS to new primary
   ├─ Completes in <30 seconds
   └─ NO DATA LOSS (shared storage)
```

---

## Aurora Advantages for Certification

**Know these for the exam:**

1. **Performance:** 5x standard MySQL, 3x PostgreSQL
2. **Automatic Storage Scaling:** Grows as needed
3. **High Availability Built-in:** 6 copies across 3 AZs
4. **Fast Failover:** <30 seconds
5. **Read Scaling:** Up to 15 read replicas
6. **MySQL/PostgreSQL Compatible:** Drop-in replacement
7. **Serverless Option:** No instance management

---

## Aurora vs Standard RDS Use Cases

### Use Aurora When:
- High throughput needed
- Requires 99.99% uptime
- Complex queries with high concurrency
- Want automatic failover (<30s)
- International expansion (cross-region read replicas)
- Cost-effective at scale (pay per operation)

### Use Standard RDS When:
- Simple, predictable workloads
- Cost-sensitive (lower overhead)
- Specific database engine needed (not compatible)
- Simple backup/restore sufficient

---

# Section 5: Amazon DynamoDB

## What is Amazon DynamoDB?

DynamoDB is a **fully managed, serverless NoSQL database** offering:
- High scalability
- Single-digit millisecond latency
- Flexible schema
- Document and key-value support

---

## When to Use DynamoDB - Key Indicators

```
Use DynamoDB if your application:
✓ Serves global users with low-latency requirements
✓ Has unstructured/semi-structured data
✓ Needs horizontal scaling
✓ High request rates (thousands/second)
✓ Don't need complex joins
✓ Mobile/IoT applications
✓ Real-time analytics

Don't use if:
✗ Complex transactions across multiple tables
✗ Need complex JOINs
✗ ACID compliance critical
✗ Complex reporting queries
```

---

## DynamoDB Data Model

### Core Concepts

```
┌──────────────────────────────────────────────────────┐
│                  TABLE                               │
│                                                      │
│  ┌────────────────────────────────────────────────┐ │
│  │  ITEM 1 (like a row)                           │ │
│  │  ┌──────────────────────────────────────────┐  │ │
│  │  │ Partition Key: UserID (required)         │  │ │
│  │  │ Sort Key: OrderID (optional)             │  │ │
│  │  │ Attribute 1: Name = "John"               │  │ │
│  │  │ Attribute 2: Email = "john@example.com"  │  │ │
│  │  │ Attribute 3: CustomField = "Any value"   │  │ │
│  │  └──────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────┐ │
│  │  ITEM 2                                        │ │
│  │  ┌──────────────────────────────────────────┐  │ │
│  │  │ Partition Key: UserID2                   │  │ │
│  │  │ Sort Key: OrderID2                       │  │ │
│  │  │ Attribute 1: Name = "Jane"               │  │ │
│  │  └──────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────┘
```

### Primary Key Types

#### 1. Simple Primary Key (Partition Key Only)
```
Table: Users
Partition Key: UserID
├─ UserID = "user1" → Points to storage partition
├─ UserID = "user2" → Points to different partition
└─ UserID = "user3" → Points to another partition

Benefit: Distributes data evenly
```

#### 2. Composite Primary Key (Partition + Sort Key)
```
Table: Orders
├─ Partition Key: UserID (determines which partition)
└─ Sort Key: OrderDate (determines order within partition)

Example:
UserID = "user1", OrderDate = "2024-01-15" → Item 1
UserID = "user1", OrderDate = "2024-02-20" → Item 2
UserID = "user2", OrderDate = "2024-01-10" → Item 3

Benefit: Query all orders by user, sorted by date
```

### Key Characteristics

| Aspect | Description |
|--------|-------------|
| **Partition Key** | Required; determines data distribution |
| **Sort Key** | Optional; enables range queries |
| **Attributes** | Schema-less; add any field anytime |
| **Item Size** | Max 400KB per item |
| **Throughput** | Provisioned or on-demand |

---

## DynamoDB Global Tables

### Purpose
Provide multi-region availability and automatic replication

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│           DYNAMODB GLOBAL TABLE                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌────────────────┐    ┌────────────────┐                  │
│  │   us-east-1    │    │  eu-central-1  │                  │
│  │  (Replica)     │    │  (Replica)     │                  │
│  │                │    │                │                  │
│  │ Same data set  │◄──►│ Same data set  │                  │
│  │ Automatically  │    │ Automatically  │                  │
│  │ replicated     │    │ replicated     │                  │
│  └────────────────┘    └────────────────┘                  │
│          │                      │                           │
│          └──────────┬───────────┘                           │
│                     │                                       │
│             ┌───────▼────────┐                             │
│             │ ap-southeast-2 │                             │
│             │  (Replica)     │                             │
│             └────────────────┘                             │
└─────────────────────────────────────────────────────────────┘
```

### Global Tables Benefits
- **Low Latency:** Users access nearest replica
- **High Availability:** If one region down, others active
- **Active-Active:** Writes to any region
- **Automatic Replication:** No manual sync needed
- **Disaster Recovery:** Complete data in all regions

---

## DynamoDB vs RDS

### Decision Matrix

| Use DynamoDB if...              | Use RDS if...                  |
|---------------------------------|--------------------------------|
| ✓ Flexible schema               | ✓ Fixed schema needed          |
| ✓ High horizontal scaling       | ✓ Vertical scaling sufficient  |
| ✓ Document/JSON data            | ✓ Structured relational data   |
| ✓ Simple access patterns        | ✓ Complex queries/JOINs        |
| ✓ Real-time apps                | ✓ Analytics/Reporting          |
| ✓ Mobile/IoT                    | ✓ Legacy applications          |
| ✓ Eventually consistent ok      | ✓ ACID compliance critical     |

---

## Certification Highlights

**DynamoDB Exam Topics:**
1. When to use (no complex joins, unstructured data)
2. Partition key design (even distribution important)
3. Global tables (multi-region)
4. Primary key types (simple vs composite)
5. Item size limits (400KB)
6. Throughput modes (provisioned vs on-demand)

---

# Section 6: AWS Database Migration Service (DMS)

## Purpose
Migrate databases from on-premises or other clouds to AWS with **minimal downtime**.

## Key Features

| **AWS DATABASE MIGRATION SERVICE (DMS)** | Description |
|------------------------------------------|--------------|
| ✓ Zero downtime migration                | Migrate databases without interrupting applications |
| ✓ Homogenous migrations (Oracle → Oracle) | Move between same database engines easily |
| ✓ Heterogenous migrations (Oracle → MySQL) | Convert between different database engines |
| ✓ Continuous replication                 | Keep source and target databases synchronized |
| ✓ Supports most commercial & open-source DBs | Works with Oracle, MySQL, PostgreSQL, SQL Server, etc. |
| ✓ Keep applications running during migration | Ensures minimal disruption to end users |


---

## DMS Architecture

### Components

```
┌──────────────────────────────────────────────────────────────────┐
│                     AWS DMS SETUP                                │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌────────────────────┐                ┌──────────────────────┐ │
│  │ SOURCE DATABASE    │                │  TARGET DATABASE     │ │
│  │                    │                │                      │ │
│  │ ├─ On-Premises DB  │                │  ├─ Amazon RDS       │ │
│  │ ├─ Oracle          │                │  ├─ Amazon Aurora    │ │
│  │ ├─ MySQL           │                │  ├─ Amazon DynamoDB  │ │
│  │ ├─ PostgreSQL      │                │  ├─ Amazon Redshift  │ │
│  │ └─ SQL Server      │                │  ├─ Amazon S3        │ │
│  └─────────┬──────────┘                │  └─ Databases on EC2 │ │
│            │                           └──────────────────────┘ │
│            │                                      ▲               │
│            │                                      │               │
│            │         ┌──────────────────┐        │               │
│            │         │ REPLICATION      │        │               │
│            ├────────►│ INSTANCE         ├────────┘               │
│            │         │ (EC2 in VPC)     │                       │
│            │         │                  │                       │
│            │         │ ┌──────────────┐ │                       │
│            │         │ │ TASK         │ │                       │
│            │         │ │ (Performs    │ │                       │
│            │         │ │  migration)  │ │                       │
│            │         │ └──────────────┘ │                       │
│            │         └──────────────────┘                       │
│            │                                                    │
│            └─────── One endpoint must be AWS                   │
│                                                                │
└──────────────────────────────────────────────────────────────────┘
```

### Four Key Components

1. **Replication Instance**
   - Amazon EC2 instance running DMS
   - High availability via Multi-AZ
   - Storage encryption available
   - Performs actual migration work

2. **Task**
   - Actual migration process
   - Runs on replication instance
   - Can configure:
     - Migrate existing data only
     - Migrate + replicate ongoing changes
     - Replicate changes only

3. **Source Endpoint**
   - Connection information to source database
   - On-premises, RDS, EC2, Azure SQL, S3

4. **Target Endpoint**
   - AWS endpoint where data goes
   - RDS, Aurora, DynamoDB, Redshift, S3, EC2

---

## Supported Source/Target Combinations

| **SOURCE / TARGET** | **On-Premises**     | **AWS Services**     | **EC2**          |
|----------------------|--------------------|----------------------|------------------|
| **On-Premises**      | ❌ Not Supported    | ✅ Supported          | ✅ Supported      |
| **Amazon RDS / S3**  | ✅ Supported        | ✅ Supported          | ✅ Supported      |
| **Amazon EC2**       | ✅ Supported        | ✅ Supported          | ✅ Supported      |
| **Azure SQL**        | ❌ Not Supported    | ✅ Supported          | ✅ Supported      |

> **Key Rule:** At least **one endpoint must be on AWS**

---

## DMS Capabilities

### Homogenous Migrations

| **Migration Type**     | **Source → Target** | **DMS Handles** |
|-------------------------|--------------------|-----------------|
| Oracle → Oracle         | Homogeneous        | ✓ Full data migration<br>✓ Schema transfer (minimal)<br>✓ Ongoing replication |
| MySQL → MySQL           | Homogeneous        | ✓ Full data migration<br>✓ Schema transfer (minimal)<br>✓ Ongoing replication |
| PostgreSQL → PostgreSQL | Homogeneous        | ✓ Full data migration<br>✓ Schema transfer (minimal)<br>✓ Ongoing replication |

### Heterogenous Migrations

| **Migration Type**     | **Source → Target** | **Requires** |
|-------------------------|--------------------|--------------|
| Oracle → MySQL          | Heterogeneous      | ✓ Schema conversion + Data migration |
| MySQL → PostgreSQL      | Heterogeneous      | ✓ Schema conversion + Data migration |
| SQL Server → Aurora     | Heterogeneous      | ✓ Schema conversion + Data migration |

---

## AWS Schema Conversion Tool (AWS SCT)

### Purpose
Convert database schema and code from source to target database format

### Two-Step Migration Process

```
┌─────────────────────────────────────────────────────────────┐
│           DATABASE MIGRATION PROCESS                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  STEP 1: Schema Conversion                                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ AWS Schema Conversion Tool (AWS SCT)                 │  │
│  │                                                      │  │
│  │ Input: Source database schema                        │  │
│  │ ├─ Tables structure                                 │  │
│  │ ├─ Views                                            │  │
│  │ ├─ Stored procedures                                │  │
│  │ ├─ Functions                                        │  │
│  │ └─ Application SQL                                  │  │
│  │                                                      │  │
│  │ Output: Target-compatible schema                    │  │
│  │ (with manual conversion notes)                      │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                 │
│  STEP 2: Data Migration                                    │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ AWS Database Migration Service (AWS DMS)             │  │
│  │                                                      │  │
│  │ ├─ Full load (all existing data)                    │  │
│  │ ├─ Change data capture (ongoing changes)            │  │
│  │ └─ Minimal downtime (applications keep running)     │  │
│  │                                                      │  │
│  │ Result: Data in target database                     │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                 │
│  Application Switchover                                    │
│  └─ Applications now use target database                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### AWS SCT Conversions Supported

| Source | Target Options |
|--------|-----------------|
| Oracle | Aurora, MySQL, PostgreSQL, MariaDB, Redshift |
| SQL Server | Aurora, Redshift, MySQL, PostgreSQL, MariaDB |
| MySQL/MariaDB | PostgreSQL |
| PostgreSQL | Aurora, MySQL, MariaDB |
| Teradata | Redshift |
| Apache Cassandra | DynamoDB |

### What SCT Converts

- ✓ Database schema
- ✓ Views
- ✓ Stored procedures
- ✓ Functions
- ✓ Application SQL
  
> ⚠ Some database-specific code (marked for manual review)

---

## DMS Best Practices for Certification

**Know These:**
1. One endpoint must always be on AWS
2. Schema conversion is separate from data migration
3. Multi-AZ replication instance for production
4. Can migrate between databases continuously
5. Use SCT first, then DMS
6. Zero-downtime possible with CDC (Change Data Capture)

---

# Section 7: Scenario-Based Questions & Answers

## Scenario 1: Financial Services - Transaction Processing

**Scenario:**
A major bank needs to process millions of credit card transactions per day. They require:
- ACID compliance (no data loss)
- Complex queries with JOINs
- Multi-region redundancy
- <100ms query response time
- Automatic failover capability

**Questions:**

**Q1: Which database service is most appropriate?**

A) Amazon DynamoDB
B) Amazon Aurora
C) Amazon ElastiCache
D) Amazon Redshift

**Answer: B) Amazon Aurora**

**Explanation:**
- ACID compliance → Rules out DynamoDB
- Complex queries with JOINs → Rules out DynamoDB
- High throughput needed → Aurora's 5x MySQL throughput
- Multi-region → Can use Aurora global read replicas
- Automatic failover → Built-in with <30s failover
- <100ms latency → Aurora provides this

ElastiCache is just for caching, Redshift is for analytics, DynamoDB doesn't support ACID/JOINs.

---

**Q2: How should they achieve multi-region redundancy?**

A) Multi-AZ deployment in single region
B) Aurora global read replicas across regions
C) DynamoDB global tables
D) Cross-region RDS read replicas

**Answer: B) Aurora global read replicas across regions**

**Explanation:**
- Aurora supports up to 15 read replicas
- Can span multiple regions
- Automatic failover if primary region fails
- Read traffic can be distributed

Multi-AZ is single region only, DynamoDB global tables are overkill for transactional workload, RDS read replicas are asynchronous (not ideal for primary backup).

---

**Q3: The bank wants automatic failover in <1 minute. What's the minimum configuration?**

A) Single RDS instance
B) RDS Multi-AZ
C) RDS with read replicas only
D) DynamoDB with global tables

**Answer: B) RDS Multi-AZ**

**Explanation:**
- Multi-AZ provides automatic failover
- Typical failover time: 1-2 minutes
- Synchronous replication = no data loss
- Works with any RDS engine (including Aurora)

Aurora could also work (even faster <30s), but Multi-AZ is the minimum required.

---

## Scenario 2: Mobile App - User Profiles & Activities

**Scenario:**
A dating app needs to:
- Store flexible user profiles (varying data)
- Handle millions of users worldwide
- Serve requests in <50ms
- Scale automatically
- Support real-time notifications

**Question:**

**Q1: Which database should handle user profile storage?**

A) Amazon RDS (MySQL)
B) Amazon Aurora
C) Amazon DynamoDB
D) Amazon Redshift

**Answer: C) Amazon DynamoDB**

**Explanation:**

Keyword analysis:
- "Flexible" profiles → Dynamic schema → DynamoDB
- "Millions of users" → Horizontal scaling → DynamoDB
- "<50ms latency" → DynamoDB provides single-digit milliseconds
- "Scale automatically" → DynamoDB auto-scales
- "Worldwide" → Can use global tables

Why not others:
- RDS/Aurora → Fixed schema not suitable for varying profiles
- Redshift → For analytics, not real-time transactions

---

**Q2: For the location-based matching feature (finding nearby users), what scaling approach?**

A) Increase RDS instance class (vertical)
B) Add RDS read replicas
C) Use DynamoDB global tables with local replicas
D) Partition DynamoDB tables by region

**Answer: D) Partition DynamoDB tables by region**

**Explanation:**
- Location-based queries need regional data
- Partitioning by region provides locality
- Users in US query US data (low latency)
- Users in EU query EU data (low latency)
- DynamoDB auto-scales within region

Global tables would replicate ALL data everywhere (expensive and high latency).

---

## Scenario 3: Data Migration - Legacy System

**Scenario:**
A company is migrating from on-premises Oracle database (500GB) to AWS. They need:
- Minimal downtime (critical business app)
- Heterogenous migration (Oracle → PostgreSQL for cost savings)
- Preserve all stored procedures and triggers
- Ongoing replication for 2 weeks post-migration

**Questions:**

**Q1: What's the correct sequence?**

A) AWS DMS → AWS SCT → Switchover
B) AWS SCT → AWS DMS → Switchover
C) AWS DMS → AWS DMS → No SCT needed
D) Manual export → Manual import → Switchover

**Answer: B) AWS SCT → AWS DMS → Switchover**

**Explanation:**
- **Step 1 (SCT):** Convert Oracle schema to PostgreSQL
  - Converts tables, views, stored procedures
  - Identifies code needing manual review
  - Creates target schema in AWS
- **Step 2 (DMS):** Migrate data
  - Full load initial 500GB
  - CDC (Change Data Capture) for ongoing changes
  - Keeps app running during migration
- **Step 3:** Verify and switchover

Wrong answers:
- DMS first → No schema to receive data
- No SCT → Oracle PL/SQL won't work on PostgreSQL
- Manual → Too error-prone and slow

---

**Q2: During migration, the application needs to keep running. What's DMS configured for?**

A) Full load only
B) Full load + CDC (ongoing replication)
C) CDC only
D) Snapshot only

**Answer: B) Full load + CDC (ongoing replication)**

**Explanation:**
- Full load → Migrate all 500GB historical data
- CDC → Capture ongoing changes from Oracle
- Application continues writing to Oracle
- Changes replicated to PostgreSQL in near-real-time
- After 2 weeks testing, application switches to PostgreSQL

This allows zero-downtime migration.

---

**Q3: What's the key constraint for AWS DMS endpoints?**

A) Both endpoints must be in same region
B) Both endpoints must be RDS
C) At least one endpoint must be on AWS
D) Source must be on-premises

**Answer: C) At least one endpoint must be on AWS**

**Explanation:**
- Your scenario: Oracle (on-prem) → PostgreSQL RDS (AWS) ✓
- DMS won't support: On-prem Oracle → On-prem PostgreSQL ✗
- This is a design decision by AWS to incentivize AWS adoption

---

## Scenario 4: Analytics & Reporting - E-commerce

**Scenario:**
An e-commerce platform has:
- Production transactional database (100GB, growing 5GB/month)
- Needs to run analytics reports without impacting production
- Reports query terabyte-scale historical data
- Executives need dashboards refreshed daily
- Budget conscious

**Questions:**

**Q1: What's the best approach for analytics?**

A) Single RDS instance for transactions AND analytics
B) RDS read replica for analytics
C) Amazon Redshift for analytics
D) Amazon DynamoDB for everything

**Answer: B) RDS read replica for analytics (if small scale) or C) Redshift (if larger scale)**

**Explanation:**
- Production DB (100GB) → Primary RDS instance
- Analytics queries → Read replica
- Protects production from heavy reporting

However, if terabyte-scale data mentioned:
- Read replica approach breaks down
- Redshift better for OLAP (analytics)
- Redshift optimized for columnar queries
- Better cost for analytics-heavy workloads

---

**Q2: Why not just add read replicas indefinitely?**

A) Cost (each replica costs money)
B) Replication lag (async replication delays)
C) Limited to 5 per instance
D) Replicas can only read, can't optimize for analytics

**Answer: A) Cost and D) Not optimized**

**Explanation:**
- Each read replica = additional EC2-like costs
- With terabyte-scale, cost becomes prohibitive
- Read replicas optimized for transactional reads
- Redshift optimized for analytical aggregations
- Redshift >> faster for aggregation queries

---

## Scenario 5: Real-time Leaderboard - Gaming

**Scenario:**
A multiplayer game needs:
- Real-time player rankings (milliseconds)
- Updates thousands/second
- Leaderboards by region
- Persistent storage of scores
- High availability

**Question:**

**Q1: What's the best combination?**

A) DynamoDB for persistent storage + ElastiCache for real-time
B) ElastiCache only
C) Aurora for both
D) Redshift for both

**Answer: A) DynamoDB for persistent storage + ElastiCache for real-time**

**Explanation:**
- **ElastiCache (Redis):**
  - In-memory = single-digit millisecond latency
  - Built for leaderboard use case
  - Supports rankings/sorted sets
  - Can handle thousands of updates/second
  
- **DynamoDB:**
  - Persistent storage backup
  - If ElastiCache fails, data still there
  - Can rebuild cache from DynamoDB
  - Global tables for regional leaderboards

Why not just DynamoDB alone?
- While fast, ElastiCache is faster for this pattern
- Redis sorted sets perfect for leaderboards

---

# Section 8: Exam Tips and Strategy

##  AWS Database Keyword Mapping

| **Keyword / Context**        | **Suggested AWS Service**                | **Notes / Use Case** |
|-------------------------------|------------------------------------------|-----------------------|
| **Graph**                     | Amazon Neptune                          | Purpose-built graph database |
| **Highly connected**          | Amazon Neptune                          | Ideal for relationship-heavy data |
| **Connected data**            | Amazon Neptune                          | Great for social networks or knowledge graphs |
| **Fraud detection**           | Amazon Neptune                          | Detects graph patterns for anomalies |
| **Real-time**                 | DynamoDB or ElastiCache                 | Low-latency data access |
| **Millisecond latency**       | ElastiCache or DynamoDB                 | In-memory caching or fast NoSQL |
| **Gaming / Leaderboard**      | ElastiCache                             | Real-time score updates |
| **Flexible schema**           | DynamoDB                                | NoSQL, schema-less design |
| **NoSQL**                     | DynamoDB                                | Key-value and document store |
| **Unstructured**              | DynamoDB                                | Handles JSON and dynamic attributes |
| **Mobile app**                | DynamoDB                                | Scalable backend for mobile workloads |
| **IoT**                       | DynamoDB                                | High throughput for device data |
| **ACID compliance**           | RDS or Aurora (NOT DynamoDB)            | Transactional consistency |
| **Complex JOINs**             | RDS or Aurora (NOT DynamoDB)            | Relational query support |
| **ERP / CRM**                 | RDS                                     | Traditional enterprise systems |
| **Transactions**              | RDS or Aurora                           | Multi-statement atomic operations |
| **High throughput**           | Aurora (if SQL) or DynamoDB             | Scalable performance |
| **5x performance**            | Aurora                                  | Optimized for high-speed SQL workloads |
| **Automatic failover**        | Aurora or RDS Multi-AZ                  | High availability setup |
| **Analytics**                 | Redshift                                | Data warehousing and BI |
| **Terabyte-scale**            | Redshift                                | Handles massive datasets |
| **Data warehouse**            | Redshift                                | Columnar storage for analytics |
| **Reporting**                 | Redshift (if large scale)               | Aggregation and visualization workloads |
| **Multi-region**              | Aurora Global Replicas or DynamoDB Global Tables | Cross-region replication |
| **Disaster recovery**         | RDS Multi-AZ or Aurora                  | Automatic failover and backup |
---

## Elimination Strategy

When you see a question, eliminate systematically:

```
1. IDENTIFY WORKLOAD TYPE
   └─ Transactional? → RDS/Aurora
   └─ Analytics? → Redshift
   └─ Real-time/cache? → ElastiCache
   └─ NoSQL/unstructured? → DynamoDB
   └─ Graph/connected? → Neptune

2. CHECK CONSISTENCY REQUIREMENTS
   └─ ACID critical? → RDS/Aurora (eliminate DynamoDB)
   └─ Eventually consistent ok? → DynamoDB

3. CHECK QUERY PATTERNS
   └─ Complex joins? → RDS/Aurora (eliminate DynamoDB)
   └─ Simple access patterns? → DynamoDB

4. CHECK SCALE REQUIREMENTS
   └─ Millions of concurrent users? → DynamoDB
   └─ Transactional at scale? → Aurora
   └─ Analytics at scale? → Redshift

5. CHECK FAILURE/AVAILABILITY
   └─ Multi-region? → Global tables/replicas
   └─ Automatic failover? → Multi-AZ or Aurora
```

---

## Common Trap Answers

**Trap 1: Choosing RDS when Aurora is better**
```
Question: "Need 5x throughput with automatic failover"
Trap: RDS (you don't get 5x)
Right: Aurora (built-in 5x + <30s failover)
```

**Trap 2: Choosing DynamoDB when RDS needed**
```
Question: "Bank needs ACID compliance"
Trap: DynamoDB (doesn't support ACID)
Right: RDS or Aurora
```

**Trap 3: Choosing RDS when DynamoDB needed**
```
Question: "Mobile app with flexible user profiles"
Trap: RDS (fixed schema)
Right: DynamoDB (dynamic schema)
```

**Trap 4: Forgetting DMS requirements**
```
Question: "Migrate from on-prem Oracle to PostgreSQL"
Trap: Just use DMS (fails on schema)
Right: SCT first (convert schema), then DMS (migrate data)
```

---

## Question Types You'll See

### Type 1: Service Selection
> "Which AWS database is best for..."

**Strategy:**
- Identify workload type
- Check for key requirements (ACID, scalability, latency)
- Eliminate unsuitable options
- Choose the purpose-built service

### Type 2: Architecture Design
> "How would you design..."

**Strategy:**
- Identify primary service
- Consider high availability (Multi-AZ, global, failover)
- Consider scaling strategy (vertical, horizontal, replicas)
- Consider consistency (sync vs async replication)

### Type 3: Migration Scenario
> "How to migrate with minimal downtime..."

**Strategy:**
- Identify source/target types
- Check DMS compatibility (at least one AWS endpoint)
- Remember: SCT for schema, DMS for data
- Remember: Full load + CDC for ongoing changes

### Type 4: Performance Optimization
> "How to improve performance of..."

**Strategy:**
- For read-heavy → add read replicas
- For write-heavy → scaling or partitioning
- For real-time → caching with ElastiCache
- For analytics → Redshift or DMS to Redshift

### Type 5: Failure/Recovery
> "What happens if primary fails..."

**Strategy:**
- Multi-AZ → automatic failover to standby
- Replicas → can be promoted (manual)
- Snapshots → point-in-time recovery
- Aurora → fastest failover <30s

---

## Before Your Exam

**Review These Concepts:**
- [ ] RDS 6 engine options
- [ ] RDS Multi-AZ vs read replicas
- [ ] Aurora high availability (6 copies, 3 AZs)
- [ ] DynamoDB partition key + sort key
- [ ] DynamoDB vs RDS decision criteria
- [ ] DMS two-step process (SCT + DMS)
- [ ] AWS SCT conversions supported
- [ ] ElastiCache use cases
- [ ] Neptune use cases
- [ ] Redshift use cases

**Do These Practice Questions:**
- Identify 5 scenarios where you'd use each service
- Create a service selection flowchart
- Design migration path for 3 different scenarios
- Answer 20+ scenario questions (below)

---

# Section 9: Additional Practice Scenarios

## Practice Scenario 1: Real-time Social Media Feed

**Company:** Social media platform with 500M users
**Requirements:**
- Real-time feed updates
- Like/comment counts update instantly
- User followers relationship tracking
- Find mutual friends (graph traversal)
- Sub-second response time

**Question:**
Which combination of services is best?

A) RDS for everything
B) DynamoDB for feeds + Neptune for relationships
C) ElastiCache only
D) Redshift for everything

**Your Answer:** ___________

**Explanation:**
DynamoDB for:
- Storing feed items (flexible schema)
- Handling millions of updates/second
- Low-latency reads

Neptune for:
- Following relationships (who follows whom)
- Friend finding (mutual connections)
- Graph pattern queries

ElastiCache could be added for caching hot data, but Neptune is essential for relationship traversal.

---

## Practice Scenario 2: Financial Reporting

**Company:** Investment bank
**Requirements:**
- Process billions of trades daily
- Run complex reports on historical data
- Support thousands of concurrent queries
- Data volumes: 50TB and growing
- Aggregate and analyze by securities, traders, time periods

**Question:**
What's the recommended architecture?

A) Single Aurora instance
B) Aurora for transactions + Redshift for analytics
C) Aurora for transactions + Aurora for analytics (with replicas)
D) DynamoDB for everything

**Your Answer:** ___________

**Explanation:**
Aurora for:
- Real-time transaction processing
- ACID compliance
- Handling complex order books

Redshift for:
- Analytical queries on 50TB historical data
- Complex aggregations
- Thousands of concurrent analytical queries
- Much more cost-effective than Aurora for OLAP

---

## Practice Scenario 3: Retail Inventory System

**Company:** Online retailer with stores in 15 countries
**Requirements:**
- Inventory counts updated in real-time
- Global visibility of stock levels
- <100ms response time globally
- Automatic synchronization across regions
- Handle peak traffic (Black Friday: 100K requests/second)

**Question:**
Recommended database?

A) RDS Multi-AZ in each region
B) DynamoDB with global tables
C) Aurora with read replicas
D) ElastiCache only

**Your Answer:** ___________

**Explanation:**
DynamoDB with global tables:
- Inventory is simple data (item_id, quantity)
- Flexible schema perfect for different product types
- Automatic replication to all regions
- Single-digit millisecond latency everywhere
- Handles 100K requests/second easily
- No complex joins needed

Aurora could work but more overhead. RDS Multi-AZ wouldn't provide same global replication.

---

## Practice Scenario 4: Data Integration Project

**Company:** Pharmaceutical company
**Current State:** Multiple databases (SQL Server, Teradata, Oracle)
**Target State:** Consolidated analytics in AWS
**Constraints:** Can't shut down systems, must complete in 3 months

**Question:**
Which tools in which order?

A) DMS for each database to S3
B) Manual export/import with validation
C) AWS SCT for each source + DMS to Redshift
D) EC2 instances running replicas of each database

**Your Answer:** ___________

**Explanation:**
AWS SCT:
- Converts SQL Server schema to Redshift-compatible
- Converts Teradata schema to Redshift-compatible
- Converts Oracle schema to Redshift-compatible
- Identifies code needing manual conversion

AWS DMS:
- Migrates data from each source to Redshift
- Supports SQL Server, Teradata, Oracle → Redshift
- One endpoint always AWS ✓
- Minimal downtime ✓
- Can run CDC for ongoing updates ✓

This approach:
- Keeps source systems running ✓
- Minimal disruption ✓
- 3-month timeline achievable ✓
- Consolidates to single Redshift ✓

---

## Practice Scenario 5: E-learning Platform

**Company:** Online course platform
**Requirements:**
- Store flexible student data (enrollment, certificates, preferences)
- Personalized recommendations
- Handle millions of concurrent learners
- Serve video metadata globally
- Real-time progress updates

**Question:**
Best architecture for student profiles?

A) RDS with complex schema
B) Aurora Serverless
C) DynamoDB
D) Redshift

**Your Answer:** ___________

**Explanation:**
DynamoDB:
- Student profiles vary (different courses, different progress)
- Dynamic schema perfect for flexible profiles
- Millions of concurrent learners
- Single-digit millisecond response time
- Global tables for video metadata access

Why not RDS?
- Fixed schema won't accommodate diverse student data
- Harder to scale horizontally to millions

Why not Redshift?
- Not for transactional workload

Why not Aurora?
- Could work, but DynamoDB is purpose-built for this

---

# Summary: Quick Reference Table

## Service Quick Reference

| Service | Best For | Scale | Consistency | Example |
|---------|----------|-------|-------------|---------|
| **RDS** | OLTP transactions with multiple engines | Vertical | ACID Strong | Banking |
| **Aurora** | High-throughput OLTP with failover | Horizontal | ACID Strong | Stock trading |
| **DynamoDB** | NoSQL, flexible schema, massive scale | Horizontal | Eventually consistent | Mobile apps |
| **Neptune** | Graph relationships, connected data | Vertical | ACID Strong | Social network |
| **ElastiCache** | Real-time caching, sub-millisecond | Vertical | Eventually consistent | Leaderboards |
| **Redshift** | OLAP, analytics, terabyte-scale | Horizontal | Eventually consistent | Analytics |
| **DMS** | Migrate databases, minimal downtime | N/A | Maintains source | Migration |
| **SCT** | Convert database schemas | N/A | N/A | Oracle→MySQL |

---

## Exam Day Checklist

- ✓ Remember RDS 6 engines (Aurora, MySQL, MariaDB, PostgreSQL, Oracle, SQL Server)
- ✓ Know Aurora is 5x MySQL, 3x PostgreSQL
- ✓ Understand Multi-AZ = automatic failover (1-2 min)
- ✓ Know read replicas are asynchronous
- ✓ Remember DynamoDB partition key distributes data
- ✓ Know DynamoDB global tables for multi-region
- ✓ Remember SCT before DMS for heterogenous migrations
- ✓ Know at least one DMS endpoint must be AWS
- ✓ Understand Neptune for graph/connected data
- ✓ Know ElastiCache for real-time/caching
- ✓ Know Redshift for large-scale analytics

---

## Final Tips

1. **Read questions carefully** - look for keywords indicating service type
2. **Eliminate obviously wrong answers** - DynamoDB for ACID needs? No.
3. **Consider the full architecture** - not just one service
4. **Think about scaling approach** - vertical vs horizontal
5. **Consider high availability** - Multi-AZ, failover, replicas
6. **Watch for migration questions** - SCT + DMS sequence
7. **Focus on use cases** - each service has a primary purpose
8. **Practice with real scenarios** - abstract scenarios are rare

---

## Good Luck!

Remember: AWS certification tests your ability to **choose the right service for the right problem**. Master the service selection matrix and you'll ace this section!
