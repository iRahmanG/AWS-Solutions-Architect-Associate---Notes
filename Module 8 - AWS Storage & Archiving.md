## 🗄️ AWS Storage & Archiving — SAA-C03 Deep Dive

This is one of the **most heavily tested** topics. Let's start with the big picture of all storage services.---
<img width="1440" height="890" alt="image" src="https://github.com/user-attachments/assets/1b8d62c9-8c0d-49e3-9b13-3ee9bcf12719" />


## 1. Amazon EBS (Elastic Block Store)

Think of EBS like a **USB drive in the cloud** — it's a network-attached disk you plug into an EC2 instance. It's persistent, meaning data survives even after the instance is stopped.

**Key properties:**
- Exists in a single **Availability Zone** (you must create it in the same AZ as your instance)
- Provides **block-level** storage (not files, not objects)
- Supports **boot volumes and data volumes**
- Can be **EBS-optimized** — a dedicated network link between instance and volume for consistent I/O

Here's how the four EBS volume types map to use cases — this is a common exam differentiator:
<img width="1440" height="636" alt="image" src="https://github.com/user-attachments/assets/b380f877-d79a-4b51-9f4c-1b0436500197" />

### EBS Snapshots — How they work

Snapshots backup your EBS volume to S3 (but you can't access them directly via S3 — AWS manages them). The critical concept is **incremental snapshots**:

- Snapshot 1 = full copy of all blocks (A, B, C)
- Snapshot 2 = only new/changed blocks (D, E, F) — but logically contains all of A–F
- Snapshot 3 = only changed blocks (G, H) — logically replaces A, B with G, H

This means **deleting any one snapshot doesn't break the others** — AWS handles the delta logic behind the scenes.

**Lifecycle Management (DLM):** You can automate snapshot creation, retention, and deletion using Amazon Data Lifecycle Manager. You tag your EBS volumes, then create a policy that says "take a snapshot every 24 hours, keep the last 5." Saves you from manual management and keeps costs down.

**Restoring a snapshot:** Use `create-volume --snapshot-id`. Note the **first-access penalty** — blocks are lazy-loaded from S3 when first read. For production, "warm" the volume first by reading all blocks before going live.

---

## 2. Instance Store

Instance store is **ephemeral** (temporary) storage physically attached to the host machine. Key differences from EBS:


| | EBS | Instance Store |
|---|---|---|
| Persistence | ✅ Survives stop/start | ❌ Lost on stop/terminate |
| Location | Network-attached | Physically on host |
| Use case | Databases, apps | Cache, buffers, temp data |
| Can be boot volume | ✅ | ✅ (but risky) |
| Can be stopped | ✅ | ❌ (terminate only) |

> **Exam trap:** "Instance-store backed instances cannot be stopped, only terminated." If a question asks about an instance that loses data when stopped — that's Instance Store.

---

## 3. Amazon EFS (Elastic File System)

EFS is a **managed NFS** (Network File System) for Linux workloads. The killer feature: **multiple EC2 instances can mount it simultaneously**, making it ideal for shared storage.
<img width="1440" height="720" alt="image" src="https://github.com/user-attachments/assets/709d0260-4221-4fe0-b6ab-bb1ce61a67da" />

**EFS Performance controls — exam-tested:**

- **Performance Mode:** General Purpose (default, low latency) vs Max I/O (higher throughput, slight latency tradeoff — for big data, media)
- **Storage Class:** Standard (frequently accessed) vs Infrequent Access (lower cost for rarely accessed files)
- **Throughput Mode:** Bursting (scales with size) vs Provisioned (you set the MB/s regardless of size)

> **Key rule:** EFS is for **Linux only** (NFS protocol). For Windows shared storage, AWS has FSx for Windows File Server. If a question mentions Linux + shared + multi-EC2 — answer is EFS.

---

## 4. Amazon S3 — The Most Tested Service

S3 is **object storage**, not a file system. You store objects (files) in buckets. Every object gets a unique URL. It's regionally provisioned but globally accessible.

**The "11 nines" durability (99.999999999%):** If you stored 10,000 objects, you'd statistically lose one object once every 10 million years. This comes from S3 automatically replicating data across multiple devices in multiple AZs.
<img width="1440" height="784" alt="image" src="https://github.com/user-attachments/assets/a3a70bfe-5517-492a-a145-e2288893483e" />

**S3 Versioning deep dive — exam must-know:**

When versioning is **enabled**: uploading the same key creates a new version (both exist, retrievable by version ID). Deleting adds a "delete marker" — the object isn't gone, just hidden. You can restore by removing the marker.

When versioning is **disabled/suspended**: uploading the same key **overwrites** permanently. Deleting is permanent. Once versioning is enabled, it can only be suspended, never fully disabled.

**Intelligent Tiering logic:** Objects not accessed for 30 days move to the infrequent access tier. If accessed again, they automatically move back. No retrieval fee (unlike Standard-IA). Ideal when access pattern is unknown.

**Object Lock modes:**
- **Compliance mode** — no one (not even root) can delete/overwrite during the retention period. Strictest.
- **Governance mode** — most users can't delete, but users with special permissions can override. Good for testing.

---

## 5. Amazon S3 Glacier — Long-Term Archiving

Glacier is for data you rarely need, stored at extremely low cost. Think of it as a **deep freezer** for your data — cheap to store, takes time to retrieve.

**How Glacier stores data:**
- **Archive** = the actual data (any file type, up to 40 TB each)
- **Vault** = a container holding archives (like a bucket in S3)
- Each account can create up to 1,000 vaults

**Three retrieval options — heavily tested:**

| Option | Speed | Cost | Use when |
|---|---|---|---|
| **Expedited** | 1–5 minutes | Highest | Urgent, occasional access |
| **Standard** | 3–5 hours | Medium | Default, planned retrieval |
| **Bulk** | 5–12 hours | Lowest | Large data, cost-sensitive |

**How to archive to Glacier:**
1. Via **S3 Lifecycle rules** — automatically transitions S3 objects to Glacier storage class over time
2. Via **Glacier API/CLI** directly — creates archives that only the Glacier API can retrieve (not visible in S3)

> ⚠️ Exam trap: Data archived via S3 lifecycle rules cannot be accessed via the Glacier API directly. AWS manages it on your behalf. Restore it using the S3 console or S3 API.

---

## 6. AWS Storage Gateway — The Hybrid Bridge

Storage Gateway is a **hybrid cloud storage service**. It sits on-premises (as a VM appliance or hardware device) and bridges your data center to AWS cloud storage. The exam loves this service because it solves a very specific real-world problem — companies that cannot move everything to the cloud immediately but want cloud benefits.
<img width="1440" height="762" alt="image" src="https://github.com/user-attachments/assets/6e2cb2a4-8814-4a79-99a5-fb6cb16de865" />


Storage Gateway runs **on-premises** (as a VM or hardware appliance) and connects your local environment to AWS storage. Three modes:> 
<img width="1440" height="656" alt="image" src="https://github.com/user-attachments/assets/1e76ed05-de5b-4da0-98fc-5ca1453993e9" />

**Exam trigger words:** "on-premises" + "AWS cloud storage" = Storage Gateway. If they say "replace physical tape backups" = Tape Gateway. If they say "shared file access on-premises" = File Gateway (NFS/SMB).

------

## Gateway Type 1 — File Gateway

**What it does:** Presents an **NFS or SMB file share** to your on-premises servers. Files written to the share are stored as **S3 objects** in your bucket. The gateway maintains a local cache of recently accessed files for low-latency reads.

**How it works internally:**
- Your application writes a file to the NFS/SMB mount point — it feels like writing to a local network share
- The gateway stores recently used files in the local cache (fast access)
- In the background it transfers the file to S3 over HTTPS
- Each file becomes one S3 object, preserving the full file path as the S3 key
- You can configure S3 lifecycle policies on the bucket — files automatically move to Glacier over time
<img width="1440" height="592" alt="image" src="https://github.com/user-attachments/assets/971ef7f6-7fd2-4ab6-90cc-c13f70ea593f" />

**Key exam points for File Gateway:**

**Files are native S3 objects.** Once in S3 they can be accessed directly by any AWS service — EC2, Lambda, Athena, EMR. You don't go through the gateway to access them from the cloud side.

**Two protocols supported:**
- **NFS** — for Linux/Unix-based servers
- **SMB** — for Windows servers, supports Active Directory authentication

**S3 storage classes for File Gateway:** You can configure the gateway to write directly to S3 Standard, S3 Standard-IA, or S3 Intelligent-Tiering per file share. Then lifecycle policies can push older files to Glacier.

**Exam scenario:** *"A company has on-premises Linux servers generating daily log files. They want to store logs in S3 and eventually archive them to Glacier, without changing their existing NFS workflow."* → **File Gateway** mounted as NFS share + S3 lifecycle policy transitioning to Glacier.

---

## Gateway Type 2 — Volume Gateway

**What it does:** Presents **iSCSI block storage volumes** to your on-premises servers. Looks exactly like a local hard disk to the OS. Data is stored in S3 as EBS snapshots. Two sub-modes — this is where candidates get confused:
<img width="1440" height="720" alt="image" src="https://github.com/user-attachments/assets/fa061fc9-d448-4e85-bd2d-cea093551444" />

### Cached Volumes Mode

- **Primary storage = S3.** All data lives in S3.
- **Local cache** holds only the most recently accessed data for low-latency reads.
- Volume sizes up to **32 TB per volume**, up to **32 volumes** = 1 PB total.
- Best for: large datasets where you want cloud as primary storage but still need fast local access to hot data.

### Stored Volumes Mode

- **Primary storage = on-premises.** All data is stored locally for the lowest possible latency.
- Data is **asynchronously backed up to S3** as EBS snapshots.
- Volume sizes up to **16 TB per volume**, up to **32 volumes** = 512 TB total.
- Best for: workloads that need full low-latency access to all data at all times, with cloud as backup/DR.

**Exam trap:** *"A company needs on-premises block storage with the lowest possible latency for ALL data, but wants cloud backups."* → **Stored volumes**, not Cached volumes. Cached volumes only cache a subset locally.

**Snapshots from Volume Gateway** are stored as **EBS snapshots** in S3. You can restore them to create a new EBS volume and attach it to an EC2 instance — useful for cloud-based disaster recovery.

---

## Gateway Type 3 — Tape Gateway

**What it does:** Replaces physical tape libraries with cloud-based virtual tapes. Your existing **backup software** (Veeam, Backup Exec, NetBackup) thinks it is writing to a real tape library — it uses iSCSI VTL (Virtual Tape Library) interface. Underneath, tapes are stored in S3 or Glacier.
<img width="1440" height="636" alt="image" src="https://github.com/user-attachments/assets/962e5099-4b0e-4a6f-9d0f-dcfb0ced00e2" />

**Two storage locations for tapes:**

- **Virtual Tape Library (VTL)** — active tapes stored in **S3**. Immediately accessible for restore. Up to 1,500 virtual tapes per gateway, up to 1 PB total.
- **Virtual Tape Shelf (VTS)** — archived tapes stored in **S3 Glacier or S3 Glacier Deep Archive**. Much cheaper. To restore, you must first retrieve the tape from the shelf back to the VTL — this takes time (just like real tape retrieval).

**Tape sizes:** Each virtual tape can be from 100 GB to 5 TB.

**Exam scenario:** *"A company uses Veritas NetBackup with physical tape libraries. They want to eliminate tape management costs and move backups to the cloud without changing their backup software or workflows."* → **Tape Gateway.** The software sees no difference — it still writes to what appears to be a tape library.

---

## All Three Gateways — Side by Side

This comparison is exam gold
<img width="1440" height="678" alt="image" src="https://github.com/user-attachments/assets/5ea5457e-5d55-4f75-b476-91517605ba0d" />

---

## Storage Gateway — Common Exam Scenarios

| Scenario in the question | Answer |
|---|---|
| On-prem servers need shared file storage backed by S3, NFS protocol | File Gateway |
| On-prem Windows servers need shared storage, Active Directory auth | File Gateway with SMB |
| On-prem app needs block storage, all data must stay local, cloud backup only | Volume Gateway — Stored mode |
| On-prem app needs block storage, OK with cloud as primary, cache hot data locally | Volume Gateway — Cached mode |
| Company wants to stop buying physical tapes, keep existing backup software | Tape Gateway |
| Disaster recovery — restore on-prem volume in AWS as EC2 instance | Volume Gateway (Stored) → EBS snapshot → create EBS volume → attach to EC2 |
| Reduce on-prem storage footprint, keep frequently used files locally | File Gateway (local cache) or Volume Cached mode |

---

## One Deployment Option Worth Knowing — Hardware Appliance

Besides running as a VM (VMware ESXi, Microsoft Hyper-V, KVM), AWS also offers a **physical Storage Gateway hardware appliance** that you order and rack in your data center. Useful when you don't have virtualization infrastructure or when you need a guaranteed hardware spec.

---

## Key Exam Rule — Storage Gateway is NOT a Migration Tool

Storage Gateway is for **ongoing hybrid connectivity** — your on-premises systems continuously reading and writing to/from AWS. It is not designed for one-time bulk data migration. For that you use **AWS DataSync** or **AWS Snowball** — which is exactly our next topic.

---

## AWS Data Migration Services — Deep Dive

These three services solve the same problem — **moving data into AWS** — but in completely different ways. The exam heavily tests which one to pick based on data volume, network bandwidth, time constraints, and whether the transfer is one-time or ongoing.
<img width="1440" height="636" alt="image" src="https://github.com/user-attachments/assets/964faad9-42b8-48a7-bcb4-e35b7e436b0b" />


------

## Service 1 — AWS Snowball Family

**The core idea:** When your internet connection is too slow or too expensive to transfer large amounts of data online, AWS ships you a physical encrypted storage device. You load your data onto it locally, ship it back to AWS, and they import it into S3.

**The Snowball family has three members — know them all:
<img width="1440" height="720" alt="image" src="https://github.com/user-attachments/assets/efd69b1e-92d1-4a43-8fcc-21d53a26e95f" />

****The Snowball process — step by step:**

1. Request a device via AWS Console → AWS ships it to you
2. Connect device to your local network
3. Install and run the **Snowball client** or use the **S3-compatible endpoint** on the device
4. Transfer data to the device locally at full LAN speed
5. Ship device back to AWS (prepaid shipping label included)
6. AWS ingests data into your S3 bucket
7. AWS securely wipes the device using NIST 800-88 standards
8. You receive a job completion notification

**Security on Snowball:**
- All data is encrypted with **256-bit AES encryption** using KMS keys — you own the keys
- The device has a **tamper-evident enclosure** — if it is physically tampered with, data becomes inaccessible
- Encryption keys **never travel with the device** — they stay in KMS

**The bandwidth rule of thumb — this appears in exam questions:**

> If it would take more than **1 week** to transfer your data over your existing internet connection, use Snowball instead.

A practical formula:
```
Time to transfer = Data size ÷ Available bandwidth

Example: 100 TB at 100 Mbps = 100 TB ÷ (100 Mbps ÷ 8) = 100 TB ÷ 12.5 MB/s
= 8,000,000 MB ÷ 12.5 MB/s = 640,000 seconds = ~7.4 days
→ Use Snowball
```

**Snowball Edge compute features — exam favourite:**
Snowball Edge devices can run **EC2 instances** and **Lambda functions locally** on the device. This is used for **edge computing** scenarios — pre-processing data before migration, running ML inference in remote locations without internet (oil rigs, ships, military bases). The compute capability works completely **offline**.

---

## Service 2 — AWS DataSync

**The core idea:** A managed online data transfer service that automates moving large amounts of data between on-premises storage and AWS storage services over the internet or AWS Direct Connect. It handles scheduling, monitoring, encryption, data validation, and bandwidth throttling for you.
<img width="1440" height="678" alt="image" src="https://github.com/user-attachments/assets/b43c38bb-15ad-4e63-8e72-45f67a7cd497" />

**How DataSync works:**

**Step 1 — Deploy the DataSync Agent** on-premises as a VM (VMware, Hyper-V, KVM, or EC2). The agent reads data from your source storage.

**Step 2 — Create a DataSync Task** in the AWS Console. Define source location, destination location, and transfer settings.

**Step 3 — DataSync transfers data** using a purpose-built protocol that is **10x faster than open-source tools** like rsync. It parallelises transfers, performs in-line data validation (checksums), and supports bandwidth throttling so it doesn't saturate your network.

**Step 4 — Schedule or run on demand.** DataSync can run on a schedule (hourly, daily, weekly) for ongoing sync, or on-demand for one-time migration.

**Key DataSync features the exam tests:**

**Automatic data validation** — DataSync verifies the integrity of every file transferred by comparing checksums. If a file is corrupted in transit, it is re-transferred automatically.

**Bandwidth throttling** — you set a maximum bandwidth limit so DataSync doesn't impact production network traffic during business hours.

**Preserves metadata** — file permissions, timestamps, ownership, and ACLs are preserved during transfer. Important for NFS and SMB shares.

**Incremental transfers** — on subsequent runs DataSync only transfers files that have changed, not the entire dataset. Highly efficient for ongoing sync.

**Supported sources:**
- NFS shares (on-premises Linux)
- SMB shares (on-premises Windows)
- HDFS (Hadoop clusters)
- Self-managed object storage (S3-compatible)
- Other AWS services (EFS to EFS, S3 to EFS)

**Supported destinations:** S3 (any storage class), EFS, FSx for Windows, FSx for Lustre

---

## Service 3 — AWS Transfer Family (Transfer for SFTP)

**The core idea:** A fully managed service that provides **SFTP, FTPS, and FTP endpoints** that store files directly in S3 or EFS. Your existing SFTP clients, workflows, and trading partners connect to these endpoints — they see a standard SFTP server, but underneath all files land in S3.
<img width="1440" height="592" alt="image" src="https://github.com/user-attachments/assets/6d9bf0b3-2dbd-4f4e-90d6-28a997acab17" />

**Three protocols supported:**
- **SFTP** — SSH File Transfer Protocol, port 22. Most secure, most common.
- **FTPS** — FTP over TLS, port 21. Common in financial industry EDI workflows.
- **FTP** — plain FTP, unencrypted. Only for use within VPCs — never expose to internet.

**Authentication options:**
- **Service-managed** — AWS Transfer manages users with SSH public keys stored in the service
- **Custom identity provider** — your own authentication via API Gateway + Lambda. Supports LDAP, Active Directory, any custom auth system.

**Endpoint types:**
- **Public endpoint** — accessible over the internet
- **VPC endpoint** — accessible only within your VPC (private, more secure)
- **VPC endpoint with internet access** — VPC-hosted but with an Elastic IP for external access

**Key exam point — why use Transfer Family over self-managed SFTP?**
You don't manage any EC2 instances, SSH server configurations, or storage mounts. It scales automatically, has built-in high availability across AZs, and files land directly in S3 without any intermediate storage.

---

## The Decision Framework — Which Service to Use

This is the most important thing to learn. The exam gives you a scenario and you must pick the right service:
<img width="1440" height="890" alt="image" src="https://github.com/user-attachments/assets/c7e1f8e8-e531-444d-ba13-f43164542b66" />

---

## Complete Comparison — All Three Services

| Factor | Snowball | DataSync | Transfer Family |
|---|---|---|---|
| Transfer method | Physical device shipped | Online over network | Online via SFTP/FTPS/FTP |
| Data volume | TBs to Exabytes | GBs to 10s of TBs | Files transferred per session |
| Transfer type | One-time bulk | One-time or recurring | Ongoing / on-demand |
| Network required | No (offline) | Yes | Yes |
| Automation | Manual (order, load, ship) | Scheduled tasks | Protocol-driven |
| Data validation | Yes (checksum) | Yes (automatic) | No built-in validation |
| Metadata preserved | No | Yes | Yes |
| Key use case | Migrate DC to AWS | Sync NFS/SMB/HDFS to AWS | Replace managed SFTP server |
| Destination | S3 | S3, EFS, FSx | S3, EFS |

---

## Exam Scenario Cheat Sheet — All Migration Services

| Scenario | Answer |
|---|---|
| 500 TB to migrate, 10 Mbps internet connection | Snowball Edge — too slow online |
| Migrate 5 PB data center to AWS | Multiple Snowball Edge devices in parallel |
| 100 EB migration — entire data center decommission | Snowmobile |
| Weekly sync of on-prem NFS share to S3 | DataSync with scheduled task |
| One-time migration of 10 TB NFS share, good bandwidth | DataSync |
| Financial partners send daily EDI files via SFTP | Transfer Family (SFTP endpoint → S3) |
| Replace self-managed SFTP server, files go to S3 | Transfer Family |
| Initial bulk migration + ongoing incremental sync | Snowball first, then DataSync for deltas |
| Run ML inference on device in a remote location without internet | Snowball Edge Compute Optimized |
| NFS to EFS migration, preserve file permissions | DataSync (preserves metadata) |
| Trading partner uses FTPS, files need to land in S3 | Transfer Family with FTPS endpoint |

---

## Important Combination Pattern — Snowball + DataSync

This appears frequently in the exam as the best practice for large migrations:

**Phase 1:** Use Snowball to migrate the bulk of historical data quickly — avoids weeks of internet transfer.

**Phase 2:** While Snowball is in transit, use DataSync to sync any new/changed files that arrived after the Snowball cutoff — keeps data current.

**Phase 3:** After Snowball import completes and DataSync has caught up, cut over your application to use AWS storage. DataSync continues running on a schedule for ongoing sync if needed.

---

All three migration services are now fully covered. You have completed the entire Module 8 content!
<img width="1440" height="530" alt="image" src="https://github.com/user-attachments/assets/b83c7db5-12bb-4011-a574-0a0913a2c68c" />

---

## 🎯 SAA-C03 Exam Traps & Quick Recall

Here's a summary of the most common exam pitfalls for this topic:

**EBS:**
- st1 and sc1 HDD volumes **cannot** be boot volumes
- Snapshots are **incremental** but **independently restorable** — deleting one doesn't corrupt others
- EBS volumes exist in **one AZ only** — to move to another AZ, take a snapshot and restore in the new AZ

**S3:**
- Default: all objects and buckets are **private**
- Versioning: once enabled, can only be **suspended**, not disabled
- Object Lock **Compliance mode** = nobody (including root) can delete during retention period
- Intelligent Tiering moves objects after **30 days** of no access; no retrieval fee
- One Zone-IA = stored in **one AZ only**, 20% cheaper than Standard-IA but risks data loss if AZ fails

**Glacier:**
- No AWS Management Console for direct archive operations (only API/CLI or S3 lifecycle)
- **Expedited** = 1–5 min (most expensive), **Bulk** = 5–12 hrs (cheapest)

**Instance Store:**
- Data is **lost** on stop, hibernate, or terminate — survives a reboot only
- Cannot be detached and reattached like EBS

**EFS vs EBS vs S3:**
- EFS = shared, NFS, Linux, scales automatically, multiple EC2s
- EBS = single EC2 (usually), block storage, one AZ
- S3 = internet-accessible, object storage, no file system semantics

---
