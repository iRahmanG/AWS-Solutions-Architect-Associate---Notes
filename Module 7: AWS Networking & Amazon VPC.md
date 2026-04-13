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

<img width="1440" height="2710" alt="image" src="https://github.com/user-attachments/assets/549b3a55-7f91-4450-b6a8-c3fa818c4903" />


**Things to remember that often trick people:**
- VPC peering is **non-transitive** — no shortcuts through a middle VPC
- Gateway endpoints only work for **S3 and DynamoDB**
- NAT gateway is **one-way outbound** only — internet cannot initiate connections
- NACL rules are evaluated in **ascending order by rule number** — first match wins
- Security groups can **reference other security groups** as the source (e.g. allow traffic from bastion's SG)

---

#  AWS SAA Networking Practice Quiz — 20 Questions


## Q1 (Select TWO that apply)

A company has EC2 instances in a private subnet that need to download OS patches from the internet. The solution must be highly available across two Availability Zones. What should you configure?

- A. Create one NAT Gateway in a private subnet, update both route tables
- B. Create a NAT Gateway in each AZ's public subnet
- C. Update each private subnet's route table to point 0.0.0.0/0 to its AZ's NAT Gateway
- D. Attach an Internet Gateway directly to the private subnets
- E. Use VPC Peering to route traffic

**Answer:** B, C

**Explanation:**
B+C are correct. For HA, deploy one NAT GW per AZ (B) and route each AZ's private subnet to its local NAT GW (C). One NAT GW creates a single point of AZ-level failure. D is wrong — IGW on private subnet makes it public. A is wrong — NAT GW must be in PUBLIC subnet.

---

## Q2 (Single answer)

A team added a DENY rule in a Network ACL for port 22 from 0.0.0.0/0. SSH from the internet stopped working but internal SSH also stopped. Port 22 was already allowed in the Security Group. What likely went wrong?

- A. Security groups override NACLs
- B. The NACL DENY rule number is lower than the existing ALLOW rule
- C. NACLs apply to all instances including those using private IPs
- D. The Security Group should also have a DENY rule

**Answer:** C

**Explanation:**
C is correct. NACL rules apply to all traffic crossing the subnet boundary including internal cross-subnet traffic. Both B and C could cause issues but the question says internal SSH also broke — pointing to C. NACLs use stateless evaluation and apply to subnet-level traffic, not just internet-sourced traffic.

---

## Q3 (Select TWO that apply)

A company connects 3 VPCs using VPC Peering: VPC-A ↔ VPC-B and VPC-B ↔ VPC-C. Instances in VPC-A cannot reach instances in VPC-C. What explains this, and what is the solution? (Select TWO)

- A. Transitive peering is not supported in AWS
- B. Create a direct peering connection between VPC-A and VPC-C
- C. Configure VPC-B as a transit hub
- D. Use Transit Gateway instead to support transitive routing
- E. Add route table entries in VPC-B to forward traffic

**Answer:** A, B

**Explanation:**
A+B are correct. AWS does NOT support transitive peering — A↔B↔C does not allow A↔C. The fix is a direct peering between A and C (B). Transit Gateway supports this at scale but peering is cheaper for small numbers. E is a trap — VPC-B cannot act as transit for peered VPCs.

---

## Q4 (Single answer)

An EC2 instance in a custom VPC's public subnet has no public IP. You just launched it with auto-assign public IP disabled. You need it accessible from the internet immediately without stopping it. What should you do?

- A. Enable auto-assign public IP setting on the subnet
- B. Allocate an Elastic IP and associate it with the instance
- C. Stop and restart the instance to get a public IP
- D. Attach a new network interface with a public IP

**Answer:** B

**Explanation:**
B is correct. Auto-assign public IP is set at launch time and cannot be changed for a running instance. EIP (B) can be associated with a running instance immediately. Stopping and restarting (C) gives a NEW dynamic public IP, but the question says 'without stopping'. A changes future instances, not this one.

---

## Q5 (Single answer)

Your company uses AWS Direct Connect for production traffic. Security team requires all data in transit to be encrypted. What is the correct approach?

- A. Direct Connect encrypts data automatically using AES-256
- B. Enable encryption on the Direct Connect connection in the AWS Console
- C. Establish a VPN connection over the Direct Connect link
- D. Use HTTPS for application-level encryption only

**Answer:** C

**Explanation:**
C is correct. Direct Connect does NOT encrypt traffic by default — it is a private dedicated network link. To add encryption, you run a VPN (IPSec) tunnel over the DX connection. This is a very commonly tested SAA point. A and B are wrong — there is no built-in encryption toggle on DX.

---

## Q6 (Single answer)

A solutions architect needs EC2 instances in a private subnet to access Amazon S3 without routing through the internet or incurring NAT Gateway costs. What should they use?

- A. Interface VPC Endpoint for S3
- B. Gateway VPC Endpoint for S3
- C. NAT Gateway with S3 bucket policy
- D. VPC Peering with S3 service VPC

**Answer:** B

**Explanation:**
B is correct. Gateway VPC Endpoint for S3 (and DynamoDB) is free and routes traffic through AWS network without internet or NAT. It works by adding a prefix list entry to the route table. Interface endpoints cost money. D is wrong — S3 is not in a VPC you can peer with.

---

## Q7 (Select TWO that apply)

A web application is hosted in a VPC. A security team wants to block all traffic from a specific IP address (203.0.113.5) that is launching a DDoS attack. Which TWO actions together will block this traffic?

- A. Add a DENY rule in the Security Group for that IP
- B. Add a DENY rule in the Network ACL with a lower rule number than existing ALLOW rules
- C. Update the NACL outbound rules to block responses
- D. Add a DENY rule in the NACL inbound rules for 203.0.113.5
- E. Remove the Security Group's ALLOW rule for port 80

**Answer:** B, D

**Explanation:**
B+D are correct. Security Groups cannot DENY specific IPs (A is wrong — SGs have no deny rules). The correct tool is NACL. You need both: a DENY inbound rule (D) with a lower rule number than any ALLOW for the same port (B). NACL is stateless so blocking inbound is sufficient for dropping the attack. C is not needed since the request never reaches the instance.

---

## Q8 (Single answer)

A company's VPC has a NAT Instance (not NAT Gateway) in a public subnet. EC2 instances in the private subnet cannot reach the internet even though the route table points 0.0.0.0/0 to the NAT instance's ENI. What is the most likely cause?

- A. The NAT instance needs an Elastic IP
- B. Source/Destination check is still enabled on the NAT instance
- C. The NAT instance security group blocks outbound traffic
- D. NAT instances cannot route to public internet

**Answer:** B

**Explanation:**
B is correct. NAT instances require that the Source/Destination Check be DISABLED on their ENI. By default, EC2 checks that it is the source or destination of traffic. A NAT instance forwards traffic for other hosts, so this check must be off. This is the #1 NAT instance configuration mistake on the exam.

---

## Q9 (Single answer)

An application uses VPC peering between VPC-A (10.0.0.0/16) and VPC-B (10.0.0.0/16). The peering connection fails at creation. Why?

- A. Cross-account peering is not supported
- B. The VPCs have overlapping CIDR blocks
- C. VPC peering requires a Transit Gateway
- D. Both VPCs must be in the same region

**Answer:** B

**Explanation:**
B is correct. VPC peering cannot be established between VPCs with overlapping (or identical) CIDR ranges.

---

## Q10 (Select TWO that apply)

A company wants instances in Subnet-A to communicate with instances in Subnet-B, both in the same VPC. A new custom NACL was applied to both subnets. Connectivity broke. What are TWO likely causes?

- A. New custom NACLs deny all inbound and outbound traffic by default
- B. Route tables need to be updated for intra-VPC traffic
- C. Outbound rules in the NACL must explicitly allow return traffic
- D. Security groups block intra-subnet VPC traffic by default
- E. The NACL must have the same rule numbers on both subnets

**Answer:** A, C

**Explanation:**
A+C are correct. A new custom NACL has a wildcard DENY * rule that blocks all traffic (unlike the default NACL which allows all). Additionally since NACLs are stateless (C), return traffic is not automatic — you must add outbound ALLOW rules for ephemeral ports. Route tables (B) for intra-VPC traffic use the local route which is automatic.

---

## Q11 (Single answer)

A financial company uses Direct Connect for low-latency access to AWS. They want a backup connection that activates automatically if DX fails. What is the recommended approach?

- A. Create a second Direct Connect connection to the same location
- B. Set up a Site-to-Site VPN as backup; BGP will prefer DX when available
- C. Use Global Accelerator as DX failover
- D. Configure VPC peering as the backup path

**Answer:** B

**Explanation:**
B is correct. Site-to-Site VPN over the internet serves as backup. BGP routing ensures DX is preferred (lower cost metric) and VPN is used as failover. This is the standard AWS HA pattern for DX. A is better HA but both DX connections could fail if the same DX location has an outage. C and D are wrong — they don't provide on-premises connectivity.

---

## Q12 (Select TWO that apply)

An EC2 instance in us-east-1a cannot connect to the internet after you configured everything. The instance is in a public subnet, has an Elastic IP, and the Security Group allows port 443 outbound. What TWO items should you check first?

- A. Whether an Internet Gateway is attached to the VPC
- B. Whether the subnet route table has 0.0.0.0/0 → Internet Gateway
- C. Whether the NACL allows outbound port 443 and inbound ephemeral ports
- D. Whether the VPC has a NAT Gateway
- E. Whether the instance type supports public IPs

**Answer:** A, B

**Explanation:**
A+B are correct. These are the two most common misses: forgetting to attach an IGW to the VPC, and forgetting to add the default route to the route table. Without both, no internet access is possible regardless of SG or EIP. C is worth checking but A+B are the first steps. D is for private subnets. E is not a real constraint.

---

## Q13 (Single answer)

A company wants to connect their on-premises DNS with AWS Route 53 for hybrid resolution. On-premises servers need to resolve private hosted zone names in AWS. What should you use?

- A. Route 53 Resolver inbound endpoint
- B. Route 53 Resolver outbound endpoint
- C. Public hosted zone in Route 53
- D. AWS Direct Connect public VIF

**Answer:** A

**Explanation:**
A is correct. Route 53 Resolver inbound endpoint allows on-premises resolvers to forward DNS queries to AWS (into the VPC). Outbound endpoint (B) is for EC2 instances that need to resolve on-premises DNS names. The direction is from the perspective of the query — inbound = coming into AWS.

---

## Q14 (Single answer)

A VPC has both a Gateway Endpoint for S3 and an Internet Gateway. An EC2 instance in a private subnet (with route to NAT Gateway) accesses S3. Which path does the traffic take?

- A. Through NAT Gateway → Internet Gateway → S3
- B. Through the Gateway Endpoint (more specific route wins)
- C. Traffic is load-balanced between both paths
- D. S3 is always accessed via the internet regardless of endpoint

**Answer:** B

**Explanation:**
B is correct. Gateway VPC Endpoints add a more specific prefix list route to the route table. The most specific route always wins in AWS routing. Since the prefix list for S3 is more specific than 0.0.0.0/0, all S3 traffic goes through the endpoint automatically. This is also why Gateway Endpoints reduce NAT Gateway data costs.

---

## Q15 (Single answer)

You need to share a service (NLB in front of your application) in VPC-A with a customer's VPC-B, without giving them access to the rest of your VPC. What should you use?

- A. VPC Peering between VPC-A and VPC-B
- B. AWS Transit Gateway connecting both VPCs
- C. AWS PrivateLink with an Interface Endpoint in VPC-B
- D. VPN connection between the two VPCs

**Answer:** C

**Explanation:**
C is correct. AWS PrivateLink allows you to expose a specific service (behind an NLB) to other VPCs without peering or exposing the entire VPC network. The consumer creates an Interface Endpoint pointing to your Endpoint Service. This is the standard pattern for secure service sharing.

---

## Q16 (Single answer)

A company has 50 VPCs that all need to access a shared services VPC (Active Directory, logging). Which architecture minimises management overhead?

- A. Create VPC peering between each of the 50 VPCs and the shared VPC
- B. Use a Transit Gateway with the shared VPC and all 50 VPCs attached
- C. Create a hub-and-spoke with VPN CloudHub
- D. Deploy the shared services in every VPC

**Answer:** B

**Explanation:**
B is correct. Transit Gateway is the scalable hub-and-spoke solution. 50 peering connections (A) is difficult to manage. TGW simplifies routing and scaling.

---

## Q17 (Select TWO that apply)

A security team notices an instance is sending unexpected traffic to 203.0.113.0/24. They want to investigate without terminating the instance. Which TWO actions help?

- A. Enable VPC Flow Logs to capture traffic metadata
- B. Take a snapshot of the instance and analyse offline
- C. Attach an additional ENI to monitor traffic
- D. Update NACL to block 203.0.113.0/24 outbound immediately
- E. Review CloudTrail for API calls related to the instance

**Answer:** A, E

**Explanation:**
A+E are correct. Flow Logs capture traffic details (source/destination/IPs). CloudTrail shows API-level activity (who changed what). Together they provide strong investigation capability.

---

## Q18 (Single answer)

An architect is designing a subnet for a highly available application across 3 AZs. They need at least 100 instances per AZ and room to scale to 200. What CIDR should be assigned to each subnet?

- A. /25 (128 IPs, 123 usable)
- B. /24 (256 IPs, 251 usable)
- C. /26 (64 IPs, 59 usable)
- D. /27 (32 IPs, 27 usable)

**Answer:** B

**Explanation:**
B is correct. /24 provides 251 usable IPs (256 - 5 reserved by AWS). This supports scaling to 200 instances.

---

## Q19 (Select TWO that apply)

A developer's EC2 instance in VPC1 (10.0.0.0/16) needs to connect to an RDS database in VPC2 (172.16.0.0/16). VPC peering is established but connection fails.

- A. Route in VPC1 route table: 172.16.0.0/16 → pcx-xxxxx
- B. Route in VPC2 route table: 10.0.0.0/16 → pcx-xxxxx
- C. Internet Gateway in both VPCs
- D. Security group on RDS allowing inbound from 10.0.0.0/16
- E. NAT Gateway in VPC2

**Answer:** A, D

**Explanation:**
A+D are correct. Routing must exist AND security groups must allow traffic. Without SG rule, traffic is blocked even if routing is correct.

---

## Q20 (Single answer)

An application running in a private subnet needs to call AWS Systems Manager API without internet access. What is the best solution?

- A. Move instance to public subnet
- B. Create Interface VPC Endpoint for Systems Manager
- C. Create Gateway Endpoint
- D. Allow outbound HTTPS

**Answer:** B

**Explanation:**
B is correct. Interface endpoints (PrivateLink) allow private access to AWS services like SSM without internet or NAT.

---
