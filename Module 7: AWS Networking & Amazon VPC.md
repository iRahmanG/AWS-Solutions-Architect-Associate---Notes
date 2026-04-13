#  Module 7: AWS Networking & Amazon VPC


##  Section 1: What is Amazon VPC?

Think of AWS as a massive apartment building. **Your VPC is your private apartment** — completely isolated from others, with your own rooms (subnets), locks (security groups), hallways (route tables), and doors (gateways).

<img width="1440" height="800" alt="image" src="https://github.com/user-attachments/assets/1afb2235-a855-48d9-8ad3-12d88af18992" />


---

##  1. Amazon VPC — The Foundation

A **VPC (Virtual Private Cloud)** is your own private, logically isolated network inside AWS. Think of it like building a private office floor inside a shared skyscraper — you control all the rooms, doors, and security.

**Key facts for SAA-C03:**
- Lives inside a single **AWS Region** but spans multiple Availability Zones
- IP range defined by a **CIDR block** — must be between `/16` (65,536 IPs) and `/28` (16 IPs)
- Uses **RFC 1918** private ranges: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`
- AWS reserves **5 IPs** per subnet (first 4 + last 1)
- Every AWS account gets a **default VPC** with CIDR `172.31.0.0/16`

Here's how a full VPC is structured:---
<img width="1200" height="600" alt="image" src="https://github.com/user-attachments/assets/9bd12e6a-a1d5-4f6c-9c7f-47d7e4d33eba" />


##  2. CIDR Notation — Must Know for the Exam

| CIDR | Address Range | # of IPs |
|---|---|---|
| `10.0.0.0/16` | 10.0.0.0 – 10.0.255.255 | 65,536 |
| `10.0.1.0/24` | 10.0.1.0 – 10.0.1.255 | 256 |
| `10.0.1.0/28` | 10.0.1.0 – 10.0.1.15 | 16 (minimum subnet) |
| `0.0.0.0/0` | All IPs | Everywhere (used for internet routes) |

**The 5 reserved IPs per subnet** (e.g. in `10.0.0.0/24`):
- `.0` → Network address
- `.1` → AWS VPC Router
- `.2` → DNS server
- `.3` → Reserved by AWS for future use
- `.255` → Broadcast (not supported, still reserved)

---

##  3. Subnets & Route Tables

A **subnet** is a slice of your VPC's IP range, confined to **one Availability Zone**. What makes a subnet "public" vs "private" is purely its **route table**:**Key rule:** A subnet is public **only if** its route table has a `0.0.0.0/0 → internet gateway` entry. That's it.
<img width="1440" height="636" alt="image" src="https://github.com/user-attachments/assets/61376368-768f-49a4-89c6-6eb97e25d372" />

---

##  4. VPC Connectivity Options — Most Exam Questions Come From Here

This is the most tested area. Here's the full picture:
<img width="1200" height="600" alt="image" src="https://github.com/user-attachments/assets/d0776d0a-51f0-45f5-827a-7b7f4ab71836" />
<img width="1408" height="768" alt="image" src="https://github.com/user-attachments/assets/2655c855-ae94-44a4-9f62-e8b2dab2e8b9" />


###  NAT Gateway
- Lets **private subnet instances reach the internet** (for updates, patches) but **blocks inbound internet connections**
- AWS-managed, built-in redundancy within an AZ
- Needs an **Elastic IP address**
- You must update the **private subnet's route table** to point `0.0.0.0/0 → NAT gateway`
- Best practice: one NAT gateway **per AZ** for high availability

<img width="1440" height="1102" alt="image" src="https://github.com/user-attachments/assets/d545e0c3-079b-4c99-a898-8e22f3b37a8b" />


###  VPC Peering
- Direct private connection between **two VPCs** (same or different accounts, same or different regions)
- Traffic stays on **AWS backbone** — no internet
- **Critical limitations:**
  - No **overlapping CIDR blocks**
  - No **transitive peering** — if A↔B and B↔C, then A cannot reach C through B
  - No internet gateway sharing between peers

###  Site-to-Site VPN
- Encrypted **IPsec tunnel** over the public internet between your VPC and corporate datacenter
- Needs: **Virtual Private Gateway (VGW)** on AWS side + **Customer Gateway** on your side
- Each VGW has **two tunnels** for redundancy

###  AWS Direct Connect
- A **physical, dedicated private line** from your datacenter to AWS — no internet involved
- More reliable, consistent bandwidth, lower latency than VPN
- Can be combined with VPN (Direct Connect + VPN) for encrypted dedicated connection

###  AWS VPN CloudHub
- **Hub-and-spoke** model — multiple branch offices connect to a single VGW
- Branches can communicate with each other **through AWS**

###  VPC Endpoints (no internet required!)
| Type | Used For | How |
|---|---|---|
| **Interface endpoint** (PrivateLink) | Most AWS services, custom services | Creates an ENI with private IP |
| **Gateway endpoint** | S3 and DynamoDB **only** | Added as a route in the route table |

<img width="1440" height="974" alt="image" src="https://github.com/user-attachments/assets/91de3ae2-6aab-45ea-b6cb-2f5bb4f1efcf" />


###  AWS Transit Gateway
- The **"network hub"** — connect thousands of VPCs and on-prem networks to one central gateway
- Eliminates the mesh of peering connections
- Supports route propagation automatically

---

##  5. Securing Your Network — Layered Defense
AWS uses a **4-layer defense model**:
<img width="1440" height="720" alt="image" src="https://github.com/user-attachments/assets/9c2b36fa-b604-4571-b108-f9670088f41a" />

### Security Groups vs NACLs — The Most Common Exam Comparison

| Feature | Security Group | Network ACL |
|---|---|---|
| **Level** | Instance / ENI | Subnet |
| **Stateful?** | ✅ Yes — responses auto-allowed | ❌ No — must explicitly allow both directions |
| **Rules** | Allow only | Allow AND Deny |
| **Rule evaluation** | All rules evaluated together | Rules evaluated in number order (lowest first) |
| **Default** | Deny all inbound, Allow all outbound | Allow all inbound AND outbound |
| **Association** | One per ENI (multiple allowed) | One per subnet |

**Stateful analogy:** Security group is like a bouncer who remembers — if you let someone in, they can walk out without being checked again. NACL is like a strict checkpoint — you must have a valid pass in both directions every time.

---

##  6. Bastion Host

A **Bastion host** is a "jump server" in your public subnet that lets admins securely SSH into private instances.

- Linux: Use **SSH agent forwarding** — private keys stay on your laptop, not the bastion
- Windows: Use **Remote Desktop Gateway (RD Gateway)** — accepts HTTPS (443), proxies to RDP (3389)
- Security group rule: private instances only allow SSH **from the bastion's security group ID**, not from any IP

---

##  7. Troubleshooting VPC — Exam Checklist

When an EC2 instance can't be reached, check in this order:

1. **Is the instance running?** (System Status + Instance Status both ✅)
2. **Security group** — does it allow the protocol/port? (port 22 for SSH, 80/443 for web)
3. **Network ACL** — are both inbound AND outbound rules allowing traffic? (stateless!)
4. **Route table** — does the subnet have a route pointing traffic to the right target?
5. **Public IP** — does the instance have a public/Elastic IP?
6. **Internet Gateway** — is one attached to the VPC?

For **NAT troubleshooting:** Check if the private subnet's route table points `0.0.0.0/0 → NAT gateway`. For a NAT **instance** (older approach), also check that **Source/Dest Check is disabled**.

---

##  Key SAA-C03 Exam Tips

**Scenario → Answer mapping** (commonly tested):

| Scenario | Answer |
|---|---|
| Private instance needs software updates from internet | **NAT Gateway** |
| Two VPCs need to communicate privately | **VPC Peering** |
| Need to connect thousands of VPCs efficiently | **Transit Gateway** |
| Access S3 without going over internet | **Gateway Endpoint** |
| Access EC2/Systems Manager without internet | **Interface Endpoint (PrivateLink)** |
| Dedicated physical line to AWS, no internet | **AWS Direct Connect** |
| Encrypted connection to on-prem over internet | **Site-to-Site VPN** |
| Multiple branch offices connecting to one VPC | **VPN CloudHub** |

**Things to remember that often trick people:**
- VPC peering is **non-transitive** — no shortcuts through a middle VPC
- Gateway endpoints only work for **S3 and DynamoDB**
- NAT gateway is **one-way outbound** only — internet cannot initiate connections
- NACL rules are evaluated in **ascending order by rule number** — first match wins
- Security groups can **reference other security groups** as the source (e.g. allow traffic from bastion's SG)

---

