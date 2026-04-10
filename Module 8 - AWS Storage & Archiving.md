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

Storage Gateway runs **on-premises** (as a VM or hardware appliance) and connects your local environment to AWS storage. Three modes:> 
<img width="1440" height="656" alt="image" src="https://github.com/user-attachments/assets/1e76ed05-de5b-4da0-98fc-5ca1453993e9" />

**Exam trigger words:** "on-premises" + "AWS cloud storage" = Storage Gateway. If they say "replace physical tape backups" = Tape Gateway. If they say "shared file access on-premises" = File Gateway (NFS/SMB).

---

## 7. Data Transfer & Migration Services
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
