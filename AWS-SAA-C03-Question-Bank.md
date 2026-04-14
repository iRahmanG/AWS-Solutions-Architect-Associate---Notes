<div align = "center"> 
  
   # AWS SAA-C03 Question Bank 
</div>

---

**Q1.** A company collects data for temperature, humidity, and atmospheric pressure in cities across multiple continents. The average volume of data that the company collects from each site daily is 500 GB. Each site has a high-speed Internet connection. The company wants to aggregate the data from all these global sites as quickly as possible in a single Amazon S3 bucket. The solution must minimize operational complexity.
Which solution meets these requirements?

- A. Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.
- B. Upload the data from each site to an S3 bucket in the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket. Then remove the data from the origin S3 bucket.
- C. Schedule AWS Snowball Edge Storage Optimized device jobs daily to transfer data from each site to the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.
- D. Upload the data from each site to an Amazon EC2 instance in the closest Region. Store the data in an Amazon Elastic Block Store (Amazon EBS) volume. At regular intervals, take an EBS snapshot and copy it to the Region that contains the destination S3 bucket. Restore the EBS volume in that Region.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Transfer Acceleration uses AWS edge locations to speed up uploads over long distances, and multipart uploads enable parallel, efficient transfers. This approach minimizes operational complexity because it requires no intermediate storage or replication management. Option B adds replication overhead; option C is impractical for daily transfers; option D is unnecessarily complex (AWS Docs: S3 Transfer Acceleration).
</details>

---

**Q2.** A company needs the ability to analyze the log files of its proprietary application. The logs are stored in JSON format in an Amazon S3 bucket. Queries will be simple and will run on-demand. A solutions architect needs to perform the analysis with minimal changes to the existing architecture. What should the solutions architect do to meet these requirements with the LEAST amount of operational overhead?

- A. Use Amazon Redshift to load all the content into one place and run the SQL queries as needed.
- B. Use Amazon CloudWatch Logs to store the logs. Run SQL queries as needed from the Amazon CloudWatch console.
- C. Use Amazon Athena directly with Amazon S3 to run the queries as needed.
- D. Use AWS Glue to catalog the logs. Use a transient Apache Spark cluster on Amazon EMR to run the SQL queries as needed.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon Athena is a serverless query service that allows you to analyze data directly in S3 using standard SQL, with no infrastructure to manage. This requires minimal changes (just point Athena to the S3 bucket) and has the least operational overhead. Redshift (A) requires loading data; CloudWatch Logs (B) is for log monitoring, not ad-hoc SQL on JSON files; EMR (D) requires managing a cluster (AWS Docs: Amazon Athena).
</details>

---

**Q3.** A company uses AWS Organizations to manage multiple AWS accounts for different departments. The management account has an Amazon S3 bucket that contains project reports. The company wants to limit access to this S3 bucket to only users of accounts within the organization in AWS Organizations. Which solution meets these requirements with the LEAST amount of operational overhead?

- A. Add the `aws:PrincipalOrgID` global condition key with a reference to the organization ID to the S3 bucket policy.
- B. Create an organizational unit (OU) for each department. Add the `aws:PrincipalOrgPaths` global condition key to the S3 bucket policy.
- C. Use AWS CloudTrail to monitor the `CreateAccount`, `InviteAccountToOrganization`, `LeaveOrganization`, and `RemoveAccountFromOrganization` events. Update the S3 bucket policy accordingly.
- D. Tag each user that needs access to the S3 bucket. Add the `aws:PrincipalTag` global condition key to the S3 bucket policy.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Using the `aws:PrincipalOrgID` condition key in an S3 bucket policy is the simplest and most efficient way to grant access to all principals (users/roles) belonging to any account in the entire AWS Organization. Option B is more granular but adds complexity; options C and D involve unnecessary manual processes or tagging overhead (AWS Docs: IAM JSON Policy Elements: Condition Operators).
</details>

---

**Q4.** An application runs on an Amazon EC2 instance in a VPC. The application processes logs that are stored in an Amazon S3 bucket. The EC2 instance needs to access the S3 bucket without connectivity to the internet. Which solution will provide private network connectivity to Amazon S3?

- A. Create a gateway VPC endpoint to the S3 bucket.
- B. Stream the logs to Amazon CloudWatch Logs. Export the logs to the S3 bucket.
- C. Create an instance profile on Amazon EC2 to allow S3 access.
- D. Create an Amazon API Gateway API with a private link to access the S3 endpoint.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* A Gateway VPC Endpoint for S3 allows traffic between the VPC and S3 to stay within the AWS network, not traverse the internet. Option B still requires internet for the EC2 instance to reach CloudWatch; option C (Instance Profile) provides permissions, not private connectivity; option D is overly complex (AWS Docs: VPC Endpoints - Gateway Endpoints).
</details>

---

**Q5.** A company is hosting a web application on AWS using a single Amazon EC2 instance that stores user-uploaded documents in an Amazon EBS volume. For better scalability and availability, the company duplicated the architecture and created a second EC2 instance and EBS volume in another Availability Zone, placing both behind an Application Load Balancer. After completing this change, users reported that, each time they refreshed the website, they could see one subset of their documents or the other, but never all of the documents at the same time. What should a solutions architect propose to ensure users see all of their documents at once?

- A. Copy the data so both EBS volumes contain all the documents.
- B. Configure the Application Load Balancer to direct a user to the server with the documents.
- C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS.
- D. Configure the Application Load Balancer to send the request to both servers. Return each document from the correct server.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* The issue is that each EC2 instance has its own EBS volume with different subsets of documents (shared-nothing architecture). Moving to a shared file system like Amazon EFS allows all instances to read/write from a single, consistent data source. Option A requires manual sync; option B is not feasible with ALB; option D is incorrect as ALB cannot split a single request to both servers (AWS Docs: Amazon EFS).
</details>

---

**Q6.** A company uses NFS to store large video files in on-premises network attached storage. Each video file ranges in size from 1 MB to 500 GB. The total storage is 70 TB and is no longer growing. The company decides to migrate the video files to Amazon S3. The company must migrate the video files as soon as possible while using the least possible network bandwidth. Which solution will meet these requirements?

- A. Create an S3 bucket. Create an IAM role that has permissions to write to the S3 bucket. Use the AWS CLI to copy all files locally to the S3 bucket.
- B. Create an AWS Snowball Edge job. Receive a Snowball Edge device on premises. Use the Snowball Edge client to transfer data to the device. Return the device so that AWS can import the data into Amazon S3.
- C. Deploy an S3 File Gateway on premises. Create a public service endpoint to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.
- D. Set up an AWS Direct Connect connection between the on-premises network and AWS. Deploy an S3 File Gateway on premises. Create a public virtual interface (VIF) to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To transfer 70 TB "as soon as possible" while using "least possible network bandwidth," AWS Snowball Edge is the best choice. It uses physical shipping, avoiding internet bandwidth usage entirely. Option A uses bandwidth; option C and D use the network (even with a File Gateway, the initial sync uses bandwidth). Snowball is designed for large offline data transfers (AWS Docs: AWS Snowball Edge).
</details>

---

**Q7.** A company has an application that ingests incoming messages. Dozens of other applications and microservices then quickly consume these messages. The number of messages varies drastically and sometimes increases suddenly to 100,000 each second. The company wants to decouple the solution and increase scalability. Which solution meets these requirements?

- A. Persist the messages to Amazon Kinesis Data Analytics. Configure the consumer applications to read and process the messages.
- B. Deploy the ingestion application on Amazon EC2 instances in an Auto Scaling group to scale the number of EC2 instances based on CPU metrics.
- C. Write the messages to Amazon Kinesis Data Streams with a single shard. Use an AWS Lambda function to preprocess messages and store them in Amazon DynamoDB. Configure the consumer applications to read from DynamoDB to process the messages.
- D. Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic with multiple Amazon Simple Queue Service (Amazon SQS) subscriptions. Configure the consumer applications to process the messages from the queues.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Decoupling with SNS and SQS allows the ingestion system to send one message that can be fanned out to dozens of consumer applications via their own queues. This handles high throughput (100k/sec) and variable traffic well. Option A (Kinesis Data Analytics) is for analytics, not message distribution; Option B doesn't decouple; Option C uses a single shard which limits throughput (AWS Docs: Fanout to SQS queues).
</details>

---

**Q8.** A company is migrating a distributed application to AWS. The application serves variable workloads. The legacy platform consists of a primary server that coordinates jobs across multiple compute nodes. The company wants to modernize the application with a solution that maximizes resiliency and scalability. How should a solutions architect design the architecture to meet these requirements?

- A. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling to use scheduled scaling.
- B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.
- C. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure AWS CloudTrail as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the primary server.
- D. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure Amazon EventBridge (Amazon CloudWatch Events) as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the compute nodes.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Using SQS to hold jobs and scaling the compute nodes (workers) based on the queue depth (ApproximateNumberOfMessages) is a classic resilient, scalable pattern. It eliminates the single point of failure (primary server). Scheduled scaling (A) doesn't handle variable workloads well; options C and D keep a primary server (AWS Docs: SQS as a buffer for dynamic scaling).
</details>

---

**Q9.** A company is running an SMB file server in its data center. The file server stores large files that are accessed frequently for the first few days after the files are created. After 7 days the files are rarely accessed. The total data size is increasing and is close to the company's total storage capacity. A solutions architect must increase the company's available storage space without losing low-latency access to the most recently accessed files. The solutions architect must also provide file lifecycle management to avoid future storage issues. Which solution will meet these requirements?

- A. Use AWS DataSync to copy data that is older than 7 days from the SMB file server to AWS.
- B. Create an Amazon S3 File Gateway to extend the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.
- C. Create an Amazon FSx for Windows File Server file system to extend the company's storage space.
- D. Install a utility on each user's computer to access Amazon S3. Create an S3 Lifecycle policy to transition the data to S3 Glacier Flexible Retrieval after 7 days.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 File Gateway provides a local NFS/SMB cache for low-latency access to recent files, while storing all data in S3. A lifecycle policy moves older files to cost-effective storage (Glacier Deep Archive). Option C (FSx) doesn't include automatic tiering to Glacier; Option D requires user changes (AWS Docs: S3 File Gateway).
</details>

---

**Q10.** A company is building an ecommerce web application on AWS. The application sends information about new orders to an Amazon API Gateway REST API to process. The company wants to ensure that orders are processed in the order that they are received. Which solution will meet these requirements?

- A. Use an API Gateway integration to publish a message to an Amazon Simple Notification Service (Amazon SNS) topic when the application receives an order. Subscribe an AWS Lambda function to the topic to perform processing.
- B. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order. Configure the SQS FIFO queue to invoke an AWS Lambda function for processing.
- C. Use an API Gateway authorizer to block any requests while the application processes an order.
- D. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) standard queue when the application receives an order. Configure the SQS standard queue to invoke an AWS Lambda function for processing.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To guarantee first-in, first-out (FIFO) processing, you must use an SQS FIFO queue. SNS (A) does not guarantee order; Standard queue (D) does not guarantee order; blocking requests (C) kills scalability (AWS Docs: SQS FIFO queues).
</details>

---

**Q11.** A company has an application that runs on Amazon EC2 instances and uses an Amazon Aurora database. The EC2 instances connect to the database by using user names and passwords that are stored locally in a file. The company wants to minimize the operational overhead of credential management. What should a solutions architect do to accomplish this goal?

- A. Use AWS Secrets Manager. Turn on automatic rotation.
- B. Use AWS Systems Manager Parameter Store. Turn on automatic rotation.
- C. Create an Amazon S3 bucket to store objects that are encrypted with an AWS Key Management Service (AWS KMS) encryption key. Migrate the credential file to the S3 bucket. Point the application to the S3 bucket.
- D. Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume for each EC2 instance. Attach the new EBS volume to each EC2 instance. Migrate the credential file to the new EBS volume. Point the application to the new EBS volume.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Secrets Manager is designed specifically for managing database credentials and other secrets, with built-in automatic rotation for Amazon Aurora and RDS. Parameter Store (B) can store secrets but has limited rotation capabilities; S3 (C) and EBS (D) add operational overhead and don't handle rotation well (AWS Docs: AWS Secrets Manager).
</details>

---

**Q12.** A global company hosts its web application on Amazon EC2 instances behind an Application Load Balancer (ALB). The web application has static data and dynamic data. The company stores its static data in an Amazon S3 bucket. The company wants to improve performance and reduce latency for the static data and dynamic data. The company is using its own domain name registered with Amazon Route 53. What should a solutions architect do to meet these requirements?

- A. Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins. Configure Route 53 to route traffic to the CloudFront distribution.
- B. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint. Configure Route 53 to route traffic to the CloudFront distribution.
- C. Create an Amazon CloudFront distribution that has the S3 bucket as an origin. Create an AWS Global Accelerator standard accelerator that has the ALB and the CloudFront distribution as endpoints. Create a custom domain name that points to the accelerator DNS name. Use the custom domain name as an endpoint for the web application.
- D. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint. Create two domain names. Point one domain name to the CloudFront DNS name for dynamic content. Point the other domain name to the accelerator DNS name for static content. Use the domain names as endpoints for the web application.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Amazon CloudFront can have multiple origins (S3 for static, ALB for dynamic) and intelligently route requests based on path patterns. This provides a single distribution for both content types, reducing latency via edge caching. Global Accelerator (B, C, D) is for TCP/UDP optimization, not static content caching (AWS Docs: CloudFront Multiple Origins).
</details>

---

**Q13.** A company performs monthly maintenance on its AWS infrastructure. During these maintenance activities, the company needs to rotate the credentials for its Amazon RDS for MySQL databases across multiple AWS Regions. Which solution will meet these requirements with the LEAST operational overhead?

- A. Store the credentials as secrets in AWS Secrets Manager. Use multi-Region secret replication for the required Regions. Configure Secrets Manager to rotate the secrets on a schedule.
- B. Store the credentials as secrets in AWS Systems Manager by creating a secure string parameter. Use multi-Region secret replication for the required Regions. Configure Systems Manager to rotate the secrets on a schedule.
- C. Store the credentials in an Amazon S3 bucket that has server-side encryption (SSE) enabled. Use Amazon EventBridge (Amazon CloudWatch Events) to invoke an AWS Lambda function to rotate the credentials.
- D. Encrypt the credentials as secrets by using AWS Key Management Service (AWS KMS) multi-Region customer managed keys. Store the secrets in an Amazon DynamoDB global table. Use an AWS Lambda function to retrieve the secrets from DynamoDB. Use the RDS API to rotate the secrets.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Secrets Manager natively supports multi-Region secret replication and automatic rotation for RDS databases, providing the least operational overhead. Parameter Store (B) does not support automatic rotation natively; options C and D require custom Lambda functions and infrastructure (AWS Docs: Replicate secrets to multiple Regions).
</details>

---

**Q14.** A company runs an ecommerce application on Amazon EC2 instances behind an Application Load Balancer. The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. The Auto Scaling group scales based on CPU utilization metrics. The ecommerce application stores the transaction data in a MySQL 8.0 database that is hosted on a large EC2 instance. The database's performance degrades quickly as application load increases. The application handles more read requests than write transactions. The company wants a solution that will automatically scale the database to meet the demand of unpredictable read workloads while maintaining high availability. Which solution will meet these requirements?

- A. Use Amazon Redshift with a single node for leader and compute functionality.
- B. Use Amazon RDS with a Single-AZ deployment. Configure Amazon RDS to add reader instances in a different Availability Zone.
- C. Use Amazon Aurora with a Multi-AZ deployment. Configure Aurora Auto Scaling with Aurora Replicas.
- D. Use Amazon ElastiCache for Memcached with EC2 Spot Instances.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon Aurora supports Auto Scaling of read replicas (Aurora Replicas) to handle unpredictable read workloads, and Multi-AZ provides high availability. Option B (RDS) does not auto-scale read replicas; Redshift (A) is for analytics; ElastiCache (D) is a cache, not a database (AWS Docs: Aurora Auto Scaling).
</details>

---

**Q15.** A company recently migrated to AWS and wants to implement a solution to protect the traffic that flows in and out of the production VPC. The company had an inspection server in its on-premises data center. The inspection server performed specific operations such as traffic flow inspection and traffic filtering. The company wants to have the same functionalities in the AWS Cloud. Which solution will meet these requirements?

- A. Use Amazon GuardDuty for traffic inspection and traffic filtering in the production VPC.
- B. Use Traffic Mirroring to mirror traffic from the production VPC for traffic inspection and filtering.
- C. Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.
- D. Use AWS Firewall Manager to create the required rules for traffic inspection and traffic filtering for the production VPC.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Network Firewall is a managed service that provides network traffic inspection and filtering (stateful/stateless) for VPCs. GuardDuty (A) is threat detection; Traffic Mirroring (B) is for copying traffic, not inspection; Firewall Manager (D) manages WAF/Shield/Network Firewall rules centrally but does not perform the inspection itself (AWS Docs: AWS Network Firewall).
</details>

---

**Q16.** A company hosts a data lake on AWS. The data lake consists of data in Amazon S3 and Amazon RDS for PostgreSQL. The company needs a reporting solution that provides data visualization and includes all the data sources within the data lake. Only the company's management team should have full access to all the visualizations. The rest of the company should have only limited access. Which solution will meet these requirements?

- A. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate IAM roles.
- B. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate users and groups.
- C. Create an AWS Glue table and crawler for the data in Amazon S3. Create an AWS Glue extract, transform, and load (ETL) job to produce reports. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.
- D. Create an AWS Glue table and crawler for the data in Amazon S3. Use Amazon Athena Federated Query to access data within Amazon RDS for PostgreSQL. Generate reports by using Amazon Athena. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon QuickSight is the correct visualization service. It supports connecting to multiple data sources (S3, RDS). Sharing dashboards with users/groups is the standard way to control access. Option A (IAM roles) is more complex; options C and D involve generating static reports rather than interactive dashboards (AWS Docs: QuickSight Sharing Dashboards).
</details>

---

**Q17.** A company is implementing a new business application. The application runs on two Amazon EC2 instances and uses an Amazon S3 bucket for document storage. A solutions architect needs to ensure that the EC2 instances can access the S3 bucket. What should the solutions architect do to meet this requirement?

- A. Create an IAM role that grants access to the S3 bucket. Attach the role to the EC2 instances.
- B. Create an IAM policy that grants access to the S3 bucket. Attach the policy to the EC2 instances.
- C. Create an IAM group that grants access to the S3 bucket. Attach the group to the EC2 instances.
- D. Create an IAM user that grants access to the S3 bucket. Attach the user account to the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* IAM roles are the correct way to grant permissions to EC2 instances. You cannot attach a policy (B), group (C), or user (D) directly to an EC2 instance. An Instance Profile is created for the role, which applications on the EC2 instance can assume (AWS Docs: IAM Roles for EC2).
</details>

---

**Q18.** An application development team is designing a microservice that will convert large images to smaller, compressed images. When a user uploads an image through the web interface, the microservice should store the image in an Amazon S3 bucket, process and compress the image with an AWS Lambda function, and store the image in its compressed form in a different S3 bucket. A solutions architect needs to design a solution that uses durable, stateless components to process the images automatically. Which combination of actions will meet these requirements? (Choose two.)

- A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.
- B. Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source. When the SQS message is successfully processed, delete the message in the queue.
- C. Configure the Lambda function to monitor the S3 bucket for new uploads. When an uploaded image is detected, write the file name to a text file in memory and use the text file to keep track of the images that were processed.
- D. Launch an Amazon EC2 instance to monitor an Amazon Simple Queue Service (Amazon SQS) queue. When items are added to the queue, log the file name in a text file on the EC2 instance and invoke the Lambda function.
- E. Configure an Amazon EventBridge (Amazon CloudWatch Events) event to monitor the S3 bucket. When an image is uploaded, send an alert to an Amazon Simple Notification Service (Amazon SNS) topic with the application owner's email address for further processing.

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* A and B together create an asynchronous, durable, and stateless architecture: S3 event -> SQS queue -> Lambda function. The queue decouples the components and provides durability if the Lambda function fails. Option C (monitoring) is polling and inefficient; D uses an EC2 instance (stateful); E uses SNS for email, not processing (AWS Docs: S3 Event Notifications to SQS, Lambda with SQS).
</details>

---

**Q19.** A company has a three-tier web application that is deployed on AWS. The web servers are deployed in a public subnet in a VPC. The application servers and database servers are deployed in private subnets in the same VPC. The company has deployed a third-party virtual firewall appliance from AWS Marketplace in an inspection VPC. The appliance is configured with an IP interface that can accept IP packets. A solutions architect needs to integrate the web application with the appliance to inspect all traffic to the application before the traffic reaches the web server. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create a Network Load Balancer in the public subnet of the applications VPC to route the traffic to the appliance for packet inspection.
- B. Create an Application Load Balancer in the public subnet of the applications VPC to route the traffic to the appliance for packet inspection.
- C. Deploy a transit gateway in the inspection VPC. Configure route tables to route the incoming packets through the transit gateway.
- D. Deploy a Gateway Load Balancer in the inspection VPC. Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Gateway Load Balancer (GWLB) is designed specifically for deploying and scaling third-party virtual appliances (firewalls, inspection). It uses GENEVE encapsulation and integrates with VPC endpoints to transparently route traffic through the appliance. Transit Gateway (C) is more complex for this use case; NLBs/ALBs (A/B) are not designed for this inline inspection pattern (AWS Docs: Gateway Load Balancer).
</details>

---

**Q20.** A company wants to improve its ability to clone large amounts of production data into a test environment in the same AWS Region. The data is stored in Amazon EC2 instances on Amazon Elastic Block Store (Amazon EBS) volumes. Modifications to the cloned data must not affect the production environment. The software that accesses this data requires consistently high I/O performance. A solutions architect needs to minimize the time that is required to clone the production data into the test environment. Which solution will meet these requirements?

- A. Take EBS snapshots of the production EBS volumes. Restore the snapshots onto EC2 instance store volumes in the test environment.
- B. Configure the production EBS volumes to use the EBS Multi-Attach feature. Take EBS snapshots of the production EBS volumes. Attach the production EBS volumes to the EC2 instances in the test environment.
- C. Take EBS snapshots of the production EBS volumes. Create and initialize new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment before restoring the volumes from the production EBS snapshots.
- D. Take EBS snapshots of the production EBS volumes. Turn on the EBS fast snapshot restore feature on the EBS snapshots. Restore the snapshots into new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* EBS Fast Snapshot Restore (FSR) ensures that volumes created from a snapshot are fully initialized at creation, eliminating the latency penalty of on-demand I/O initialization. This minimizes cloning time. Option A uses instance store (ephemeral); B (Multi-Attach) doesn't clone; C has initialization delay (AWS Docs: EBS Fast Snapshot Restore).
</details>

---

**Q21.** An ecommerce company wants to launch a one-deal-a-day website on AWS. Each day will feature exactly one product on sale for a period of 24 hours. The company wants to be able to handle millions of requests each hour with millisecond latency during peak hours. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon S3 to host the full website in different S3 buckets. Add Amazon CloudFront distributions. Set the S3 buckets as origins for the distributions. Store the order data in Amazon S3.
- B. Deploy the full website on Amazon EC2 instances that run in Auto Scaling groups across multiple Availability Zones. Add an Application Load Balancer (ALB) to distribute the website traffic. Add another ALB for the backend APIs. Store the data in Amazon RDS for MySQL.
- C. Migrate the full application to run in containers. Host the containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use the Kubernetes Cluster Autoscaler to increase and decrease the number of pods to process bursts in traffic. Store the data in Amazon RDS for MySQL.
- D. Use an Amazon S3 bucket to host the website's static content. Deploy an Amazon CloudFront distribution. Set the S3 bucket as the origin. Use Amazon API Gateway and AWS Lambda functions for the backend APIs. Store the data in Amazon DynamoDB.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* A serverless architecture (S3 for static hosting, CloudFront for CDN, API Gateway + Lambda for backend, DynamoDB for database) is the most cost-effective and operationally efficient way to handle millions of requests with millisecond latency. Option A stores order data in S3 (not suitable for transactional data); B and C have higher operational overhead (managing servers/containers) (AWS Docs: Serverless Web Application).
</details>

---

**Q22.** A solutions architect is using Amazon S3 to design the storage architecture of a new digital media application. The media files must be resilient to the loss of an Availability Zone. Some files are accessed frequently while other files are rarely accessed in an unpredictable pattern. The solutions architect must minimize the costs of storing and retrieving the media files. Which storage option meets these requirements?

- A. S3 Standard
- B. S3 Intelligent-Tiering
- C. S3 Standard-Infrequent Access (S3 Standard-IA)
- D. S3 One Zone-Infrequent Access (S3 One Zone-IA)

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Intelligent-Tiering automatically moves objects between frequent, infrequent, and archive instant access tiers based on changing access patterns, while providing AZ resilience. It is ideal for "unpredictable patterns" and minimizes cost. S3 Standard (A) is for frequent access; S3 Standard-IA (C) is for predictable infrequent access; S3 One Zone-IA (D) is not AZ resilient (AWS Docs: S3 Intelligent-Tiering).
</details>

---

**Q23.** A company is storing backup files by using Amazon S3 Standard storage. The files are accessed frequently for 1 month. However, the files are not accessed after 1 month. The company must keep the files indefinitely. Which storage solution will meet these requirements MOST cost-effectively?

- A. Configure S3 Intelligent-Tiering to automatically migrate objects.
- B. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.
- C. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 1 month.
- D. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 month.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Glacier Deep Archive is the lowest-cost storage class, suitable for data accessed very rarely (once or twice a year) and requiring long-term retention. Transitioning after 1 month from S3 Standard is cost-effective. Option A (Intelligent-Tiering) has monitoring costs; C and D (IA classes) are more expensive than Deep Archive for long-term, rarely accessed data (AWS Docs: S3 Lifecycle Transitions).
</details>

---

**Q24.** A company observes an increase in Amazon EC2 costs in its most recent bill. The billing team notices unwanted vertical scaling of instance types for a couple of EC2 instances. A solutions architect needs to create a graph comparing the last 2 months of EC2 costs and perform an in-depth analysis to identify the root cause of the vertical scaling. How should the solutions architect generate the information with the LEAST operational overhead?

- A. Use AWS Budgets to create a budget report and compare EC2 costs based on instance types.
- B. Use Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types.
- C. Use graphs from the AWS Billing and Cost Management dashboard to compare EC2 costs based on instance types for the last 2 months.
- D. Use AWS Cost and Usage Reports to create a report and send it to an Amazon S3 bucket. Use Amazon QuickSight with Amazon S3 as a source to generate an interactive graph based on instance types.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Cost Explorer provides built-in, interactive graphs and granular filtering (by instance type, time range) with no additional setup, making it the least operational overhead for this analysis. Budgets (A) are for alerts; Basic dashboard (C) has limited filtering; CUR + QuickSight (D) is overkill for a simple graph (AWS Docs: Cost Explorer).
</details>

---

**Q25.** A company is designing an application. The application uses an AWS Lambda function to receive information through Amazon API Gateway and to store the information in an Amazon Aurora PostgreSQL database. During the proof-of-concept stage, the company has to increase the Lambda quotas significantly to handle the high volumes of data that the company needs to load into the database. A solutions architect must recommend a new design to improve scalability and minimize the configuration effort. Which solution will meet these requirements?

- A. Refactor the Lambda function code to Apache Tomcat code that runs on Amazon EC2 instances. Connect the database by using native Java Database Connectivity (JDBC) drivers.
- B. Change the platform from Aurora to Amazon DynamoDB. Provision a DynamoDB Accelerator (DAX) cluster. Use the DAX client SDK to point the existing DynamoDB API calls at the DAX cluster.
- C. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using Amazon Simple Notification Service (Amazon SNS).
- D. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using an Amazon Simple Queue Service (Amazon SQS) queue.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Decoupling the ingestion Lambda from the database loading Lambda using an SQS queue allows the system to absorb bursts of traffic without throttling or hitting Lambda concurrency limits. The queue acts as a buffer. SNS (C) doesn't provide the same buffering and decoupling benefits for this use case. Option A increases operational overhead; Option B changes the database (AWS Docs: SQS as a buffer for Lambda).
</details>

---

**Q26.** A company needs to review its AWS Cloud deployment to ensure that its Amazon S3 buckets do not have unauthorized configuration changes. What should a solutions architect do to accomplish this goal?

- A. Turn on AWS Config with the appropriate rules.
- B. Turn on AWS Trusted Advisor with the appropriate checks.
- C. Turn on Amazon Inspector with the appropriate assessment template.
- D. Turn on Amazon S3 server access logging. Configure Amazon EventBridge (Amazon CloudWatch Events).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Config continuously records configuration changes and can evaluate compliance against desired rules (e.g., "s3-bucket-public-read-prohibited"). Trusted Advisor (B) is for best practice checks, not continuous compliance; Inspector (C) is for vulnerability scanning; S3 server access logging (D) is for access logs, not config changes (AWS Docs: AWS Config).
</details>

---

**Q27.** A company is launching a new application and will display application metrics on an Amazon CloudWatch dashboard. The company's product manager needs to access this dashboard periodically. The product manager does not have an AWS account. A solutions architect must provide access to the product manager by following the principle of least privilege. Which solution will meet these requirements?

- A. Share the dashboard from the CloudWatch console. Enter the product manager's email address, and complete the sharing steps. Provide a shareable link for the dashboard to the product manager.
- B. Create an IAM user specifically for the product manager. Attach the CloudWatchReadOnlyAccess AWS managed policy to the user. Share the new login credentials with the product manager. Share the browser URL of the correct dashboard with the product manager.
- C. Create an IAM user for the company's employees. Attach the ViewOnlyAccess AWS managed policy to the IAM user. Share the new login credentials with the product manager. Ask the product manager to navigate to the CloudWatch console and locate the dashboard by name in the Dashboards section.
- D. Deploy a bastion server in a public subnet. When the product manager requires access to the dashboard, start the server and share the RDP credentials. On the bastion server, ensure that the browser is configured to open the dashboard URL with cached AWS credentials that have appropriate permissions to view the dashboard.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudWatch dashboards can be shared directly with a public, shareable link (with optional password/expiration). This does not require the product manager to have an AWS account or IAM credentials, following the principle of least privilege and minimizing overhead. Options B and C require creating IAM users; Option D is highly complex (AWS Docs: Share CloudWatch Dashboards).
</details>

---

**Q28.** A company is migrating applications to AWS. The applications are deployed in different accounts. The company manages the accounts centrally by using AWS Organizations. The company's security team needs a single sign-on (SSO) solution across all the company's accounts. The company must continue managing the users and groups in its on-premises self-managed Microsoft Active Directory. Which solution will meet these requirements?

- A. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a one-way forest trust or a one-way domain trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
- B. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a two-way forest trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
- C. Use AWS Directory Service. Create a two-way trust relationship with the company's self-managed Microsoft Active Directory.
- D. Deploy an identity provider (IdP) on premises. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS SSO can connect to a self-managed Active Directory via AWS Directory Service for Microsoft AD (Managed Microsoft AD) and a one-way trust (on-premises AD is the authoritative source). This allows users to sign in to the AWS SSO user portal with their on-premises credentials. Two-way trust (B) is not required. Option C misses AWS SSO; Option D suggests an on-premises IdP, which is more work (AWS Docs: Connect AWS SSO to On-Premises Active Directory).
</details>

---

**Q29.** A company provides a Voice over Internet Protocol (VoIP) service that uses UDP connections. The service consists of Amazon EC2 instances that run in an Auto Scaling group. The company has deployments across multiple AWS Regions. The company needs to route users to the Region with the lowest latency. The company also needs automated failover between Regions. Which solution will meet these requirements?

- A. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Use the NLB as an AWS Global Accelerator endpoint in each Region.
- B. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Use the ALB as an AWS Global Accelerator endpoint in each Region.
- C. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 latency record that points to aliases for each NLB. Create an Amazon CloudFront distribution that uses the latency record as an origin.
- D. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 weighted record that points to aliases for each ALB. Deploy an Amazon CloudFront distribution that uses the weighted record as an origin.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Global Accelerator supports UDP traffic, provides static IP addresses, and automatically routes users to the optimal Region based on latency, with fast failover. NLBs are required for UDP traffic (ALBs only handle HTTP/HTTPS). Option C uses CloudFront, which does not support UDP as an origin protocol (AWS Docs: Global Accelerator - UDP).
</details>

---

**Q30.** A development team runs monthly resource-intensive tests on its general purpose Amazon RDS for MySQL DB instance with Performance Insights enabled. The testing lasts for 48 hours once a month and is the only process that uses the database. The team wants to reduce the cost of running the tests without reducing the compute and memory attributes of the DB instance. Which solution meets these requirements MOST cost-effectively?

- A. Stop the DB instance when tests are completed. Restart the DB instance when required.
- B. Use an Auto Scaling policy with the DB instance to automatically scale when tests are completed.
- C. Create a snapshot when tests are completed. Terminate the DB instance and restore the snapshot when required.
- D. Modify the DB instance to a low-capacity instance when tests are completed. Modify the DB instance again when required.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Since the instance is only used for 48 hours per month, the most cost-effective solution is to take a snapshot after the tests, terminate the instance (stop incurring compute costs), and restore from the snapshot when needed again. Stopping (A) still incurs storage costs for the provisioned instance (though less than running). Option D requires modifying instance type each time. Snapshotting and terminating is a common pattern for cost optimization (AWS Docs: RDS Snapshots).
</details>

---

**Q31.** A company that hosts its web application on AWS wants to ensure all Amazon EC2 instances, Amazon RDS DB instances, and Amazon Redshift clusters are configured with tags. The company wants to minimize the effort of configuring and operating this check. What should a solutions architect do to accomplish this?

- A. Use AWS Config rules to define and detect resources that are not properly tagged.
- B. Use Cost Explorer to display resources that are not properly tagged. Tag those resources manually.
- C. Write API calls to check all resources for proper tag allocation. Periodically run the code on an EC2 instance.
- D. Write API calls to check all resources for proper tag allocation. Schedule an AWS Lambda function through Amazon CloudWatch to periodically run the code.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Config provides managed rules (e.g., `required-tags`) that can continuously detect non-compliant resources with minimal setup. Options B, C, and D require manual effort or custom code that needs to be maintained. Config is the most operationally efficient for this compliance check (AWS Docs: AWS Config Managed Rules for Tags).
</details>

---

**Q32.** A development team needs to host a website that will be accessed by other teams. The website contents consist of HTML, CSS, client-side JavaScript, and images. Which method is the MOST cost-effective for hosting the website?

- A. Containerize the website and host it in AWS Fargate.
- B. Create an Amazon S3 bucket and host the website there.
- C. Deploy a web server on an Amazon EC2 instance to host the website.
- D. Configure an Application Load Balancer with an AWS Lambda target that uses the Express.js framework.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 static website hosting is the most cost-effective solution for static content (HTML/CSS/JS/images). You pay only for storage and data transfer. Fargate (A) and EC2 (C) require paying for compute resources even when idle. Lambda + ALB (D) is overkill and expensive for static files (AWS Docs: S3 Static Website Hosting).
</details>

---

**Q33.** A company runs an online marketplace web application on AWS. The application serves hundreds of thousands of users during peak hours. The company needs a scalable, near-real-time solution to share the details of millions of financial transactions with several other internal applications. Transactions also need to be processed to remove sensitive data before being stored in a document database for low-latency retrieval. What should a solutions architect recommend to meet these requirements?

- A. Store the transactions data into Amazon DynamoDB. Set up a rule in DynamoDB to remove sensitive data from every transaction upon write. Use DynamoDB Streams to share the transactions data with other applications.
- B. Stream the transactions data into Amazon Kinesis Data Firehose to store data in Amazon DynamoDB and Amazon S3. Use AWS Lambda integration with Kinesis Data Firehose to remove sensitive data. Other applications can consume the data stored in Amazon S3.
- C. Stream the transactions data into Amazon Kinesis Data Streams. Use AWS Lambda integration to remove sensitive data from every transaction and then store the transactions data in Amazon DynamoDB. Other applications can consume the transactions data off the Kinesis data stream.
- D. Store the batched transactions data in Amazon S3 as files. Use AWS Lambda to process every file and remove sensitive data before updating the files in Amazon S3. The Lambda function then stores the data in Amazon DynamoDB. Other applications can consume transaction files stored in Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Kinesis Data Streams provides near-real-time streaming, durable storage for up to 365 days, and the ability for multiple consumer applications to read the same stream at their own pace. Lambda can process the stream to remove sensitive data and write to DynamoDB. Option A (DynamoDB rule) isn't real; Option B (Firehose) is for delivery, not multiple consumers; Option D is batch, not near-real-time (AWS Docs: Kinesis Data Streams Multiple Consumers).
</details>

---

**Q34.** A company hosts its multi-tier applications on AWS. For compliance, governance, auditing, and security, the company must track configuration changes on its AWS resources and record a history of API calls made to these resources. What should a solutions architect do to meet these requirements?

- A. Use AWS CloudTrail to track configuration changes and AWS Config to record API calls.
- B. Use AWS Config to track configuration changes and AWS CloudTrail to record API calls.
- C. Use AWS Config to track configuration changes and Amazon CloudWatch to record API calls.
- D. Use AWS CloudTrail to track configuration changes and Amazon CloudWatch to record API calls.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Config tracks configuration changes (what changed, when, and how). AWS CloudTrail records API calls (who made the call, what action, from where). This pairing is standard for governance and auditing. Option A swaps the roles; CloudWatch (C/D) is for metrics and logs, not API call history (AWS Docs: AWS Config vs AWS CloudTrail).
</details>

---

**Q35.** A company is preparing to launch a public-facing web application in the AWS Cloud. The architecture consists of Amazon EC2 instances within a VPC behind an Elastic Load Balancer (ELB). A third-party service is used for the DNS. The company's solutions architect must recommend a solution to detect and protect against large-scale DDoS attacks. Which solution meets these requirements?

- A. Enable Amazon GuardDuty on the account.
- B. Enable Amazon Inspector on the EC2 instances.
- C. Enable AWS Shield and assign Amazon Route 53 to it.
- D. Enable AWS Shield Advanced and assign the ELB to it.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Shield Advanced provides enhanced DDoS protection for applications running on ELB, CloudFront, Global Accelerator, and Route 53. For a public-facing web app behind an ELB, Shield Advanced should be assigned to the ELB. GuardDuty (A) is threat detection, not DDoS mitigation; Inspector (B) is vulnerability scanning; Standard Shield (C) is free but less effective; assigning to Route 53 (C) is not enough (AWS Docs: Shield Advanced).
</details>

---

**Q36.** A company is building an application in the AWS Cloud. The application will store data in Amazon S3 buckets in two AWS Regions. The company must use an AWS Key Management Service (AWS KMS) customer managed key to encrypt all data that is stored in the S3 buckets. The data in both S3 buckets must be encrypted and decrypted with the same KMS key. The data and the key must be stored in each of the two Regions. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.
- B. Create a customer managed multi-Region KMS key. Create an S3 bucket in each Region. Configure replication between the S3 buckets. Configure the application to use the KMS key with client-side encryption.
- C. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.
- D. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with AWS KMS keys (SSE-KMS). Configure replication between the S3 buckets.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A multi-Region KMS key allows you to use the same key ID and material in multiple Regions. With S3 replication, objects encrypted with SSE-KMS in the source Region can be replicated to the destination Region using the multi-Region key. Option A/D use SSE-S3 or single-Region KMS keys; client-side encryption (B) is acceptable but the key is a multi-Region KMS key (AWS Docs: Multi-Region Keys for S3 Replication).
</details>

---

**Q37.** A company recently launched a variety of new workloads on Amazon EC2 instances in its AWS account. The company needs to create a strategy to access and administer the instances remotely and securely. The company needs to implement a repeatable process that works with native AWS services and follows the AWS Well-Architected Framework. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use the EC2 serial console to directly access the terminal interface of each instance for administration.
- B. Attach the appropriate IAM role to each existing instance and new instance. Use AWS Systems Manager Session Manager to establish a remote SSH session.
- C. Create an administrative SSH key pair. Load the public key into each EC2 instance. Deploy a bastion host in a public subnet to provide a tunnel for administration of each instance.
- D. Establish an AWS Site-to-Site VPN connection. Instruct administrators to use their local on-premises machines to connect directly to the instances by using SSH keys across the VPN tunnel.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Systems Manager Session Manager provides secure, auditable, and IAM-authenticated shell access to EC2 instances without needing open inbound ports, bastion hosts, or SSH keys. It is a best practice according to the Well-Architected Framework. Option A (serial console) is not for routine admin; C and D require bastion/VPN and SSH key management (AWS Docs: Session Manager).
</details>

---

**Q38.** A company is hosting a static website on Amazon S3 and is using Amazon Route 53 for DNS. The website is experiencing increased demand from around the world. The company must decrease latency for users who access the website. Which solution meets these requirements MOST cost-effectively?

- A. Replicate the S3 bucket that contains the website to all AWS Regions. Add Route 53 geolocation routing entries.
- B. Provision accelerators in AWS Global Accelerator. Associate the supplied IP addresses with the S3 bucket. Edit the Route 53 entries to point to the IP addresses of the accelerators.
- C. Add an Amazon CloudFront distribution in front of the S3 bucket. Edit the Route 53 entries to point to the CloudFront distribution.
- D. Enable S3 Transfer Acceleration on the bucket. Edit the Route 53 entries to point to the new endpoint.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* CloudFront caches content at edge locations globally, dramatically reducing latency for static website users. It is cost-effective and the standard solution for this use case. Option A (replicating to all Regions) is extremely expensive; Global Accelerator (B) is for TCP/UDP optimization, not caching; S3 Transfer Acceleration (D) speeds up uploads, not downloads (AWS Docs: CloudFront for S3 Static Websites).
</details>

---

**Q39.** A company maintains a searchable repository of items on its website. The data is stored in an Amazon RDS for MySQL database table that contains more than 10 million rows. The database has 2 TB of General Purpose SSD storage. There are millions of updates against this data every day through the company's website. The company has noticed that some insert operations are taking 10 seconds or longer. The company has determined that the database storage performance is the problem. Which solution addresses this performance issue?

- A. Change the storage type to Provisioned IOPS SSD.
- B. Change the DB instance to a memory optimized instance class.
- C. Change the DB instance to a burstable performance instance class.
- D. Enable Multi-AZ RDS read replicas with MySQL native asynchronous replication.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* General Purpose SSD (gp2/gp3) has baseline IOPS that may be insufficient for "millions of updates per day" causing insert latency. Provisioned IOPS (io1/io2) provides consistent, high IOPS performance suitable for demanding transactional workloads. Option B (memory) addresses caching, not IOPS; Option C (burstable) is worse; Option D (read replicas) helps reads, not writes (AWS Docs: RDS Storage Types).
</details>

---

**Q40.** A company has thousands of edge devices that collectively generate 1 TB of status alerts each day. Each alert is approximately 2 KB in size. A solutions architect needs to implement a solution to ingest and store the alerts for future analysis. The company wants a highly available solution. However, the company needs to minimize costs and does not want to manage additional infrastructure. Additionally, the company wants to keep 14 days of data available for immediate analysis and archive any data older than 14 days. What is the MOST operationally efficient solution that meets these requirements?

- A. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days.
- B. Launch Amazon EC2 instances across two Availability Zones and place them behind an Elastic Load Balancer to ingest the alerts. Create a script on the EC2 instances that will store the alerts in an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days.
- C. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster. Set up the Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster to take manual snapshots every day and delete data from the cluster that is older than 14 days.
- D. Create an Amazon Simple Queue Service (Amazon SQS) standard queue to ingest the alerts, and set the message retention period to 14 days. Configure consumers to poll the SQS queue, check the age of the message, and analyze the message data as needed. If the message is 14 days old, the consumer should copy the message to an Amazon S3 bucket and delete the message from the SQS queue.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Kinesis Data Firehose is a fully managed, serverless service for ingesting streaming data. It can deliver directly to S3. S3 Lifecycle policies handle the transition to Glacier after 14 days. This is highly available, cost-effective, and requires no infrastructure management. Options B (EC2), C (OpenSearch), and D (SQS with custom consumers) all require more operational overhead (AWS Docs: Kinesis Data Firehose to S3).
</details>

---

**Q41.** A company's application integrates with multiple software-as-a-service (SaaS) sources for data collection. The company runs Amazon EC2 instances to receive the data and to upload the data to an Amazon S3 bucket for analysis. The same EC2 instance that receives and uploads the data also sends a notification to the user when an upload is complete. The company has noticed slow application performance and wants to improve the performance as much as possible. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an Auto Scaling group so that EC2 instances can scale out. Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.
- B. Create an Amazon AppFlow flow to transfer data between each SaaS source and the S3 bucket. Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.
- C. Create an Amazon EventBridge (Amazon CloudWatch Events) rule for each SaaS source to send output data. Configure the S3 bucket as the rule's target. Create a second EventBridge (CloudWatch Events) rule to send events when the upload to the S3 bucket is complete. Configure an Amazon Simple Notification Service (Amazon SNS) topic as the second rule's target.
- D. Create a Docker container to use instead of an EC2 instance. Host the containerized application on Amazon Elastic Container Service (Amazon ECS). Configure Amazon CloudWatch Container Insights to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon AppFlow is a fully managed integration service designed to transfer data between SaaS applications (like Salesforce, Marketo, etc.) and AWS services (S3). It eliminates the need for EC2 instances entirely, reducing operational overhead and improving performance. Option A still uses EC2; Option C uses EventBridge with complex rules; Option D uses ECS (still compute management) (AWS Docs: Amazon AppFlow).
</details>

---

**Q42.** A company runs a highly available image-processing application on Amazon EC2 instances in a single VPC. The EC2 instances run inside several subnets across multiple Availability Zones. The EC2 instances do not communicate with each other. However, the EC2 instances download images from Amazon S3 and upload images to Amazon S3 through a single NAT gateway. The company is concerned about data transfer charges. What is the MOST cost-effective way for the company to avoid Regional data transfer charges?

- A. Launch the NAT gateway in each Availability Zone.
- B. Replace the NAT gateway with a NAT instance.
- C. Deploy a gateway VPC endpoint for Amazon S3.
- D. Provision an EC2 Dedicated Host to run the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* A gateway VPC endpoint for S3 allows traffic between the VPC and S3 to stay within the AWS network and does not incur NAT gateway data processing charges. The current architecture uses a NAT gateway which charges for data processing. Option A increases costs; Option B (NAT instance) still incurs EC2 costs; Option D (Dedicated Host) is unrelated (AWS Docs: VPC Endpoints - S3).
</details>

---

**Q43.** A company has an on-premises application that generates a large amount of time-sensitive data that is backed up to Amazon S3. The application has grown and there are user complaints about internet bandwidth limitations. A solutions architect needs to design a long-term solution that allows for both timely backups to Amazon S3 and with minimal impact on internet connectivity for internal users. Which solution meets these requirements?

- A. Establish AWS VPN connections and proxy all traffic through a VPC gateway endpoint.
- B. Establish a new AWS Direct Connect connection and direct backup traffic through this new connection.
- C. Order daily AWS Snowball devices. Load the data onto the Snowball devices and return the devices to AWS each day.
- D. Submit a support ticket through the AWS Management Console. Request the removal of S3 service limits from the account.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Direct Connect provides a dedicated private network connection from on-premises to AWS, bypassing the public internet. This solves bandwidth limitations and provides consistent, timely backups without impacting internet users. Snowball (C) is for offline transfer, not "timely backups". VPN (A) still uses internet. Support ticket (D) is irrelevant (AWS Docs: AWS Direct Connect).
</details>

---

**Q44.** A company has an Amazon S3 bucket that contains critical data. The company must protect the data from accidental deletion. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- A. Enable versioning on the S3 bucket.
- B. Enable MFA Delete on the S3 bucket.
- C. Create a bucket policy on the S3 bucket.
- D. Enable default encryption on the S3 bucket.
- E. Create a lifecycle policy for the objects in the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* Versioning allows you to restore previous versions of an object if it is accidentally deleted or overwritten. MFA Delete adds an extra layer of security, requiring MFA authentication to change bucket versioning state or permanently delete object versions. These two features directly address accidental deletion. Option C (bucket policy) controls access; D (encryption) protects confidentiality; E (lifecycle) manages transitions/expiration (AWS Docs: S3 Versioning, MFA Delete).
</details>

---

**Q45.** A company has a data ingestion workflow that consists of the following: - An Amazon Simple Notification Service (Amazon SNS) topic for notifications about new data deliveries - An AWS Lambda function to process the data and record metadata. The company observes that the ingestion workflow fails occasionally because of network connectivity issues. When such a failure occurs, the Lambda function does not ingest the corresponding data unless the company manually reruns the job. Which combination of actions should a solutions architect take to ensure that the Lambda function ingests all data in the future? (Choose two.)

- A. Deploy the Lambda function in multiple Availability Zones.
- B. Create an Amazon Simple Queue Service (Amazon SQS) queue, and subscribe it to the SNS topic.
- C. Increase the CPU and memory that are allocated to the Lambda function.
- D. Increase provisioned throughput for the Lambda function.
- E. Modify the Lambda function to read from an Amazon Simple Queue Service (Amazon SQS) queue.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* To handle transient failures (network issues) and ensure no data is lost, decouple the SNS notification from the Lambda function using an SQS queue. The SNS topic sends messages to the SQS queue (B), and the Lambda function reads from the queue (E). This provides durability and retries. Options A, C, D do not address the fundamental issue of lost notifications during network blips (AWS Docs: SNS to SQS to Lambda).
</details>

---

**Q46.** A company has an application that provides marketing services to stores. The services are based on previous purchases by store customers. The stores upload transaction data to the company through SFTP, and the data is processed and analyzed to generate new marketing offers. Some of the files can exceed 200 GB in size. Recently, the company discovered that some of the stores have uploaded files that contain personally identifiable information (PII) that should not have been included. The company wants administrators to be alerted if PII is shared again. The company also wants to automate remediation. What should a solutions architect do to meet these requirements with the LEAST development effort?

- A. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Inspector to scan the objects in the bucket. If objects contain PII, trigger an S3 Lifecycle policy to remove the objects that contain PII.
- B. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Macie to scan the objects in the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.
- C. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.
- D. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Email Service (Amazon SES) to trigger a notification to the administrators and trigger an S3 Lifecycle policy to remove the objects that contain PII.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon Macie is a fully managed service that uses machine learning to automatically discover, classify, and protect sensitive data (PII) in S3. It has built-in integrations with SNS for alerts. This requires the least development effort. Inspector (A) is for vulnerability scanning. Options C and D require custom Lambda code (development effort) (AWS Docs: Amazon Macie).
</details>

---

**Q47.** A company needs guaranteed Amazon EC2 capacity in three specific Availability Zones in a specific AWS Region for an upcoming event that will last 1 week. What should the company do to guarantee the EC2 capacity?

- A. Purchase Reserved Instances that specify the Region needed.
- B. Create an On-Demand Capacity Reservation that specifies the Region needed.
- C. Purchase Reserved Instances that specify the Region and three Availability Zones needed.
- D. Create an On-Demand Capacity Reservation that specifies the Region and three Availability Zones needed.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* On-Demand Capacity Reservations allow you to reserve compute capacity for a specific Availability Zone for any duration, with no long-term commitment. This guarantees capacity for the 1-week event. Reserved Instances (A/C) provide a billing discount but do not guarantee capacity unless you also create a Capacity Reservation. Option D is the direct solution (AWS Docs: On-Demand Capacity Reservations).
</details>

---

**Q48.** A company's website uses an Amazon EC2 instance store for its catalog of items. The company wants to make sure that the catalog is highly available and that the catalog is stored in a durable location. What should a solutions architect do to meet these requirements?

- A. Move the catalog to Amazon ElastiCache for Redis.
- B. Deploy a larger EC2 instance with a larger instance store.
- C. Move the catalog from the instance store to Amazon S3 Glacier Deep Archive.
- D. Move the catalog to an Amazon Elastic File System (Amazon EFS) file system.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Instance store volumes are ephemeral (data lost on instance stop/termination). Amazon EFS provides a durable, highly available, shared file system that can be mounted by multiple EC2 instances, ensuring the catalog is persistent and available. ElastiCache (A) is in-memory cache; S3 Glacier (C) is for archival, not active catalog access (AWS Docs: Amazon EFS).
</details>

---

**Q49.** A company stores call transcript files on a monthly basis. Users access the files randomly within 1 year of the call, but users access the files infrequently after 1 year. The company wants to optimize its solution by giving users the ability to query and retrieve files that are less than 1-year-old as quickly as possible. A delay in retrieving older files is acceptable. Which solution will meet these requirements MOST cost-effectively?

- A. Store individual files with tags in Amazon S3 Glacier Instant Retrieval. Query the tags to retrieve the files from S3 Glacier Instant Retrieval.
- B. Store individual files in Amazon S3 Intelligent-Tiering. Use S3 Lifecycle policies to move the files to S3 Glacier Flexible Retrieval after 1 year. Query and retrieve the files that are in Amazon S3 by using Amazon Athena. Query and retrieve the files that are in S3 Glacier by using S3 Glacier Select.
- C. Store individual files with tags in Amazon S3 Standard storage. Store search metadata for each archive in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Instant Retrieval after 1 year. Query and retrieve the files by searching for metadata from Amazon S3.
- D. Store individual files in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Deep Archive after 1 year. Store search metadata in Amazon RDS. Query the files from Amazon RDS. Retrieve the files from S3 Glacier Deep Archive.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Intelligent-Tiering automatically optimizes costs for files accessed within the first year. Moving to Glacier Flexible Retrieval after 1 year balances cost with acceptable retrieval delay (minutes to hours). Athena can query S3 data; S3 Glacier Select allows querying archived data without full retrieval. Option A (Instant Retrieval) is more expensive; Option C (Standard -> Instant) is not optimal; Option D (Deep Archive) has higher retrieval delay and uses RDS (overhead) (AWS Docs: S3 Lifecycle with Glacier).
</details>

---

**Q50.** A company has a production workload that runs on 1,000 Amazon EC2 Linux instances. The workload is powered by third-party software. The company needs to patch the third-party software on all EC2 instances as quickly as possible to remediate a critical security vulnerability. What should a solutions architect do to meet these requirements?

- A. Create an AWS Lambda function to apply the patch to all EC2 instances.
- B. Configure AWS Systems Manager Patch Manager to apply the patch to all EC2 instances.
- C. Schedule an AWS Systems Manager maintenance window to apply the patch to all EC2 instances.
- D. Use AWS Systems Manager Run Command to run a custom command that applies the patch to all EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Systems Manager Run Command allows you to remotely and securely run a custom command (e.g., a script to patch third-party software) on a fleet of managed EC2 instances on-demand, without needing a schedule or predefined patch baseline. This is the fastest way to remediate a critical vulnerability across 1,000 instances. Patch Manager (B) is for OS patches, not custom third-party software; Maintenance Window (C) requires scheduling (AWS Docs: Systems Manager Run Command).
</details>

---

**Q51.** A company is developing an application that provides order shipping statistics for retrieval by a REST API. The company wants to extract the shipping statistics, organize the data into an easy-to-read HTML format, and send the report to several email addresses at the same time every morning. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- A. Configure the application to send the data to Amazon Kinesis Data Firehose.
- B. Use Amazon Simple Email Service (Amazon SES) to format the data and to send the report by email.
- C. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Glue job to query the application's API for the data.
- D. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Lambda function to query the application's API for the data.
- E. Store the application data in Amazon S3. Create an Amazon Simple Notification Service (Amazon SNS) topic as an S3 event destination to send the report by email.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* A scheduled EventBridge rule (D) can invoke a Lambda function daily to query the API, fetch the data, and format it into HTML. Amazon SES (B) is the email service for sending the formatted report to multiple addresses. Option A (Firehose) is for streaming; Option C (Glue) is heavy for this task; Option E (S3 event) is not scheduled (AWS Docs: EventBridge Scheduled Events, Amazon SES).
</details>

---

**Q52.** A company wants to migrate its on-premises application to AWS. The application produces output files that vary in size from tens of gigabytes to hundreds of terabytes. The application data must be stored in a standard file system structure. The company wants a solution that scales automatically, is highly available, and requires minimum operational overhead. Which solution will meet these requirements?

- A. Migrate the application to run as containers on Amazon Elastic Container Service (Amazon ECS). Use Amazon S3 for storage.
- B. Migrate the application to run as containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use Amazon Elastic Block Store (Amazon EBS) for storage.
- C. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic File System (Amazon EFS) for storage.
- D. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic Block Store (Amazon EBS) for storage.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon EFS is a fully managed, highly available, and scalable file system that grows and shrinks automatically. It provides a standard file system structure and can be mounted on multiple EC2 instances in a Multi-AZ Auto Scaling group. EBS (B/D) cannot be shared across instances easily; S3 (A) is object storage, not a standard file system (AWS Docs: Amazon EFS).
</details>

---

**Q53.** A company needs to store its accounting records in Amazon S3. The records must be immediately accessible for 1 year and then must be archived for an additional 9 years. No one at the company, including administrative users and root users, can be able to delete the records during the entire 10-year period. The records must be stored with maximum resiliency. Which solution will meet these requirements?

- A. Store the records in S3 Glacier for the entire 10-year period. Use an access control policy to deny deletion of the records for a period of 10 years.
- B. Store the records by using S3 Intelligent-Tiering. Use an IAM policy to deny deletion of the records. After 10 years, change the IAM policy to allow deletion.
- C. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 Glacier Deep Archive after 1 year. Use S3 Object Lock in compliance mode for a period of 10 years.
- D. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 year. Use S3 Object Lock in governance mode for a period of 10 years.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* S3 Object Lock in compliance mode ensures that no user (including root) can delete or overwrite objects for the specified retention period (10 years). A lifecycle policy transitions from Standard (for immediate access in year 1) to Glacier Deep Archive (low-cost archival for years 2-10). Option A (Glacier only) is not "immediately accessible"; Option B (IAM policy) can be changed by admins; Option D (One Zone-IA) is not maximally resilient (AWS Docs: S3 Object Lock Compliance Mode).
</details>

---

**Q54.** A company runs multiple Windows workloads on AWS. The company's employees use Windows file shares that are hosted on two Amazon EC2 instances. The file shares synchronize data between themselves and maintain duplicate copies. The company wants a highly available and durable storage solution that preserves how users currently access the files. What should a solutions architect do to meet these requirements?

- A. Migrate all the data to Amazon S3. Set up IAM authentication for users to access files.
- B. Set up an Amazon S3 File Gateway. Mount the S3 File Gateway on the existing EC2 instances.
- C. Extend the file share environment to Amazon FSx for Windows File Server with a Multi-AZ configuration. Migrate all the data to FSx for Windows File Server.
- D. Extend the file share environment to Amazon Elastic File System (Amazon EFS) with a Multi-AZ configuration. Migrate all the data to Amazon EFS.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon FSx for Windows File Server provides fully managed, highly available, and durable Windows file shares (SMB protocol) with Multi-AZ support. It is the native AWS replacement for Windows file servers. S3 (A) is object storage; S3 File Gateway (B) is a hybrid solution; EFS (D) is NFS, not SMB (Windows) (AWS Docs: Amazon FSx for Windows File Server).
</details>

---

**Q55.** A solutions architect is developing a VPC architecture that includes multiple subnets. The architecture will host applications that use Amazon EC2 instances and Amazon RDS DB instances. The architecture consists of six subnets in two Availability Zones. Each Availability Zone includes a public subnet, a private subnet, and a dedicated subnet for databases. Only EC2 instances that run in the private subnets can have access to the RDS databases. Which solution will meet these requirements?

- A. Create a new route table that excludes the route to the public subnets' CIDR blocks. Associate the route table with the database subnets.
- B. Create a security group that denies inbound traffic from the security group that is assigned to instances in the public subnets. Attach the security group to the DB instances.
- C. Create a security group that allows inbound traffic from the security group that is assigned to instances in the private subnets. Attach the security group to the DB instances.
- D. Create a new peering connection between the public subnets and the private subnets. Create a different peering connection between the private subnets and the database subnets.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Security groups are stateful and control traffic at the instance level. By creating a security group for the RDS DB instances that allows inbound traffic only from the security group attached to the private EC2 instances, you meet the requirement. Route tables (A) and peering (D) are not appropriate; denying traffic (B) is not the correct way to allow specific traffic (AWS Docs: Security Group Rules).
</details>

---

**Q56.** A company has registered its domain name with Amazon Route 53. The company uses Amazon API Gateway in the ca-central-1 Region as a public interface for its backend microservice APIs. Third-party services consume the APIs securely. The company wants to design its API Gateway URL with the company's domain name and corresponding certificate so that the third-party services can use HTTPS. Which solution will meet these requirements?

- A. Create stage variables in API Gateway with Name="Endpoint-URL" and Value="Company Domain Name" to overwrite the default URL. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM).
- B. Create Route 53 DNS records with the company's domain name. Point the alias record to the Regional API Gateway stage endpoint. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region.
- C. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the same Region. Attach the certificate to the API Gateway endpoint. Configure Route 53 to route traffic to the API Gateway endpoint.
- D. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region. Attach the certificate to the API Gateway endpoint. Create Route 53 DNS records with the company's domain name. Point an A record to the company's domain name.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* For a Regional API Gateway endpoint, you must import the SSL/TLS certificate into ACM in the same Region (ca-central-1). Then, configure a custom domain name for the API, attach the certificate, and create a Route 53 alias record pointing to the API Gateway's regional domain name. Option B uses us-east-1 for ACM (only required for Edge-optimized endpoints). (AWS Docs: API Gateway Custom Domain Names).
</details>

---

**Q57.** A company is running a popular social media website. The website gives users the ability to upload images to share with other users. The company wants to make sure that the images do not contain inappropriate content. The company needs a solution that minimizes development effort. What should a solutions architect do to meet these requirements?

- A. Use Amazon Comprehend to detect inappropriate content. Use human review for low-confidence predictions.
- B. Use Amazon Rekognition to detect inappropriate content. Use human review for low-confidence predictions.
- C. Use Amazon SageMaker to detect inappropriate content. Use ground truth to label low-confidence predictions.
- D. Use AWS Fargate to deploy a custom machine learning model to detect inappropriate content. Use ground truth to label low-confidence predictions.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon Rekognition has built-in moderation APIs to detect inappropriate content in images. It minimizes development effort compared to building custom models (C, D). Comprehend (A) is for text. Rekognition is the correct managed service for image moderation (AWS Docs: Amazon Rekognition Moderation).
</details>

---

**Q58.** A company wants to run its critical applications in containers to meet requirements for scalability and availability. The company prefers to focus on maintenance of the critical applications. The company does not want to be responsible for provisioning and managing the underlying infrastructure that runs the containerized workload. What should a solutions architect do to meet these requirements?

- A. Use Amazon EC2 instances, and install Docker on the instances.
- B. Use Amazon Elastic Container Service (Amazon ECS) on Amazon EC2 worker nodes.
- C. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.
- D. Use Amazon EC2 instances from an Amazon Elastic Container Service (Amazon ECS)-optimized Amazon Machine Image (AMI).

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Fargate is a serverless compute engine for containers that removes the need to provision, configure, or manage EC2 instances. This allows the company to focus on the applications, not the infrastructure. Options A, B, and D all require managing EC2 instances (AWS Docs: AWS Fargate).
</details>

---

**Q59.** A company hosts more than 300 global websites and applications. The company requires a platform to analyze more than 30 TB of clickstream data each day. What should a solutions architect do to transmit and process the clickstream data?

- A. Design an AWS Data Pipeline to archive the data to an Amazon S3 bucket and run an Amazon EMR cluster with the data to generate analytics.
- B. Create an Auto Scaling group of Amazon EC2 instances to process the data and send it to an Amazon S3 data lake for Amazon Redshift to use for analysis.
- C. Cache the data to Amazon CloudFront. Store the data in an Amazon S3 bucket. When an object is added to the S3 bucket, run an AWS Lambda function to process the data for analysis.
- D. Collect the data from Amazon Kinesis Data Streams. Use Amazon Kinesis Data Firehose to transmit the data to an Amazon S3 data lake. Load the data in Amazon Redshift for analysis.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* For large-scale (30 TB/day) clickstream data, Kinesis Data Streams for ingestion, Kinesis Data Firehose for delivery to S3, and Redshift for analysis is a standard, scalable, and managed pipeline. Option A uses Data Pipeline (legacy); Option B requires managing EC2; Option C uses CloudFront (CDN) incorrectly (AWS Docs: Kinesis to Redshift).
</details>

---

**Q60.** A company has a website hosted on AWS. The website is behind an Application Load Balancer (ALB) that is configured to handle HTTP and HTTPS separately. The company wants to forward all requests to the website so that the requests will use HTTPS. What should a solutions architect do to meet this requirement?

- A. Update the ALB's network ACL to accept only HTTPS traffic.
- B. Create a rule that replaces the HTTP in the URL with HTTPS.
- C. Create a listener rule on the ALB to redirect HTTP traffic to HTTPS.
- D. Replace the ALB with a Network Load Balancer configured to use Server Name Indication (SNI).

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Application Load Balancers support native HTTP to HTTPS redirect actions via listener rules. This is the standard, correct way to enforce HTTPS. Network ACLs (A) cannot perform redirects; URL replacement (B) is not a load balancer feature; NLB (D) is not needed (AWS Docs: ALB HTTP to HTTPS Redirect).
</details>

---

**Q61.** A company is developing a two-tier web application on AWS. The company's developers have deployed the application on an Amazon EC2 instance that connects directly to a backend Amazon RDS database. The company must not hardcode database credentials in the application. The company must also implement a solution to automatically rotate the database credentials on a regular basis. Which solution will meet these requirements with the LEAST operational overhead?

- A. Store the database credentials in the instance metadata. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and instance metadata at the same time.
- B. Store the database credentials in a configuration file in an encrypted Amazon S3 bucket. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and the credentials in the configuration file at the same time. Use S3 Versioning to ensure the ability to fall back to previous values.
- C. Store the database credentials as a secret in AWS Secrets Manager. Turn on automatic rotation for the secret. Attach the required permission to the EC2 role to grant access to the secret.
- D. Store the database credentials as encrypted parameters in AWS Systems Manager Parameter Store. Turn on automatic rotation for the encrypted parameters. Attach the required permission to the EC2 role to grant access to the encrypted parameters.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Secrets Manager is designed for this exact use case: storing database credentials, automatically rotating them (with native RDS integration), and providing secure access via IAM roles. Parameter Store (D) does not have native automatic rotation. Options A and B require custom Lambda functions (more overhead) (AWS Docs: Secrets Manager Rotation for RDS).
</details>

---

**Q62.** A company is deploying a new public web application to AWS. The application will run behind an Application Load Balancer (ALB). The application needs to be encrypted at the edge with an SSL/TLS certificate that is issued by an external certificate authority (CA). The certificate must be rotated each year before the certificate expires. What should a solutions architect do to meet these requirements?

- A. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
- B. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Import the key material from the certificate. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
- C. Use AWS Certificate Manager (ACM) Private Certificate Authority to issue an SSL/TLS certificate from the root CA. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
- D. Use AWS Certificate Manager (ACM) to import an SSL/TLS certificate. Apply the certificate to the ALB. Use Amazon EventBridge (Amazon CloudWatch Events) to send a notification when the certificate is nearing expiration. Rotate the certificate manually.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Since the certificate is issued by an external CA, ACM cannot automatically renew it. You must import the certificate into ACM, then set up monitoring (e.g., EventBridge) to alert before expiration, and manually rotate (re-import) the new certificate each year. Options A, B, C involve ACM-issued or managed certificates (AWS Docs: Importing Certificates into ACM).
</details>

---

**Q63.** A company runs its infrastructure on AWS and has a registered base of 700,000 users for its document management application. The company intends to create a product that converts large .pdf files to .jpg image files. The .pdf files average 5 MB in size. The company needs to store the original files and the converted files. A solutions architect must design a scalable solution to accommodate demand that will grow rapidly over time. Which solution meets these requirements MOST cost-effectively?

- A. Save the .pdf files to Amazon S3. Configure an S3 PUT event to invoke an AWS Lambda function to convert the files to .jpg format and store them back in Amazon S3.
- B. Save the .pdf files to Amazon DynamoDB. Use the DynamoDB Streams feature to invoke an AWS Lambda function to convert the files to .jpg format and store them back in DynamoDB.
- C. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic Block Store (Amazon EBS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the files to .jpg format. Save the .pdf files and the .jpg files in the EBS store.
- D. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic File System (Amazon EFS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the file to .jpg format. Save the .pdf files and the .jpg files in the EBS store.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Using S3 for storage and Lambda for serverless PDF-to-JPG conversion is highly scalable, cost-effective (pay per invocation/storage), and requires no infrastructure management. Storing files in DynamoDB (B) is expensive and inefficient for binary data; EC2/EBS-based solutions (C, D) have higher operational overhead and cost (AWS Docs: S3 + Lambda for image processing).
</details>

---

**Q64.** A company has more than 5 TB of file data on Windows file servers that run on premises. Users and applications interact with the data each day. The company is moving its Windows workloads to AWS. As the company continues this process, the company requires access to AWS and on-premises file storage with minimum latency. The company needs a solution that minimizes operational overhead and requires no significant changes to the existing file access patterns. The company uses an AWS Site-to-Site VPN connection for connectivity to AWS. What should a solutions architect do to meet these requirements?

- A. Deploy and configure Amazon FSx for Windows File Server on AWS. Move the on-premises file data to FSx for Windows File Server. Reconfigure the workloads to use FSx for Windows File Server on AWS.
- B. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to the S3 File Gateway. Reconfigure the on-premises workloads and the cloud workloads to use the S3 File Gateway.
- C. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to Amazon S3. Reconfigure the workloads to use either Amazon S3 directly or the S3 File Gateway, depending on each workload's location.
- D. Deploy and configure Amazon FSx for Windows File Server on AWS. Deploy and configure an Amazon FSx File Gateway on premises. Move the on-premises file data to the FSx File Gateway. Configure the cloud workloads to use FSx for Windows File Server on AWS. Configure the on-premises workloads to use the FSx File Gateway.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* To provide low-latency access from both on-premises and AWS during migration, you need an FSx File Gateway on-premises (caching recent data locally) and FSx for Windows File Server in AWS (central data store). This allows workloads in both locations to access the same data with minimal latency and no major changes to access patterns (AWS Docs: FSx File Gateway).
</details>

---

**Q65.** A hospital recently deployed a RESTful API with Amazon API Gateway and AWS Lambda. The hospital uses API Gateway and Lambda to upload reports that are in PDF format and JPEG format. The hospital needs to modify the Lambda code to identify protected health information (PHI) in the reports. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use existing Python libraries to extract the text from the reports and to identify the PHI from the extracted text.
- B. Use Amazon Textract to extract the text from the reports. Use Amazon SageMaker to identify the PHI from the extracted text.
- C. Use Amazon Textract to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.
- D. Use Amazon Rekognition to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon Textract extracts text and data from documents (PDF/JPEG). Amazon Comprehend Medical is a HIPAA-eligible service specifically designed to detect PHI in medical text. This combination minimizes development effort compared to custom libraries (A) or building models (B). Rekognition (D) is for image analysis, not document text extraction (AWS Docs: Amazon Comprehend Medical, Amazon Textract).
</details>

---

**Q66.** A company has an application that generates a large number of files, each approximately 5 MB in size. The files are stored in Amazon S3. Company policy requires the files to be stored for 4 years before they can be deleted. Immediate accessibility is always required as the files contain critical business data that is not easy to reproduce. The files are frequently accessed in the first 30 days of the object creation but are rarely accessed after the first 30 days. Which storage solution is MOST cost-effective?

- A. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Glacier 30 days from object creation. Delete the files 4 years after object creation.
- B. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) 30 days from object creation. Delete the files 4 years after object creation.
- C. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Delete the files 4 years after object creation.
- D. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Move the files to S3 Glacier 4 years after object creation.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* The files need immediate accessibility for 4 years, so they cannot be moved to Glacier (which has retrieval delays). S3 Standard-IA is cheaper than S3 Standard and suitable for infrequent access after 30 days, while still providing immediate access. Option B (One Zone-IA) is not as durable; Option A/D use Glacier which does not meet "immediate accessibility" requirement (AWS Docs: S3 Storage Classes).
</details>

---

**Q67.** A company hosts an application on multiple Amazon EC2 instances. The application processes messages from an Amazon SQS queue, writes to an Amazon RDS table, and deletes the message from the queue. Occasional duplicate records are found in the RDS table. The SQS queue does not contain any duplicate messages. What should a solutions architect do to ensure messages are being processed once only?

- A. Use the CreateQueue API call to create a new queue.
- B. Use the AddPermission API call to add appropriate permissions.
- C. Use the ReceiveMessage API call to set an appropriate wait time.
- D. Use the ChangeMessageVisibility API call to increase the visibility timeout.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Duplicate processing in SQS standard queues often occurs when a message is processed but not deleted before the visibility timeout expires, causing the message to become visible again and be reprocessed. Increasing the visibility timeout ensures the consumer has enough time to process the message and delete it before it becomes visible again. (AWS Docs: SQS Visibility Timeout).
</details>

---

**Q68.** A solutions architect is designing a new hybrid architecture to extend a company's on-premises infrastructure to AWS. The company requires a highly available connection with consistent low latency to an AWS Region. The company needs to minimize costs and is willing to accept slower traffic if the primary connection fails. What should the solutions architect do to meet these requirements?

- A. Provision an AWS Direct Connect connection to a Region. Provision a VPN connection as a backup if the primary Direct Connect connection fails.
- B. Provision a VPN tunnel connection to a Region for private connectivity. Provision a second VPN tunnel for private connectivity and as a backup if the primary VPN connection fails.
- C. Provision an AWS Direct Connect connection to a Region. Provision a second Direct Connect connection to the same Region as a backup if the primary Direct Connect connection fails.
- D. Provision an AWS Direct Connect connection to a Region. Use the Direct Connect failover attribute from the AWS CLI to automatically create a backup connection if the primary Direct Connect connection fails.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Direct Connect provides consistent low latency. A VPN connection over the internet can serve as a lower-cost backup. This meets the requirements of low latency for primary and cost minimization with slower failover. Option B (VPN only) doesn't provide consistent low latency; Option C (second DX) is more expensive; Option D (failover attribute) doesn't exist (AWS Docs: Direct Connect with VPN Backup).
</details>

---

**Q69.** A company is running a business-critical web application on Amazon EC2 instances behind an Application Load Balancer. The EC2 instances are in an Auto Scaling group. The application uses an Amazon Aurora PostgreSQL database that is deployed in a single Availability Zone. The company wants the application to be highly available with minimum downtime and minimum loss of data. Which solution will meet these requirements with the LEAST operational effort?

- A. Place the EC2 instances in different AWS Regions. Use Amazon Route 53 health checks to redirect traffic. Use Aurora PostgreSQL Cross-Region Replication.
- B. Configure the Auto Scaling group to use multiple Availability Zones. Configure the database as Multi-AZ. Configure an Amazon RDS Proxy instance for the database.
- C. Configure the Auto Scaling group to use one Availability Zone. Generate hourly snapshots of the database. Recover the database from the snapshots in the event of a failure.
- D. Configure the Auto Scaling group to use multiple AWS Regions. Write the data from the application to Amazon S3. Use S3 Event Notifications to launch an AWS Lambda function to write the data to the database.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* For high availability with minimum downtime and data loss, use Multi-AZ for the Auto Scaling group and Multi-AZ for Aurora (which automatically replicates data to a standby in another AZ). RDS Proxy adds connection management but is not strictly required. Option A (Cross-Region) adds complexity; Option C (snapshots) has higher RTO/RPO; Option D (S3 + Lambda) is overly complex (AWS Docs: Aurora Multi-AZ).
</details>

---

**Q70.** A company's HTTP application is behind a Network Load Balancer (NLB). The NLB's target group is configured to use an Amazon EC2 Auto Scaling group with multiple EC2 instances that run the web service. The company notices that the NLB is not detecting HTTP errors for the application. These errors require a manual restart of the EC2 instances that run the web service. The company needs to improve the application's availability without writing custom scripts or code. What should a solutions architect do to meet these requirements?

- A. Enable HTTP health checks on the NLB, supplying the URL of the company's application.
- B. Add a cron job to the EC2 instances to check the local application's logs once each minute. If HTTP errors are detected, the application will restart.
- C. Replace the NLB with an Application Load Balancer. Enable HTTP health checks by supplying the URL of the company's application. Configure an Auto Scaling action to replace unhealthy instances.
- D. Create an Amazon CloudWatch alarm that monitors the UnhealthyHostCount metric for the NLB. Configure an Auto Scaling action to replace unhealthy instances when the alarm is in the ALARM state.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* NLBs operate at layer 4 and cannot perform HTTP-level health checks. ALBs support HTTP health checks (e.g., checking a specific URL for 200 OK). By replacing the NLB with an ALB and enabling HTTP health checks, the Auto Scaling group can automatically replace unhealthy instances. Option D uses an NLB metric but still won't detect HTTP errors (AWS Docs: ALB Health Checks).
</details>

---

**Q71.** A company runs a shopping application that uses Amazon DynamoDB to store customer information. In case of data corruption, a solutions architect needs to design a solution that meets a recovery point objective (RPO) of 15 minutes and a recovery time objective (RTO) of 1 hour. What should the solutions architect recommend to meet these requirements?

- A. Configure DynamoDB global tables. For RPO recovery, point the application to a different AWS Region.
- B. Configure DynamoDB point-in-time recovery. For RPO recovery, restore to the desired point in time.
- C. Export the DynamoDB data to Amazon S3 Glacier on a daily basis. For RPO recovery, import the data from S3 Glacier to DynamoDB.
- D. Schedule Amazon Elastic Block Store (Amazon EBS) snapshots for the DynamoDB table every 15 minutes. For RPO recovery, restore the DynamoDB table by using the EBS snapshot.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* DynamoDB Point-in-Time Recovery (PITR) provides continuous backups with second-level granularity, allowing restoration to any point in the last 35 days. This meets the 15-minute RPO and 1-hour RTO. Global tables (A) are for multi-region active-active, not corruption recovery; Daily export (C) fails RPO; EBS snapshots (D) are not applicable to DynamoDB (AWS Docs: DynamoDB PITR).
</details>

---

**Q72.** A company runs a photo processing application that needs to frequently upload and download pictures from Amazon S3 buckets that are located in the same AWS Region. A solutions architect has noticed an increased cost in data transfer fees and needs to implement a solution to reduce these costs. How can the solutions architect meet this requirement?

- A. Deploy Amazon API Gateway into a public subnet and adjust the route table to route S3 calls through it.
- B. Deploy a NAT gateway into a public subnet and attach an endpoint policy that allows access to the S3 buckets.
- C. Deploy the application into a public subnet and allow it to route through an internet gateway to access the S3 buckets.
- D. Deploy an S3 VPC gateway endpoint into the VPC and attach an endpoint policy that allows access to the S3 buckets.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* A gateway VPC endpoint for S3 allows traffic between the VPC and S3 to stay within the AWS network, avoiding NAT gateway or internet gateway data transfer charges. This directly reduces data transfer costs. Options A, B, C either don't reduce costs or add complexity (AWS Docs: S3 VPC Endpoint Cost).
</details>

---

**Q73.** A company recently launched Linux-based application instances on Amazon EC2 in a private subnet and launched a Linux-based bastion host on an Amazon EC2 instance in a public subnet of a VPC. A solutions architect needs to connect from the on-premises network, through the company's internet connection, to the bastion host, and to the application servers. The solutions architect must make sure that the security groups of all the EC2 instances will allow that access. Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

- A. Replace the current security group of the bastion host with one that only allows inbound access from the application instances.
- B. Replace the current security group of the bastion host with one that only allows inbound access from an internal IP range for the company.
- C. Replace the current security group of the bastion host with one that only allows inbound access from the external IP range for the company.
- D. Replace the current security group of the application instances with one that allows inbound SSH access from only the private IP address of the bastion host.
- E. Replace the current security group of the application instances with one that allows inbound SSH access from only a public IP address of the bastion host.

<details>
<summary>Show Answer</summary>

**Answer: C, D**

*Explanation:* The bastion host in the public subnet needs inbound SSH access from the on-premises network's *external* IP range (C). The application instances in the private subnet should only allow inbound SSH from the *private* IP address of the bastion host (D), not from the internet. This follows the principle of least privilege and the bastion pattern (AWS Docs: Bastion Host Security Groups).
</details>

---

**Q74.** A solutions architect is designing a two-tier web application. The application consists of a public-facing web tier hosted on Amazon EC2 in public subnets. The database tier consists of Microsoft SQL Server running on Amazon EC2 in a private subnet. Security is a high priority for the company. How should security groups be configured in this situation? (Choose two.)

- A. Configure the security group for the web tier to allow inbound traffic on port 443 from 0.0.0.0/0.
- B. Configure the security group for the web tier to allow outbound traffic on port 443 from 0.0.0.0/0.
- C. Configure the security group for the database tier to allow inbound traffic on port 1433 from the security group for the web tier.
- D. Configure the security group for the database tier to allow outbound traffic on ports 443 and 1433 to the security group for the web tier.
- E. Configure the security group for the database tier to allow inbound traffic on ports 443 and 1433 from the security group for the web tier.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* Web tier should allow inbound HTTPS (443) from anywhere (A). The database tier (SQL Server on port 1433) should allow inbound traffic only from the web tier's security group (C). Security groups are stateful, so return traffic is automatically allowed; no explicit outbound rules are needed for the database to respond. Option B is outbound (not needed for web tier inbound), D/E have incorrect ports/protocols (AWS Docs: Security Group Rules).
</details>

---

**Q75.** A company wants to move a multi-tiered application from on premises to the AWS Cloud to improve the application's performance. The application consists of application tiers that communicate with each other by way of RESTful services. Transactions are dropped when one tier becomes overloaded. A solutions architect must design a solution that resolves these issues and modernizes the application. Which solution meets these requirements and is the MOST operationally efficient?

- A. Use Amazon API Gateway and direct transactions to the AWS Lambda functions as the application layer. Use Amazon Simple Queue Service (Amazon SQS) as the communication layer between application services.
- B. Use Amazon CloudWatch metrics to analyze the application performance history to determine the servers' peak utilization during the performance failures. Increase the size of the application server's Amazon EC2 instances to meet the peak requirements.
- C. Use Amazon Simple Notification Service (Amazon SNS) to handle the messaging between application servers running on Amazon EC2 in an Auto Scaling group. Use Amazon CloudWatch to monitor the SNS queue length and scale up and down as required.
- D. Use Amazon Simple Queue Service (Amazon SQS) to handle the messaging between application servers running on Amazon EC2 in an Auto Scaling group. Use Amazon CloudWatch to monitor the SQS queue length and scale up when communication failures are detected.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Modernizing to a serverless architecture with API Gateway (RESTful endpoint), Lambda (application layer), and SQS (decoupling communication between services) resolves the overload/dropped transaction issue and is operationally efficient. Option B is not modernizing; C and D still use EC2 and have scaling based on failures, not proactive (AWS Docs: Serverless Applications).
</details>

---

**Q76.** A company receives 10 TB of instrumentation data each day from several machines located at a single factory. The data consists of JSON files stored on a storage area network (SAN) in an on-premises data center located within the factory. The company wants to send this data to Amazon S3 where it can be accessed by several additional systems that provide critical near-real-time analytics. A secure transfer is important because the data is considered sensitive. Which solution offers the MOST reliable data transfer?

- A. AWS DataSync over public internet
- B. AWS DataSync over AWS Direct Connect
- C. AWS Database Migration Service (AWS DMS) over public internet
- D. AWS Database Migration Service (AWS DMS) over AWS Direct Connect

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS DataSync is designed for high-volume file transfers to/from S3, with built-in encryption, validation, and reliability features. Using Direct Connect provides a private, secure, and consistent network path, making it the most reliable option. DMS (C/D) is for database migration, not file-based data (JSON files). (AWS Docs: AWS DataSync).
</details>

---

**Q77.** A company needs to configure a real-time data ingestion architecture for its application. The company needs an API, a process that transforms data as the data is streamed, and a storage solution for the data. Which solution will meet these requirements with the LEAST operational overhead?

- A. Deploy an Amazon EC2 instance to host an API that sends data to an Amazon Kinesis data stream. Create an Amazon Kinesis Data Firehose delivery stream that uses the Kinesis data stream as a data source. Use AWS Lambda functions to transform the data. Use the Kinesis Data Firehose delivery stream to send the data to Amazon S3.
- B. Deploy an Amazon EC2 instance to host an API that sends data to AWS Glue. Stop source/destination checking on the EC2 instance. Use AWS Glue to transform the data and to send the data to Amazon S3.
- C. Configure an Amazon API Gateway API to send data to an Amazon Kinesis data stream. Create an Amazon Kinesis Data Firehose delivery stream that uses the Kinesis data stream as a data source. Use AWS Lambda functions to transform the data. Use the Kinesis Data Firehose delivery stream to send the data to Amazon S3.
- D. Configure an Amazon API Gateway API to send data to AWS Glue. Use AWS Lambda functions to transform the data. Use AWS Glue to send the data to Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* This solution uses fully managed, serverless services: API Gateway (API), Kinesis Data Streams (ingestion), Lambda (transform), Kinesis Data Firehose (delivery), S3 (storage). This has the least operational overhead compared to deploying EC2 instances (A, B). Glue (B, D) is not ideal for real-time streaming transforms (AWS Docs: Real-time Ingestion with Kinesis).
</details>

---

**Q78.** A company needs to keep user transaction data in an Amazon DynamoDB table. The company must retain the data for 7 years. What is the MOST operationally efficient solution that meets these requirements?

- A. Use DynamoDB point-in-time recovery to back up the table continuously.
- B. Use AWS Backup to create backup schedules and retention policies for the table.
- C. Create an on-demand backup of the table by using the DynamoDB console. Store the backup in an Amazon S3 bucket. Set an S3 Lifecycle configuration for the S3 bucket.
- D. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to invoke an AWS Lambda function. Configure the Lambda function to back up the table and to store the backup in an Amazon S3 bucket. Set an S3 Lifecycle configuration for the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Backup provides a centralized, managed solution for creating backup schedules and defining retention policies for DynamoDB (and other services) with minimal operational overhead. PITR (A) is for recovery, not long-term retention; Manual on-demand backups (C) are not scheduled; Custom Lambda (D) is more overhead (AWS Docs: AWS Backup for DynamoDB).
</details>

---

**Q79.** A company is planning to use an Amazon DynamoDB table for data storage. The company is concerned about cost optimization. The table will not be used on most mornings. In the evenings, the read and write traffic will often be unpredictable. When traffic spikes occur, they will happen very quickly. What should a solutions architect recommend?

- A. Create a DynamoDB table in on-demand capacity mode.
- B. Create a DynamoDB table with a global secondary index.
- C. Create a DynamoDB table with provisioned capacity and auto scaling.
- D. Create a DynamoDB table in provisioned capacity mode, and configure it as a global table.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* On-demand capacity mode is best for unpredictable traffic patterns, including sudden spikes. It scales automatically and you pay only for the requests you make. Provisioned with auto scaling (C) can lag behind sudden spikes. Global tables (D) are for multi-region, not cost optimization (AWS Docs: DynamoDB On-Demand).
</details>

---

**Q80.** A company recently signed a contract with an AWS Managed Service Provider (MSP) Partner for help with an application migration initiative. A solutions architect needs to share an Amazon Machine Image (AMI) from an existing AWS account with the MSP Partner's AWS account. The AMI is backed by Amazon Elastic Block Store (Amazon EBS) and uses an AWS Key Management Service (AWS KMS) customer managed key to encrypt EBS volume snapshots. What is the MOST secure way for the solutions architect to share the AMI with the MSP Partner's AWS account?

- A. Make the encrypted AMI and snapshots publicly available. Modify the key policy to allow the MSP Partner's AWS account to use the key.
- B. Modify the launchPermission property of the AMI. Share the AMI with the MSP Partner's AWS account only. Modify the key policy to allow the MSP Partner's AWS account to use the key.
- C. Modify the launchPermission property of the AMI. Share the AMI with the MSP Partner's AWS account only. Modify the key policy to trust a new KMS key that is owned by the MSP Partner for encryption.
- D. Export the AMI from the source account to an Amazon S3 bucket in the MSP Partner's AWS account. Encrypt the S3 bucket with a new KMS key that is owned by the MSP Partner. Copy and launch the AMI in the MSP Partner's AWS account.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* The most secure way is to modify launchPermission to share the AMI *only* with the MSP's account, and modify the KMS key policy to grant the MSP's account permission to use the key (decrypt). Option A (public) is insecure; Option C (new key) doesn't work because snapshots encrypted with the original key cannot be decrypted by a different key; Option D is overly complex (AWS Docs: Share Encrypted AMIs).
</details>

---

**Q81.** A solutions architect is designing the cloud architecture for a new application being deployed on AWS. The process should run in parallel while adding and removing application nodes as needed based on the number of jobs to be processed. The processor application is stateless. The solutions architect must ensure that the application is loosely coupled and the job items are durably stored. Which design should the solutions architect use?

- A. Create an Amazon SNS topic to send the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch configuration that uses the AMI. Create an Auto Scaling group using the launch configuration. Set the scaling policy for the Auto Scaling group to add and remove nodes based on CPU usage.
- B. Create an Amazon SQS queue to hold the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch configuration that uses the AMI. Create an Auto Scaling group using the launch configuration. Set the scaling policy for the Auto Scaling group to add and remove nodes based on network usage.
- C. Create an Amazon SQS queue to hold the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch template that uses the AMI. Create an Auto Scaling group using the launch template. Set the scaling policy for the Auto Scaling group to add and remove nodes based on the number of items in the SQS queue.
- D. Create an Amazon SNS topic to send the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch template that uses the AMI. Create an Auto Scaling group using the launch template. Set the scaling policy for the Auto Scaling group to add and remove nodes based on the number of messages published to the SNS topic.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* SQS durably stores jobs, providing loose coupling. Scaling based on the queue depth (ApproximateNumberOfMessages) is the correct pattern for a job processing system. Scaling based on CPU/network (A/B) is less direct; SNS (D) is not a queue and doesn't provide durable storage for jobs (AWS Docs: SQS-based scaling).
</details>

---

**Q82.** A company hosts its web applications in the AWS Cloud. The company configures Elastic Load Balancers to use certificates that are imported into AWS Certificate Manager (ACM). The company's security team must be notified 30 days before the expiration of each certificate. What should a solutions architect recommend to meet this requirement?

- A. Add a rule in ACM to publish a custom message to an Amazon Simple Notification Service (Amazon SNS) topic every day, beginning 30 days before any certificate will expire.
- B. Create an AWS Config rule that checks for certificates that will expire within 30 days. Configure Amazon EventBridge (Amazon CloudWatch Events) to invoke a custom alert by way of Amazon Simple Notification Service (Amazon SNS) when AWS Config reports a noncompliant resource.
- C. Use AWS Trusted Advisor to check for certificates that will expire within 30 days. Create an Amazon CloudWatch alarm that is based on Trusted Advisor metrics for check status changes. Configure the alarm to send a custom alert by way of Amazon Simple Notification Service (Amazon SNS).
- D. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to detect any certificates that will expire within 30 days. Configure the rule to invoke an AWS Lambda function. Configure the Lambda function to send a custom alert by way of Amazon Simple Notification Service (Amazon SNS).

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Config can evaluate resources (ACM certificates) against a rule (e.g., expiration within 30 days) and publish findings to EventBridge, which can then trigger an SNS alert. This is the standard way to automate certificate expiration monitoring. Option A (ACM rule) doesn't exist; Trusted Advisor (C) doesn't provide event-driven alerts easily; Option D requires a Lambda function (more overhead than Config) (AWS Docs: AWS Config ACM Expiration).
</details>

---

**Q83.** A company's dynamic website is hosted using on-premises servers in the United States. The company is launching its product in Europe, and it wants to optimize site loading times for new European users. The site's backend must remain in the United States. The product is being launched in a few days, and an immediate solution is needed. What should the solutions architect recommend?

- A. Launch an Amazon EC2 instance in us-east-1 and migrate the site to it.
- B. Move the website to Amazon S3. Use Cross-Region Replication between Regions.
- C. Use Amazon CloudFront with a custom origin pointing to the on-premises servers.
- D. Use an Amazon Route 53 geoproximity routing policy pointing to on-premises servers.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* CloudFront with a custom origin (the on-premises US servers) will cache static and dynamic content at edge locations in Europe, dramatically improving load times for European users without moving the backend. This is quick to implement (days). Option A (EC2) requires migration; Option B (S3) is for static sites; Option D (geoproximity) just routes traffic, doesn't cache (AWS Docs: CloudFront Custom Origin).
</details>

---

**Q84.** A company wants to reduce the cost of its existing three-tier web architecture. The web, application, and database servers are running on Amazon EC2 instances for the development, test, and production environments. The EC2 instances average 30% CPU utilization during peak hours and 10% CPU utilization during non-peak hours. The production EC2 instances run 24 hours a day. The development and test EC2 instances run for at least 8 hours each day. The company plans to implement automation to stop the development and test EC2 instances when they are not in use. Which EC2 instance purchasing solution will meet the company's requirements MOST cost-effectively?

- A. Use Spot Instances for the production EC2 instances. Use Reserved Instances for the development and test EC2 instances.
- B. Use Reserved Instances for the production EC2 instances. Use On-Demand Instances for the development and test EC2 instances.
- C. Use Spot blocks for the production EC2 instances. Use Reserved Instances for the development and test EC2 instances.
- D. Use On-Demand Instances for the production EC2 instances. Use Spot blocks for the development and test EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Production runs 24/7, so Reserved Instances (1-year or 3-year) provide the largest discount. Development and test run only 8 hours/day and will be stopped when not in use, so On-Demand is cost-effective (no upfront commitment). Spot Instances (A/C/D) can be interrupted, which is not ideal for production. Spot blocks are deprecated/not recommended (AWS Docs: EC2 Pricing).
</details>

---

**Q85.** A company has a production web application in which users upload documents through a web interface or a mobile app. According to a new regulatory requirement, new documents cannot be modified or deleted after they are stored. What should a solutions architect do to meet this requirement?

- A. Store the uploaded documents in an Amazon S3 bucket with S3 Versioning and S3 Object Lock enabled.
- B. Store the uploaded documents in an Amazon S3 bucket. Configure an S3 Lifecycle policy to archive the documents periodically.
- C. Store the uploaded documents in an Amazon S3 bucket with S3 Versioning enabled. Configure an ACL to restrict all access to read-only.
- D. Store the uploaded documents on an Amazon Elastic File System (Amazon EFS) volume. Access the data by mounting the volume in read-only mode.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Object Lock (in compliance mode) prevents any user from deleting or overwriting an object for a set retention period. Versioning is required for Object Lock. This meets the regulatory requirement. Lifecycle policies (B) manage transitions, not immutability; ACLs (C) can be changed; EFS (D) does not have built-in immutability like Object Lock (AWS Docs: S3 Object Lock).
</details>

---

**Q86.** A company has several web servers that need to frequently access a common Amazon RDS MySQL Multi-AZ DB instance. The company wants a secure method for the web servers to connect to the database while meeting a security requirement to rotate user credentials frequently. Which solution meets these requirements?

- A. Store the database user credentials in AWS Secrets Manager. Grant the necessary IAM permissions to allow the web servers to access AWS Secrets Manager.
- B. Store the database user credentials in AWS Systems Manager OpsCenter. Grant the necessary IAM permissions to allow the web servers to access OpsCenter.
- C. Store the database user credentials in a secure Amazon S3 bucket. Grant the necessary IAM permissions to allow the web servers to retrieve credentials and access the database.
- D. Store the database user credentials in files encrypted with AWS Key Management Service (AWS KMS) on the web server file system. The web server should be able to decrypt the files and access the database.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Secrets Manager is the AWS service designed for storing and rotating database credentials. The web servers can retrieve credentials programmatically via IAM roles. OpsCenter (B) is for operational issues; S3 (C) and local files (D) add overhead and don't provide native rotation (AWS Docs: Secrets Manager for RDS).
</details>

---

**Q87.** A company hosts an application on AWS Lambda functions that are invoked by an Amazon API Gateway API. The Lambda functions save customer data to an Amazon Aurora MySQL database. Whenever the company upgrades the database, the Lambda functions fail to establish database connections until the upgrade is complete. The result is that customer data is not recorded for some of the event. A solutions architect needs to design a solution that stores customer data that is created during database upgrades. Which solution will meet these requirements?

- A. Provision an Amazon RDS proxy to sit between the Lambda functions and the database. Configure the Lambda functions to connect to the RDS proxy.
- B. Increase the run time of the Lambda functions to the maximum. Create a retry mechanism in the code that stores the customer data in the database.
- C. Persist the customer data to Lambda local storage. Configure new Lambda functions to scan the local storage to save the customer data to the database.
- D. Store the customer data in an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Create a new Lambda function that polls the queue and stores the customer data in the database.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Using an SQS queue to buffer customer data during database upgrades ensures no data is lost. The Lambda function can consume from the queue once the database is available. RDS Proxy (A) helps with connection management but doesn't buffer data during an outage. Lambda local storage (C) is ephemeral. Option D provides durability and decoupling (AWS Docs: SQS as Buffer).
</details>

---

**Q88.** A survey company has gathered data for several years from areas in the United States. The company hosts the data in an Amazon S3 bucket that is 3 TB in size and growing. The company has started to share the data with a European marketing firm that has S3 buckets. The company wants to ensure that its data transfer costs remain as low as possible. Which solution will meet these requirements?

- A. Configure the Requester Pays feature on the company's S3 bucket.
- B. Configure S3 Cross-Region Replication from the company's S3 bucket to one of the marketing firm's S3 buckets.
- C. Configure cross-account access for the marketing firm so that the marketing firm has access to the company's S3 bucket.
- D. Configure the company's S3 bucket to use S3 Intelligent-Tiering. Sync the S3 bucket to one of the marketing firm's S3 buckets.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Requester Pays shifts the cost of data transfer and requests from the bucket owner (company) to the requester (marketing firm). This ensures the company's costs remain low. Cross-Region Replication (B) incurs costs for the company. Cross-account access (C) without Requester Pays still charges the company for data transfer. Intelligent-Tiering (D) optimizes storage cost, not transfer cost (AWS Docs: S3 Requester Pays).
</details>

---

**Q89.** A company uses Amazon S3 to store its confidential audit documents. The S3 bucket uses bucket policies to restrict access to audit team IAM user credentials according to the principle of least privilege. Company managers are worried about accidental deletion of documents in the S3 bucket and want a more secure solution. What should a solutions architect do to secure the audit documents?

- A. Enable the versioning and MFA Delete features on the S3 bucket.
- B. Enable multi-factor authentication (MFA) on the IAM user credentials for each audit team IAM user account.
- C. Add an S3 Lifecycle policy to the audit team's IAM user accounts to deny the s3:DeleteObject action during audit dates.
- D. Use AWS Key Management Service (AWS KMS) to encrypt the S3 bucket and restrict audit team IAM user accounts from accessing the KMS key.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Versioning allows recovery of accidentally deleted objects. MFA Delete adds an extra authentication requirement to permanently delete object versions or change versioning state, preventing accidental deletion even if IAM credentials are compromised. Option B (MFA on IAM user) doesn't protect against accidental deletion via API; Option C (Lifecycle) is not for access control; Option D (KMS) protects confidentiality, not deletion (AWS Docs: MFA Delete).
</details>

---

**Q90.** A company is using a SQL database to store movie data that is publicly accessible. The database runs on an Amazon RDS Single-AZ DB instance. A script runs queries at random intervals each day to record the number of new movies that have been added to the database. The script must report a final total during business hours. The company's development team notices that the database performance is inadequate for development tasks when the script is running. A solutions architect must recommend a solution to resolve this issue. Which solution will meet this requirement with the LEAST operational overhead?

- A. Modify the DB instance to be a Multi-AZ deployment.
- B. Create a read replica of the database. Configure the script to query only the read replica.
- C. Instruct the development team to manually export the entries in the database at the end of each day.
- D. Use Amazon ElastiCache to cache the common queries that the script runs against the database.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Creating a read replica offloads the read queries from the script to the replica, eliminating performance impact on the primary database used by development tasks. Multi-AZ (A) provides high availability, not read offloading; Manual export (C) is not automated; ElastiCache (D) would require application changes (AWS Docs: RDS Read Replicas).
</details>

---

**Q91.** A company has applications that run on Amazon EC2 instances in a VPC. One of the applications needs to call the Amazon S3 API to store and read objects. According to the company's security regulations, no traffic from the applications is allowed to travel across the internet. Which solution will meet these requirements?

- A. Configure an S3 gateway endpoint.
- B. Create an S3 bucket in a private subnet.
- C. Create an S3 bucket in the same AWS Region as the EC2 instances.
- D. Configure a NAT gateway in the same subnet as the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* A gateway VPC endpoint for S3 allows traffic to S3 to stay within the AWS network and not traverse the internet. S3 buckets (B, C) are not inside a VPC/subnet. NAT gateway (D) routes traffic to the internet, violating the regulation (AWS Docs: S3 VPC Endpoint).
</details>

---

**Q92.** A company is storing sensitive user information in an Amazon S3 bucket. The company wants to provide secure access to this bucket from the application tier running on Amazon EC2 instances inside a VPC. Which combination of steps should a solutions architect take to accomplish this? (Choose two.)

- A. Configure a VPC gateway endpoint for Amazon S3 within the VPC.
- B. Create a bucket policy to make the objects in the S3 bucket public.
- C. Create a bucket policy that limits access to only the application tier running in the VPC.
- D. Create an IAM user with an S3 access policy and copy the IAM credentials to the EC2 instance.
- E. Create a NAT instance and have the EC2 instances use the NAT instance to access the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* A VPC gateway endpoint (A) provides private connectivity. A bucket policy (C) can restrict access to only that specific VPC or VPC endpoint, ensuring security. Making objects public (B) is insecure. Copying IAM credentials (D) to EC2 is bad practice (use roles). NAT instance (E) routes over internet, not private (AWS Docs: S3 Bucket Policy with VPC Endpoint).
</details>

---

**Q93.** A company runs an on-premises application that is powered by a MySQL database. The company is migrating the application to AWS to increase the application's elasticity and availability. The current architecture shows heavy read activity on the database during times of normal operation. Every 4 hours, the company's development team pulls a full export of the production database to populate a database in the staging environment. During this period, users experience unacceptable application latency. The development team is unable to use the staging environment until the procedure completes. A solutions architect must recommend replacement architecture that alleviates the application latency issue. The replacement architecture also must give the development team the ability to continue using the staging environment without delay. Which solution meets these requirements?

- A. Use Amazon Aurora MySQL with Multi-AZ Aurora Replicas for production. Populate the staging database by implementing a backup and restore process that uses the mysqldump utility.
- B. Use Amazon Aurora MySQL with Multi-AZ Aurora Replicas for production. Use database cloning to create the staging database on-demand.
- C. Use Amazon RDS for MySQL with a Multi-AZ deployment and read replicas for production. Use the standby instance for the staging database.
- D. Use Amazon RDS for MySQL with a Multi-AZ deployment and read replicas for production. Populate the staging database by implementing a backup and restore process that uses the mysqldump utility.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon Aurora supports fast database cloning, which creates a copy of the database without impacting production performance. This allows the staging environment to be populated quickly without the latency caused by full exports (mysqldump). Options A, C, D still use backup/restore or exports, which cause performance issues (AWS Docs: Aurora Cloning).
</details>

---

**Q94.** A company is designing an application where users upload small files into Amazon S3. After a user uploads a file, the file requires one-time simple processing to transform the data and save the data in JSON format for later analysis. Each file must be processed as quickly as possible after it is uploaded. Demand will vary. On some days, users will upload a high number of files. On other days, users will upload a few files or no files. Which solution meets these requirements with the LEAST operational overhead?

- A. Configure Amazon EMR to read text files from Amazon S3. Run processing scripts to transform the data. Store the resulting JSON file in an Amazon Aurora DB cluster.
- B. Configure Amazon S3 to send an event notification to an Amazon Simple Queue Service (Amazon SQS) queue. Use Amazon EC2 instances to read from the queue and process the data. Store the resulting JSON file in Amazon DynamoDB.
- C. Configure Amazon S3 to send an event notification to an Amazon Simple Queue Service (Amazon SQS) queue. Use an AWS Lambda function to read from the queue and process the data. Store the resulting JSON file in Amazon DynamoDB.
- D. Configure Amazon EventBridge (Amazon CloudWatch Events) to send an event to Amazon Kinesis Data Streams when a new file is uploaded. Use an AWS Lambda function to consume the event from the stream and process the data. Store the resulting JSON file in an Amazon Aurora DB cluster.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Using S3 event notification -> SQS -> Lambda provides a serverless, scalable, and low-latency processing pipeline with minimal overhead. SQS decouples the event from processing. Option A (EMR) and B (EC2) have more overhead; Option D (Kinesis) is overkill for file upload events (AWS Docs: S3 to SQS to Lambda).
</details>

---

**Q95.** An application allows users at a company's headquarters to access product data. The product data is stored in an Amazon RDS MySQL DB instance. The operations team has isolated an application performance slowdown and wants to separate read traffic from write traffic. A solutions architect needs to optimize the application's performance quickly. What should the solutions architect recommend?

- A. Change the existing database to a Multi-AZ deployment. Serve the read requests from the primary Availability Zone.
- B. Change the existing database to a Multi-AZ deployment. Serve the read requests from the secondary Availability Zone.
- C. Create read replicas for the database. Configure the read replicas with half of the compute and storage resources as the source database.
- D. Create read replicas for the database. Configure the read replicas with the same compute and storage resources as the source database.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Read replicas are designed to offload read traffic. For optimal performance, the read replica should have at least the same resources as the source to handle the read load effectively. Multi-AZ (A/B) provides high availability, not read scaling. Option C (half resources) may lead to poor performance (AWS Docs: RDS Read Replicas Performance).
</details>

---

**Q96.** An Amazon EC2 administrator created the following policy associated with an IAM group containing several users:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:TerminateInstances",
      "Resource": "*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "10.100.100.0/24"
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "ec2:Region": "us-east-1"
        }
      }
    }
  ]
}
```
What is the effect of this policy?

- A. Users can terminate an EC2 instance in any AWS Region except us-east-1.
- B. Users can terminate an EC2 instance with the IP address 10.100.100.1 in the us-east-1 Region.
- C. Users can terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254.
- D. Users cannot terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* The Allow statement grants TerminateInstances in any Region if source IP is in 10.100.100.0/24. The Deny statement denies all EC2 actions if the Region is *not* us-east-1. So, TerminateInstances is only allowed in us-east-1 *and* when source IP is in the range. Option C matches both conditions (IP 10.100.100.254 is in range). (AWS Docs: IAM Policy Evaluation Logic).
</details>

---

**Q97.** A company has a large Microsoft SharePoint deployment running on-premises that requires Microsoft Windows shared file storage. The company wants to migrate this workload to the AWS Cloud and is considering various storage options. The storage solution must be highly available and integrated with Active Directory for access control. Which solution will satisfy these requirements?

- A. Configure Amazon EFS storage and set the Active Directory domain for authentication.
- B. Create an SMB file share on an AWS Storage Gateway file gateway in two Availability Zones.
- C. Create an Amazon S3 bucket and configure Microsoft Windows Server to mount it as a volume.
- D. Create an Amazon FSx for Windows File Server file system on AWS and set the Active Directory domain for authentication.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Amazon FSx for Windows File Server is a fully managed, highly available Windows file system that natively integrates with Active Directory for access control. It supports SMB protocol required by SharePoint. EFS (A) is NFS, not SMB. Storage Gateway (B) is a hybrid solution. S3 (C) is object storage, not a file system (AWS Docs: FSx for Windows File Server AD Integration).
</details>

---

**Q98.** An image-processing company has a web application that users use to upload images. The application uploads the images into an Amazon S3 bucket. The company has set up S3 event notifications to publish the object creation events to an Amazon Simple Queue Service (Amazon SQS) standard queue. The SQS queue serves as the event source for an AWS Lambda function that processes the images and sends the results to users through email. Users report that they are receiving multiple email messages for every uploaded image. A solutions architect determines that SQS messages are invoking the Lambda function more than once, resulting in multiple email messages. What should the solutions architect do to resolve this issue with the LEAST operational overhead?

- A. Set up long polling in the SQS queue by increasing the ReceiveMessage wait time to 30 seconds.
- B. Change the SQS standard queue to an SQS FIFO queue. Use the message deduplication ID to discard duplicate messages.
- C. Increase the visibility timeout in the SQS queue to a value that is greater than the total of the function timeout and the batch window timeout.
- D. Modify the Lambda function to delete each message from the SQS queue immediately after the message is read before processing.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Duplicate processing often occurs when the Lambda function takes longer than the visibility timeout, causing the message to become visible again and be reprocessed. Increasing the visibility timeout ensures the function has enough time to process and delete the message. Switching to FIFO (B) might work but adds complexity and cost. Option D (deleting before processing) risks data loss if processing fails (AWS Docs: SQS Visibility Timeout).
</details>

---

**Q99.** A company is implementing a shared storage solution for a gaming application that is hosted in an on-premises data center. The company needs the ability to use Lustre clients to access data. The solution must be fully managed. Which solution meets these requirements?

- A. Create an AWS Storage Gateway file gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.
- B. Create an Amazon EC2 Windows instance. Install and configure a Windows file share role on the instance. Connect the application server to the file share.
- C. Create an Amazon Elastic File System (Amazon EFS) file system, and configure it to support Lustre. Attach the file system to the origin server. Connect the application server to the file system.
- D. Create an Amazon FSx for Lustre file system. Attach the file system to the origin server. Connect the application server to the file system.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Amazon FSx for Lustre is a fully managed, high-performance file system optimized for HPC and machine learning workloads, and it supports Lustre clients. Storage Gateway (A) doesn't support Lustre; EC2 Windows (B) is not managed and not Lustre; EFS (C) does not support Lustre (AWS Docs: Amazon FSx for Lustre).
</details>

---

**Q100.** A company's containerized application runs on an Amazon EC2 instance. The application needs to download security certificates before it can communicate with other business applications. The company wants a highly secure solution to encrypt and decrypt the certificates in near real time. The solution also needs to store data in highly available storage after the data is encrypted. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create AWS Secrets Manager secrets for encrypted certificates. Manually update the certificates as needed. Control access to the data by using fine-grained IAM access.
- B. Create an AWS Lambda function that uses the Python cryptography library to receive and perform encryption operations. Store the function in an Amazon S3 bucket.
- C. Create an AWS Key Management Service (AWS KMS) customer managed key. Allow the EC2 role to use the KMS key for encryption operations. Store the encrypted data on Amazon S3.
- D. Create an AWS Key Management Service (AWS KMS) customer managed key. Allow the EC2 role to use the KMS key for encryption operations. Store the encrypted data on Amazon Elastic Block Store (Amazon EBS) volumes.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS KMS provides managed encryption/decryption (including near real-time) with no operational overhead for key management. S3 provides highly available, durable storage for the encrypted certificates. Option A (Secrets Manager) is for secrets, not general data encryption; Option B requires custom Lambda; Option D (EBS) is not as highly available/durable as S3 for this purpose (AWS Docs: AWS KMS + S3).
</details>

---

**Q101.** A solutions architect is designing a VPC with public and private subnets. The VPC and subnets use IPv4 CIDR blocks. There is one public subnet and one private subnet in each of three Availability Zones (AZs) for high availability. An internet gateway is used to provide internet access for the public subnets. The private subnets require access to the internet to allow Amazon EC2 instances to download software updates. What should the solutions architect do to enable Internet access for the private subnets?

- A. Create three NAT gateways, one for each public subnet in each AZ. Create a private route table for each AZ that forwards non-VPC traffic to the NAT gateway in its AZ.
- B. Create three NAT instances, one for each private subnet in each AZ. Create a private route table for each AZ that forwards non-VPC traffic to the NAT instance in its AZ.
- C. Create a second internet gateway on one of the private subnets. Update the route table for the private subnets that forward non-VPC traffic to the private internet gateway.
- D. Create an egress-only internet gateway on one of the public subnets. Update the route table for the private subnets that forward non-VPC traffic to the egress-only Internet gateway.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* To provide internet access to private subnets for software updates (outbound IPv4 traffic), you need NAT gateways in the public subnets. For high availability across AZs, create one NAT gateway per AZ in the public subnet, and route private subnet traffic to the NAT gateway in the same AZ. Option B (NAT instances) are less managed; Option C (IGW in private subnet) is invalid; Option D (egress-only IGW) is for IPv6 only (AWS Docs: NAT Gateway High Availability).
</details>

---

**Q102.** A company wants to migrate an on-premises data center to AWS. The data center hosts an SFTP server that stores its data on an NFS-based file system. The server holds 200 GB of data that needs to be transferred. The server must be hosted on an Amazon EC2 instance that uses an Amazon Elastic File System (Amazon EFS) file system. Which combination of steps should a solutions architect take to automate this task? (Choose two.)

- A. Launch the EC2 instance into the same Availability Zone as the EFS file system.
- B. Install an AWS DataSync agent in the on-premises data center.
- C. Create a secondary Amazon Elastic Block Store (Amazon EBS) volume on the EC2 instance for the data.
- D. Manually use an operating system copy command to push the data to the EC2 instance.
- E. Use AWS DataSync to create a suitable location configuration for the on-premises SFTP server.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* AWS DataSync automates data transfer between on-premises storage and AWS. You install a DataSync agent on-premises (B), configure a source location (on-premises NFS/SFTP) (E), and a destination location (EFS). DataSync handles the transfer. Option A is not required (EFS accessible across AZs); C (EBS) is not needed; D is manual, not automated (AWS Docs: DataSync with EFS).
</details>

---

**Q103.** A company has an AWS Glue extract, transform, and load (ETL) job that runs every day at the same time. The job processes XML data that is in an Amazon S3 bucket. New data is added to the S3 bucket every day. A solutions architect notices that AWS Glue is processing all the data during each run. What should the solutions architect do to prevent AWS Glue from reprocessing old data?

- A. Edit the job to use job bookmarks.
- B. Edit the job to delete data after the data is processed.
- C. Edit the job by setting the NumberOfWorkers field to 1.
- D. Use a FindMatches machine learning (ML) transform.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Glue job bookmarks track data that has already been processed, allowing subsequent job runs to process only new data. This prevents reprocessing of old data. Option B (deleting) is risky; Option C (number of workers) affects parallelism, not processing scope; Option D (FindMatches) is for deduplication, not incremental processing (AWS Docs: Glue Job Bookmarks).
</details>

---

**Q104.** A solutions architect must design a highly available infrastructure for a website. The website is powered by Windows web servers that run on Amazon EC2 instances. The solutions architect must implement a solution that can mitigate a large-scale DDoS attack that originates from thousands of IP addresses. Downtime is not acceptable for the website. Which actions should the solutions architect take to protect the website from such an attack? (Choose two.)

- A. Use AWS Shield Advanced to stop the DDoS attack.
- B. Configure Amazon GuardDuty to automatically block the attackers.
- C. Configure the website to use Amazon CloudFront for both static and dynamic content.
- D. Use an AWS Lambda function to automatically add attacker IP addresses to VPC network ACLs.
- E. Use EC2 Spot Instances in an Auto Scaling group with a target tracking scaling policy that is set to 80% CPU utilization.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* AWS Shield Advanced provides enhanced DDoS protection. CloudFront provides DDoS mitigation at the edge, absorbing and filtering malicious traffic. GuardDuty (B) is threat detection, not mitigation; Lambda adding IPs to NACLs (D) is too slow for DDoS; Spot Instances (E) don't mitigate DDoS (AWS Docs: DDoS Protection with Shield Advanced and CloudFront).
</details>

---

**Q105.** A company is preparing to deploy a new serverless workload. A solutions architect must use the principle of least privilege to configure permissions that will be used to run an AWS Lambda function. An Amazon EventBridge (Amazon CloudWatch Events) rule will invoke the function. Which solution meets these requirements?

- A. Add an execution role to the function with lambda:InvokeFunction as the action and * as the principal.
- B. Add an execution role to the function with lambda:InvokeFunction as the action and Service: lambda.amazonaws.com as the principal.
- C. Add a resource-based policy to the function with lambda:* as the action and Service: events.amazonaws.com as the principal.
- D. Add a resource-based policy to the function with lambda:InvokeFunction as the action and Service: events.amazonaws.com as the principal.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* EventBridge needs permission to invoke the Lambda function. The most secure way is to use a resource-based policy on the Lambda function, granting `lambda:InvokeFunction` to the EventBridge service principal. Option A is too broad; Option B is for the execution role (what the function can do), not for who can invoke it; Option C grants all Lambda actions (AWS Docs: Lambda Resource-Based Policies for EventBridge).
</details>

---

**Q106.** A company is preparing to store confidential data in Amazon S3. For compliance reasons, the data must be encrypted at rest. Encryption key usage must be logged for auditing purposes. Keys must be rotated every year. Which solution meets these requirements and is the MOST operationally efficient?

- A. Server-side encryption with customer-provided keys (SSE-C)
- B. Server-side encryption with Amazon S3 managed keys (SSE-S3)
- C. Server-side encryption with AWS KMS keys (SSE-KMS) with manual rotation
- D. Server-side encryption with AWS KMS keys (SSE-KMS) with automatic rotation

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* SSE-KMS with automatic rotation meets all requirements: encryption at rest, key usage logging via CloudTrail, and automatic annual key rotation. SSE-C (A) requires you to manage keys; SSE-S3 (B) does not provide key usage logs; Manual rotation (C) is less operationally efficient (AWS Docs: SSE-KMS Key Rotation).
</details>

---

**Q107.** A bicycle sharing company is developing a multi-tier architecture to track the location of its bicycles during peak operating hours. The company wants to use these data points in its existing analytics platform. A solutions architect must determine the most viable multi-tier option to support this architecture. The data points must be accessible from the REST API. Which action meets these requirements for storing and retrieving location data?

- A. Use Amazon Athena with Amazon S3.
- B. Use Amazon API Gateway with AWS Lambda.
- C. Use Amazon QuickSight with Amazon Redshift.
- D. Use Amazon API Gateway with Amazon Kinesis Data Analytics.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* API Gateway provides the REST API endpoint, and Lambda can process and store/retrieve location data. This is a standard serverless multi-tier architecture. Athena (A) is for querying; QuickSight (C) is for visualization; Kinesis Data Analytics (D) is for real-time analytics, not primary storage/retrieval (AWS Docs: API Gateway + Lambda).
</details>

---

**Q108.** A company has an automobile sales website that stores its listings in a database on Amazon RDS. When an automobile is sold, the listing needs to be removed from the website and the data must be sent to multiple target systems. Which design should a solutions architect recommend?

- A. Create an AWS Lambda function triggered when the database on Amazon RDS is updated to send the information to an Amazon Simple Queue Service (Amazon SQS) queue for the targets to consume.
- B. Create an AWS Lambda function triggered when the database on Amazon RDS is updated to send the information to an Amazon Simple Queue Service (Amazon SQS) FIFO queue for the targets to consume.
- C. Subscribe to an RDS event notification and send an Amazon Simple Queue Service (Amazon SQS) queue fanned out to multiple Amazon Simple Notification Service (Amazon SNS) topics. Use AWS Lambda functions to update the targets.
- D. Subscribe to an RDS event notification and send an Amazon Simple Notification Service (Amazon SNS) topic fanned out to multiple Amazon Simple Queue Service (Amazon SQS) queues. Use AWS Lambda functions to update the targets.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* RDS event notifications can trigger an SNS topic. Fanning out from SNS to multiple SQS queues allows each target system to have its own queue and consume messages at its own pace. Lambda functions can then process from the queues. Option A/B use Lambda triggered directly by RDS (not an event source). Option C inverts the fan-out pattern (SQS to SNS is less common) (AWS Docs: RDS Event Notifications with SNS/SQS).
</details>

---

**Q109.** A company needs to store data in Amazon S3 and must prevent the data from being changed. The company wants new objects that are uploaded to Amazon S3 to remain unchangeable for a nonspecific amount of time until the company decides to modify the objects. Only specific users in the company's AWS account can have the ability to delete the objects. What should a solutions architect do to meet these requirements?

- A. Create an S3 Glacier vault. Apply a write-once, read-many (WORM) vault lock policy to the objects.
- B. Create an S3 bucket with S3 Object Lock enabled. Enable versioning. Set a retention period of 100 years. Use governance mode as the S3 bucket's default retention mode for new objects.
- C. Create an S3 bucket. Use AWS CloudTrail to track any S3 API events that modify the objects. Upon notification, restore the modified objects from any backup versions that the company has.
- D. Create an S3 bucket with S3 Object Lock enabled. Enable versioning. Add a legal hold to the objects. Add the s3:PutObjectLegalHold permission to the IAM policies of users who need to delete the objects.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Legal hold prevents object version deletion until the hold is removed. It is independent of a retention period, suitable for "nonspecific amount of time". Only users with the `s3:PutObjectLegalHold` permission can remove the hold and then delete. Option B (governance mode) allows authorized users to shorten retention; Option A (Glacier vault) is not S3 standard; Option C is reactive, not preventive (AWS Docs: S3 Legal Hold).
</details>

---

**Q110.** A social media company allows users to upload images to its website. The website runs on Amazon EC2 instances. During upload requests, the website resizes the images to a standard size and stores the resized images in Amazon S3. Users are experiencing slow upload requests to the website. The company needs to reduce coupling within the application and improve website performance. A solutions architect must design the most operationally efficient process for image uploads. Which combination of actions should the solutions architect take to meet these requirements? (Choose two.)

- A. Configure the application to upload images to S3 Glacier.
- B. Configure the web server to upload the original images to Amazon S3.
- C. Configure the application to upload images directly from each user's browser to Amazon S3 through the use of a presigned URL.
- D. Configure S3 Event Notifications to invoke an AWS Lambda function when an image is uploaded. Use the function to resize the image.
- E. Create an Amazon EventBridge (Amazon CloudWatch Events) rule that invokes an AWS Lambda function on a schedule to resize uploaded images.

<details>
<summary>Show Answer</summary>

**Answer: C, D**

*Explanation:* Using presigned URLs (C) allows direct browser-to-S3 uploads, offloading the web server and reducing latency. S3 event notifications triggering a Lambda function (D) resizes the image asynchronously, decoupling the upload from processing. Option A (Glacier) is for archival; Option B still uses web server; Option E (scheduled) is not real-time (AWS Docs: S3 Presigned URLs, S3 Event Notifications to Lambda).
</details>

---

**Q111.** A company recently migrated a message processing system to AWS. The system receives messages into an ActiveMQ queue running on an Amazon EC2 instance. Messages are processed by a consumer application running on Amazon EC2. The consumer application processes the messages and writes results to a MySQL database running on Amazon EC2. The company wants this application to be highly available with low operational complexity. Which architecture offers the HIGHEST availability?

- A. Add a second ActiveMQ server to another Availability Zone. Add an additional consumer EC2 instance in another Availability Zone. Replicate the MySQL database to another Availability Zone.
- B. Use Amazon MQ with active/standby brokers configured across two Availability Zones. Add an additional consumer EC2 instance in another Availability Zone. Replicate the MySQL database to another Availability Zone.
- C. Use Amazon MQ with active/standby brokers configured across two Availability Zones. Add an additional consumer EC2 instance in another Availability Zone. Use Amazon RDS for MySQL with Multi-AZ enabled.
- D. Use Amazon MQ with active/standby brokers configured across two Availability Zones. Add an Auto Scaling group for the consumer EC2 instances across two Availability Zones. Use Amazon RDS for MySQL with Multi-AZ enabled.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* This provides the highest availability: Amazon MQ (managed, active/standby across AZs), Auto Scaling group for consumers (multi-AZ, self-healing), and RDS Multi-AZ (managed, automatic failover). Option C lacks Auto Scaling for consumers. Options A and B use self-managed components or replication, which have higher operational complexity and lower availability guarantees (AWS Docs: High Availability Patterns).
</details>

---

**Q112.** A company hosts a containerized web application on a fleet of on-premises servers that process incoming requests. The number of requests is growing quickly. The on-premises servers cannot handle the increased number of requests. The company wants to move the application to AWS with minimum code changes and minimum development effort. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Fargate on Amazon Elastic Container Service (Amazon ECS) to run the containerized web application with Service Auto Scaling. Use an Application Load Balancer to distribute the incoming requests.
- B. Use two Amazon EC2 instances to host the containerized web application. Use an Application Load Balancer to distribute the incoming requests.
- C. Use AWS Lambda with a new code that uses one of the supported languages. Create multiple Lambda functions to support the load. Use Amazon API Gateway as an entry point to the Lambda functions.
- D. Use a high performance computing (HPC) solution such as AWS ParallelCluster to establish an HPC cluster that can process the incoming requests at the appropriate scale.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Fargate is a serverless container compute engine that minimizes operational overhead. You can run the existing containerized application with minimal changes, and Service Auto Scaling + ALB provides scalability. Option B requires managing EC2 instances; Option C requires code changes; Option D (HPC) is overkill (AWS Docs: AWS Fargate).
</details>

---

**Q113.** A company uses 50 TB of data for reporting. The company wants to move this data from on premises to AWS. A custom application in the company's data center runs a weekly data transformation job. The company plans to pause the application until the data transfer is complete and needs to begin the transfer process as soon as possible. The data center does not have any available network bandwidth for additional workloads. A solutions architect must transfer the data and must configure the transformation job to continue to run in the AWS Cloud. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS DataSync to move the data. Create a custom transformation job by using AWS Glue.
- B. Order an AWS Snowcone device to move the data. Deploy the transformation application to the device.
- C. Order an AWS Snowball Edge Storage Optimized device. Copy the data to the device. Create a custom transformation job by using AWS Glue.
- D. Order an AWS Snowball Edge Storage Optimized device that includes Amazon EC2 compute. Copy the data to the device. Create a new EC2 instance on AWS to run the transformation application.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Since there is no available network bandwidth, physical transfer (Snowball Edge) is required. Using a Snowball Edge Storage Optimized device with EC2 compute allows you to run the transformation application on the device during transfer. After transfer, you can run the job on AWS EC2. Option A (DataSync) uses bandwidth; Option B (Snowcone) has limited capacity (50 TB would require many devices); Option C (Glue) would require network to move data to Glue (AWS Docs: Snowball Edge Compute).
</details>

---

**Q114.** A company has created an image analysis application in which users can upload photos and add photo frames to their images. The users upload images and metadata to indicate which photo frames they want to add to their images. The application uses a single Amazon EC2 instance and Amazon DynamoDB to store the metadata. The application is becoming more popular, and the number of users is increasing. The company expects the number of concurrent users to vary significantly depending on the time of day and day of week. The company must ensure that the application can scale to meet the needs of the growing user base. Which solution meets these requirements?

- A. Use AWS Lambda to process the photos. Store the photos and metadata in DynamoDB.
- B. Use Amazon Kinesis Data Firehose to process the photos and to store the photos and metadata.
- C. Use AWS Lambda to process the photos. Store the photos in Amazon S3. Retain DynamoDB to store the metadata.
- D. Increase the number of EC2 instances to three. Use Provisioned IOPS SSD (io2) Amazon Elastic Block Store (Amazon EBS) volumes to store the photos and metadata.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* This solution separates storage: S3 for photos (scalable, durable), DynamoDB for metadata (scalable). Using Lambda for processing provides serverless scalability. Storing photos in DynamoDB (A) is expensive and inefficient. Firehose (B) is for streaming data delivery. Option D (EC2) has more operational overhead and less elasticity (AWS Docs: Serverless Image Processing).
</details>

---

**Q115.** A medical records company is hosting an application on Amazon EC2 instances. The application processes customer data files that are stored on Amazon S3. The EC2 instances are hosted in public subnets. The EC2 instances access Amazon S3 over the internet, but they do not require any other network access. A new requirement mandates that the network traffic for file transfers take a private route and not be sent over the internet. Which change to the network architecture should a solutions architect recommend to meet this requirement?

- A. Create a NAT gateway. Configure the route table for the public subnets to send traffic to Amazon S3 through the NAT gateway.
- B. Configure the security group for the EC2 instances to restrict outbound traffic so that only traffic to the S3 prefix list is permitted.
- C. Move the EC2 instances to private subnets. Create a VPC endpoint for Amazon S3, and link the endpoint to the route table for the private subnets.
- D. Remove the internet gateway from the VPC. Set up an AWS Direct Connect connection, and route traffic to Amazon S3 over the Direct Connect connection.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* To ensure traffic to S3 stays private, move EC2 instances to private subnets and create a VPC gateway endpoint for S3. This routes S3 traffic through the AWS network, not the internet. Option A (NAT) still routes through the internet; Option B (security group) doesn't change routing; Option D (Direct Connect) is overkill and expensive (AWS Docs: S3 VPC Endpoint).
</details>

---

**Q116.** A company uses a popular content management system (CMS) for its corporate website. However, the required patching and maintenance are burdensome. The company is redesigning its website and wants a new solution. The website will be updated four times a year and does not need to have any dynamic content available. The solution must provide high scalability and enhanced security. Which combination of changes will meet these requirements with the LEAST operational overhead? (Choose two.)

- A. Configure Amazon CloudFront in front of the website to use HTTPS functionality.
- B. Deploy an AWS WAF web ACL in front of the website to provide HTTPS functionality.
- C. Create and deploy an AWS Lambda function to manage and serve the website content.
- D. Create the new website and an Amazon S3 bucket. Deploy the website on the S3 bucket with static website hosting enabled.
- E. Create the new website. Deploy the website by using an Auto Scaling group of Amazon EC2 instances behind an Application Load Balancer.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* A static website hosted on S3 (D) eliminates patching/maintenance, is highly scalable, and cost-effective. CloudFront (A) provides HTTPS, edge caching, and enhanced security (DDoS protection, integration with AWS WAF). Option B (WAF) provides security but not HTTPS itself; Option C (Lambda) adds complexity; Option E (EC2) reintroduces maintenance overhead (AWS Docs: S3 Static Website + CloudFront).
</details>

---

**Q117.** A company stores its application logs in an Amazon CloudWatch Logs log group. A new policy requires the company to store all application logs in Amazon OpenSearch Service (Amazon Elasticsearch Service) in near-real time. Which solution will meet this requirement with the LEAST operational overhead?

- A. Configure a CloudWatch Logs subscription to stream the logs to Amazon OpenSearch Service (Amazon Elasticsearch Service).
- B. Create an AWS Lambda function. Use the log group to invoke the function to write the logs to Amazon OpenSearch Service (Amazon Elasticsearch Service).
- C. Create an Amazon Kinesis Data Firehose delivery stream. Configure the log group as the delivery stream's source. Configure Amazon OpenSearch Service (Amazon Elasticsearch Service) as the delivery stream's destination.
- D. Install and configure Amazon Kinesis Agent on each application server to deliver the logs to Amazon Kinesis Data Streams. Configure Kinesis Data Streams to deliver the logs to Amazon OpenSearch Service (Amazon Elasticsearch Service).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudWatch Logs subscriptions support direct streaming to Amazon OpenSearch Service (via Lambda or Kinesis, but the console provides a direct subscription). This is the least overhead. Option B requires a custom Lambda; Option C requires Kinesis Firehose; Option D requires installing agents (AWS Docs: CloudWatch Logs Subscription to OpenSearch).
</details>

---

**Q118.** A company is building a web-based application running on Amazon EC2 instances in multiple Availability Zones. The web application will provide access to a repository of text documents totaling about 900 TB in size. The company anticipates that the web application will experience periods of high demand. A solutions architect must ensure that the storage component for the text documents can scale to meet the demand of the application at all times. The company is concerned about the overall cost of the solution. Which storage solution meets these requirements MOST cost-effectively?

- A. Amazon Elastic Block Store (Amazon EBS)
- B. Amazon Elastic File System (Amazon EFS)
- C. Amazon OpenSearch Service (Amazon Elasticsearch Service)
- D. Amazon S3

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Amazon S3 is object storage that scales infinitely and is highly cost-effective for large volumes of data (900 TB). It can handle high demand periods. EBS (A) is block storage, not suitable for this scale/cost; EFS (B) is more expensive than S3 for this use case; OpenSearch (C) is for search/analytics, not primary storage (AWS Docs: S3 Scalability and Cost).
</details>

---

**Q119.** A global company is using Amazon API Gateway to design REST APIs for its loyalty club users in the us-east-1 Region and the ap-southeast-2 Region. A solutions architect must design a solution to protect these API Gateway managed REST APIs across multiple accounts from SQL injection and cross-site scripting attacks. Which solution will meet these requirements with the LEAST amount of administrative effort?

- A. Set up AWS WAF in both Regions. Associate Regional web ACLs with an API stage.
- B. Set up AWS Firewall Manager in both Regions. Centrally configure AWS WAF rules.
- C. Set up AWS Shield in both Regions. Associate Regional web ACLs with an API stage.
- D. Set up AWS Shield in one of the Regions. Associate Regional web ACLs with an API stage.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Firewall Manager allows you to centrally configure and manage AWS WAF rules across multiple accounts and Regions, reducing administrative effort. Option A requires managing WAF separately in each Region. Shield (C/D) is for DDoS protection, not SQL injection/XSS (AWS Docs: AWS Firewall Manager).
</details>

---

**Q120.** A company has implemented a self-managed DNS solution on three Amazon EC2 instances behind a Network Load Balancer (NLB) in the us-west-2 Region. Most of the company's users are located in the United States and Europe. The company wants to improve the performance and availability of the solution. The company launches and configures three EC2 instances in the eu-west-1 Region and adds the EC2 instances as targets for a new NLB. Which solution can the company use to route traffic to all the EC2 instances?

- A. Create an Amazon Route 53 geolocation routing policy to route requests to one of the two NLBs. Create an Amazon CloudFront distribution. Use the Route 53 record as the distribution's origin.
- B. Create a standard accelerator in AWS Global Accelerator. Create endpoint groups in us-west-2 and eu-west-1. Add the two NLBs as endpoints for the endpoint groups.
- C. Attach Elastic IP addresses to the six EC2 instances. Create an Amazon Route 53 geolocation routing policy to route requests to one of the six EC2 instances. Create an Amazon CloudFront distribution. Use the Route 53 record as the distribution's origin.
- D. Replace the two NLBs with two Application Load Balancers (ALBs). Create an Amazon Route 53 latency routing policy to route requests to one of the two ALBs. Create an Amazon CloudFront distribution. Use the Route 53 record as the distribution's origin.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Global Accelerator can route traffic to multiple endpoints (NLBs) across Regions, providing low latency, performance, and failover. Route 53 geolocation/latency (A/C/D) can work, but Global Accelerator is optimized for this and provides static IPs. CloudFront is not ideal for DNS protocol. (AWS Docs: Global Accelerator Multi-Region).
</details>

---

**Q121.** A company is running an online transaction processing (OLTP) workload on AWS. This workload uses an unencrypted Amazon RDS DB instance in a Multi-AZ deployment. Daily database snapshots are taken from this instance. What should a solutions architect do to ensure the database and snapshots are always encrypted moving forward?

- A. Encrypt a copy of the latest DB snapshot. Replace existing DB instance by restoring the encrypted snapshot.
- B. Create a new encrypted Amazon Elastic Block Store (Amazon EBS) volume and copy the snapshots to it. Enable encryption on the DB instance.
- C. Copy the snapshots and enable encryption using AWS Key Management Service (AWS KMS). Restore encrypted snapshot to an existing DB instance.
- D. Copy the snapshots to an Amazon S3 bucket that is encrypted using server-side encryption with AWS Key Management Service (AWS KMS) managed keys (SSE-KMS).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* To encrypt an unencrypted RDS instance, you must create an encrypted snapshot (by copying the latest snapshot with encryption enabled), then restore from that encrypted snapshot to a new encrypted DB instance. You cannot enable encryption on an existing unencrypted instance. Option B (EBS volume) is not applicable; Option C (restore to existing) is not possible; Option D (S3) is not a DB instance (AWS Docs: Encrypting RDS Snapshots).
</details>

---

**Q122.** A company wants to build a scalable key management infrastructure to support developers who need to encrypt data in their applications. What should a solutions architect do to reduce the operational burden?

- A. Use multi-factor authentication (MFA) to protect the encryption keys.
- B. Use AWS Key Management Service (AWS KMS) to protect the encryption keys.
- C. Use AWS Certificate Manager (ACM) to create, store, and assign the encryption keys.
- D. Use an IAM policy to limit the scope of users who have access permissions to protect the encryption keys.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS KMS is a managed service that simplifies the creation, management, and scaling of encryption keys, reducing operational burden. MFA (A) is an added security measure, not the infrastructure itself; ACM (C) is for SSL/TLS certificates, not general encryption keys; IAM (D) manages access but not the key infrastructure (AWS Docs: AWS KMS).
</details>

---

**Q123.** A company has a dynamic web application hosted on two Amazon EC2 instances. The company has its own SSL certificate, which is on each instance to perform SSL termination. There has been an increase in traffic recently, and the operations team determined that SSL encryption and decryption is causing the compute capacity of the web servers to reach their maximum limit. What should a solutions architect do to increase the application's performance?

- A. Create a new SSL certificate using AWS Certificate Manager (ACM). Install the ACM certificate on each instance.
- B. Create an Amazon S3 bucket. Migrate the SSL certificate to the S3 bucket. Configure the EC2 instances to reference the bucket for SSL termination.
- C. Create another EC2 instance as a proxy server. Migrate the SSL certificate to the new instance and configure it to direct connections to the existing EC2 instances.
- D. Import the SSL certificate into AWS Certificate Manager (ACM). Create an Application Load Balancer with an HTTPS listener that uses the SSL certificate from ACM.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Offloading SSL termination to an Application Load Balancer (ALB) removes the CPU-intensive encryption/decryption workload from the EC2 instances, improving their performance. ACM manages the certificate. Options A and C still have instances doing SSL work; Option B (S3) is not feasible (AWS Docs: ALB SSL Offloading).
</details>

---

**Q124.** A company has a highly dynamic batch processing job that uses many Amazon EC2 instances to complete it. The job is stateless in nature, can be started and stopped at any given time with no negative impact, and typically takes upwards of 60 minutes total to complete. The company has asked a solutions architect to design a scalable and cost-effective solution that meets the requirements of the job. What should the solutions architect recommend?

- A. Implement EC2 Spot Instances.
- B. Purchase EC2 Reserved Instances.
- C. Implement EC2 On-Demand Instances.
- D. Implement the processing on AWS Lambda.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* The job is stateless, fault-tolerant, and can be interrupted. Spot Instances offer significant cost savings (up to 90%) compared to On-Demand. Reserved Instances (B) are for steady-state, not dynamic. Lambda (D) has a 15-minute timeout limit, insufficient for a 60-minute job (AWS Docs: Spot Instances for Batch).
</details>

---

**Q125.** A company runs its two-tier ecommerce website on AWS. The web tier consists of a load balancer that sends traffic to Amazon EC2 instances. The database tier uses an Amazon RDS DB instance. The EC2 instances and the RDS DB instance should not be exposed to the public internet. The EC2 instances require internet access to complete payment processing of orders through a third-party web service. The application must be highly available. Which combination of configuration options will meet these requirements? (Choose two.)

- A. Use an Auto Scaling group to launch the EC2 instances in private subnets. Deploy an RDS Multi-AZ DB instance in private subnets.
- B. Configure a VPC with two private subnets and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the private subnets.
- C. Use an Auto Scaling group to launch the EC2 instances in public subnets across two Availability Zones. Deploy an RDS Multi-AZ DB instance in private subnets.
- D. Configure a VPC with one public subnet, one private subnet, and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the public subnet.
- E. Configure a VPC with two public subnets, two private subnets, and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the public subnets.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* EC2 instances in private subnets (A) with Auto Scaling. RDS Multi-AZ in private subnets (A). For internet access (payment processing), NAT gateways in public subnets. ALB must be in public subnets to receive internet traffic (E). Option B has ALB in private (won't receive internet traffic). Option C has EC2 in public (exposed). Option D has only one public subnet (not HA for ALB). (AWS Docs: Highly Available Web App Architecture).
</details>

---

**Q126.** A solutions architect needs to implement a solution to reduce a company's storage costs. All the company's data is in the Amazon S3 Standard storage class. The company must keep all data for at least 25 years. Data from the most recent 2 years must be highly available and immediately retrievable. Which solution will meet these requirements?

- A. Set up an S3 Lifecycle policy to transition objects to S3 Glacier Deep Archive immediately.
- B. Set up an S3 Lifecycle policy to transition objects to S3 Glacier Deep Archive after 2 years.
- C. Use S3 Intelligent-Tiering. Activate the archiving option to ensure that data is archived in S3 Glacier Deep Archive.
- D. Set up an S3 Lifecycle policy to transition objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) immediately and to S3 Glacier Deep Archive after 2 years.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Data from the last 2 years must be in a highly available, immediately retrievable storage class (S3 Standard). After 2 years, it can be moved to S3 Glacier Deep Archive for long-term, low-cost retention. Option A archives immediately (not HA for recent data); Option C (Intelligent-Tiering) is more expensive than a simple lifecycle rule; Option D (One Zone-IA) is not as resilient (AWS Docs: S3 Lifecycle Rules).
</details>

---

**Q127.** A media company is evaluating the possibility of moving its systems to the AWS Cloud. The company needs at least 10 TB of storage with the maximum possible I/O performance for video processing, 300 TB of very durable storage for storing media content, and 900 TB of storage to meet requirements for archival media that is not in use anymore. Which set of services should a solutions architect recommend to meet these requirements?

- A. Amazon EBS for maximum performance, Amazon S3 for durable data storage, and Amazon S3 Glacier for archival storage
- B. Amazon EBS for maximum performance, Amazon EFS for durable data storage, and Amazon S3 Glacier for archival storage
- C. Amazon EC2 instance store for maximum performance, Amazon EFS for durable data storage, and Amazon S3 for archival storage
- D. Amazon EC2 instance store for maximum performance, Amazon S3 for durable data storage, and Amazon S3 Glacier for archival storage

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* EC2 Instance Store provides maximum I/O performance for video processing (ephemeral). S3 provides high durability for media content. S3 Glacier provides low-cost archival storage. EBS (A/B) has lower max I/O than instance store; EFS (B/C) is not as durable as S3 for 300 TB (S3 is 11 9's durability). (AWS Docs: Storage Services Comparison).
</details>

---

**Q128.** A company wants to run applications in containers in the AWS Cloud. These applications are stateless and can tolerate disruptions within the underlying infrastructure. The company needs a solution that minimizes cost and operational overhead. What should a solutions architect do to meet these requirements?

- A. Use Spot Instances in an Amazon EC2 Auto Scaling group to run the application containers.
- B. Use Spot Instances in an Amazon Elastic Kubernetes Service (Amazon EKS) managed node group.
- C. Use On-Demand Instances in an Amazon EC2 Auto Scaling group to run the application containers.
- D. Use On-Demand Instances in an Amazon Elastic Kubernetes Service (Amazon EKS) managed node group.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* EKS with Spot Instances provides a managed Kubernetes control plane (operational overhead) and low-cost, interruptible worker nodes suitable for stateless, fault-tolerant applications. Option A (EC2 Auto Scaling group) requires managing the container orchestration yourself. Options C/D (On-Demand) are more expensive. (AWS Docs: EKS Managed Node Groups with Spot).
</details>

---

**Q129.** A company is running a multi-tier web application on premises. The web application is containerized and runs on a number of Linux hosts connected to a PostgreSQL database that contains user records. The operational overhead of maintaining the infrastructure and capacity planning is limiting the company's growth. A solutions architect must improve the application's infrastructure. Which combination of actions should the solutions architect take to accomplish this? (Choose two.)

- A. Migrate the PostgreSQL database to Amazon Aurora.
- B. Migrate the web application to be hosted on Amazon EC2 instances.
- C. Set up an Amazon CloudFront distribution for the web application content.
- D. Set up Amazon ElastiCache between the web application and the PostgreSQL database.
- E. Migrate the web application to be hosted on AWS Fargate with Amazon Elastic Container Service (Amazon ECS).

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* Migrating to Aurora (A) reduces database management overhead (automatic scaling, patching, backups). Migrating the containerized app to AWS Fargate (E) removes the need to manage the underlying hosts/containers. Option B (EC2) doesn't reduce overhead; C (CloudFront) improves performance but not infrastructure overhead; D (ElastiCache) adds complexity (AWS Docs: Modernization with Aurora and Fargate).
</details>

---

**Q130.** An application runs on Amazon EC2 instances across multiple Availability Zones. The instances run in an Amazon EC2 Auto Scaling group behind an Application Load Balancer. The application performs best when the CPU utilization of the EC2 instances is at or near 40%. What should a solutions architect do to maintain the desired performance across all instances in the group?

- A. Use a simple scaling policy to dynamically scale the Auto Scaling group.
- B. Use a target tracking policy to dynamically scale the Auto Scaling group.
- C. Use an AWS Lambda function to update the desired Auto Scaling group capacity.
- D. Use scheduled scaling actions to scale up and scale down the Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Target tracking scaling policies automatically adjust the ASG capacity to maintain a specific metric average (e.g., average CPU utilization at 40%). This is the most direct and automated way. Simple scaling policies (A) react to breaches but don't maintain a target; scheduled scaling (D) is for predictable patterns; Lambda (C) adds overhead (AWS Docs: Target Tracking Scaling Policies).
</details>

---

**Q131.** A company is developing a file-sharing application that will use an Amazon S3 bucket for storage. The company wants to serve all the files through an Amazon CloudFront distribution. The company does not want the files to be accessible through direct navigation to the S3 URL. What should a solutions architect do to meet these requirements?

- A. Write individual policies for each S3 bucket to grant read permission for only CloudFront access.
- B. Create an IAM user. Grant the user read permission to objects in the S3 bucket. Assign the user to CloudFront.
- C. Write an S3 bucket policy that assigns the CloudFront distribution ID as the Principal and assigns the target S3 bucket as the Amazon Resource Name (ARN).
- D. Create an origin access identity (OAI). Assign the OAI to the CloudFront distribution. Configure the S3 bucket permissions so that only the OAI has read permission.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* An Origin Access Identity (OAI) is a special CloudFront user. By restricting S3 bucket access to only the OAI (via bucket policy), you ensure that files can only be accessed via CloudFront, not directly via S3 URLs. Option C is incorrect syntax; Options A/B are not standard patterns (AWS Docs: Restricting Access to S3 Origin).
</details>

---

**Q132.** A company's website provides users with downloadable historical performance reports. The website needs a solution that will scale to meet the company's website demands globally. The solution should be cost-effective, limit the provisioning of infrastructure resources, and provide the fastest possible response time. Which combination should a solutions architect recommend to meet these requirements?

- A. Amazon CloudFront and Amazon S3
- B. AWS Lambda and Amazon DynamoDB
- C. Application Load Balancer with Amazon EC2 Auto Scaling
- D. Amazon Route 53 with internal Application Load Balancers

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudFront provides edge caching for fast global response times, and S3 provides scalable, cost-effective storage for static files (reports). This requires no infrastructure provisioning. Lambda/DynamoDB (B) is serverless but more complex for static files; EC2/ALB (C) requires provisioning; Route 53 with internal ALBs (D) doesn't serve content globally (AWS Docs: CloudFront + S3 for Static Content).
</details>

---

**Q133.** A company runs an Oracle database on premises. As part of the company's migration to AWS, the company wants to upgrade the database to the most recent available version. The company also wants to set up disaster recovery (DR) for the database. The company needs to minimize the operational overhead for normal operations and DR setup. The company also needs to maintain access to the database's underlying operating system. Which solution will meet these requirements?

- A. Migrate the Oracle database to an Amazon EC2 instance. Set up database replication to a different AWS Region.
- B. Migrate the Oracle database to Amazon RDS for Oracle. Activate Cross-Region automated backups to replicate the snapshots to another AWS Region.
- C. Migrate the Oracle database to Amazon RDS Custom for Oracle. Create a read replica for the database in another AWS Region.
- D. Migrate the Oracle database to Amazon RDS for Oracle. Create a standby database in another Availability Zone.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* RDS Custom for Oracle provides access to the underlying operating system (required), while still offloading some management tasks. It supports cross-Region read replicas for DR. Option A (EC2) has high operational overhead; Option B (RDS) does not provide OS access; Option D (standby in same Region) is not cross-Region DR (AWS Docs: RDS Custom for Oracle).
</details>

---

**Q134.** A company wants to move its application to a serverless solution. The serverless solution needs to analyze existing and new data by using SQL. The company stores the data in an Amazon S3 bucket. The data requires encryption and must be replicated to a different AWS Region. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create a new S3 bucket. Load the data into the new S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with AWS KMS multi-Region keys (SSE-KMS). Use Amazon Athena to query the data.
- B. Create a new S3 bucket. Load the data into the new S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with AWS KMS multi-Region keys (SSE-KMS). Use Amazon RDS to query the data.
- C. Load the data into the existing S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use Amazon Athena to query the data.
- D. Load the data into the existing S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use Amazon RDS to query the data.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Athena provides serverless SQL querying directly on S3 data. CRR with SSE-KMS multi-Region keys ensures encrypted replication. SSE-S3 (C/D) does not allow cross-Region replication of encrypted objects with customer-managed keys. RDS (B/D) is not serverless for this purpose (AWS Docs: Athena on S3, CRR with KMS).
</details>

---

**Q135.** A company runs workloads on AWS. The company needs to connect to a service from an external provider. The service is hosted in the provider's VPC. According to the company's security team, the connectivity must be private and must be restricted to the target service. The connection must be initiated only from the company's VPC. Which solution will meet these requirements?

- A. Create a VPC peering connection between the company's VPC and the provider's VPC. Update the route table to connect to the target service.
- B. Ask the provider to create a virtual private gateway in its VPC. Use AWS PrivateLink to connect to the target service.
- C. Create a NAT gateway in a public subnet of the company's VPC. Update the route table to connect to the target service.
- D. Ask the provider to create a VPC endpoint for the target service. Use AWS PrivateLink to connect to the target service.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS PrivateLink (VPC endpoint services) allows you to connect to a service in another VPC privately, without requiring VPC peering, internet gateways, or VPN. The provider creates a VPC endpoint service (powered by Network Load Balancer), and you create an interface endpoint in your VPC. This meets the "restricted to target service" requirement (AWS Docs: AWS PrivateLink).
</details>

---

**Q136.** A company is migrating its on-premises PostgreSQL database to Amazon Aurora PostgreSQL. The on-premises database must remain online and accessible during the migration. The Aurora database must remain synchronized with the on-premises database. Which combination of actions must a solutions architect take to meet these requirements? (Choose two.)

- A. Create an ongoing replication task.
- B. Create a database backup of the on-premises database.
- C. Create an AWS Database Migration Service (AWS DMS) replication server.
- D. Convert the database schema by using the AWS Schema Conversion Tool (AWS SCT).
- E. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to monitor the database synchronization.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* AWS DMS can perform ongoing replication (change data capture) from an on-premises source to an AWS target. You need a replication server (C) and an ongoing replication task (A). Schema Conversion Tool (D) is needed for heterogeneous migrations (e.g., Oracle to Aurora), but PostgreSQL to Aurora PostgreSQL is homogeneous. Backup (B) alone doesn't provide ongoing sync. (AWS Docs: DMS Ongoing Replication).
</details>

---

**Q137.** A company uses AWS Organizations to create dedicated AWS accounts for each business unit to manage each business unit's account independently upon request. The root email recipient missed a notification that was sent to the root user email address of one account. The company wants to ensure that all future notifications are not missed. Future notifications must be limited to account administrators. Which solution will meet these requirements?

- A. Configure the company's email server to forward notification email messages that are sent to the AWS account root user email address to all users in the organization.
- B. Configure all AWS account root user email addresses as distribution lists that go to a few administrators who can respond to alerts. Configure AWS account alternate contacts in the AWS Organizations console or programmatically.
- C. Configure all AWS account root user email messages to be sent to one administrator who is responsible for monitoring alerts and forwarding those alerts to the appropriate groups.
- D. Configure all existing AWS accounts and all newly created accounts to use the same root user email address. Configure AWS account alternate contacts in the AWS Organizations console or programmatically.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Setting root user email addresses as distribution lists ensures multiple administrators receive notifications. Additionally, configuring alternate contacts (billing, operations, security) in AWS Organizations provides a more targeted and manageable way to route notifications to the right teams, without using the root user email for all. Option D (same root email) is insecure and not best practice (AWS Docs: Managing AWS Account Alternate Contacts).
</details>

---

**Q138.** A company runs its ecommerce application on AWS. Every new order is published as a message in a RabbitMQ queue that runs on an Amazon EC2 instance in a single Availability Zone. These messages are processed by a different application that runs on a separate EC2 instance. This application stores the details in a PostgreSQL database on another EC2 instance. All the EC2 instances are in the same Availability Zone. The company needs to redesign its architecture to provide the highest availability with the least operational overhead. What should a solutions architect do to meet these requirements?

- A. Migrate the queue to a redundant pair (active/standby) of RabbitMQ instances on Amazon MQ. Create a Multi-AZ Auto Scaling group for EC2 instances that host the application. Create another Multi-AZ Auto Scaling group for EC2 instances that host the PostgreSQL database.
- B. Migrate the queue to a redundant pair (active/standby) of RabbitMQ instances on Amazon MQ. Create a Multi-AZ Auto Scaling group for EC2 instances that host the application. Migrate the database to run on a Multi-AZ deployment of Amazon RDS for PostgreSQL.
- C. Create a Multi-AZ Auto Scaling group for EC2 instances that host the RabbitMQ queue. Create another Multi-AZ Auto Scaling group for EC2 instances that host the application. Migrate the database to run on a Multi-AZ deployment of Amazon RDS for PostgreSQL.
- D. Create a Multi-AZ Auto Scaling group for EC2 instances that host the RabbitMQ queue. Create another Multi-AZ Auto Scaling group for EC2 instances that host the application. Create a third Multi-AZ Auto Scaling group for EC2 instances that host the PostgreSQL database.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* This solution uses managed services for the queue (Amazon MQ active/standby) and database (RDS Multi-AZ), reducing operational overhead. The application can run in a Multi-AZ Auto Scaling group for high availability. Option A uses EC2 for database (more overhead); Options C and D use self-managed RabbitMQ on EC2 (more overhead) (AWS Docs: Amazon MQ High Availability, RDS Multi-AZ).
</details>

---

**Q139.** A reporting team receives files each day in an Amazon S3 bucket. The reporting team manually reviews and copies the files from this initial S3 bucket to an analysis S3 bucket each day at the same time to use with Amazon QuickSight. Additional teams are starting to send more files in larger sizes to the initial S3 bucket. The reporting team wants to move the files automatically to the analysis S3 bucket as the files enter the initial S3 bucket. The reporting team also wants to use AWS Lambda functions to run pattern-matching code on the copied data. In addition, the reporting team wants to send the data files to a pipeline in Amazon SageMaker Pipelines. What should a solutions architect do to meet these requirements with the LEAST operational overhead?

- A. Create a Lambda function to copy the files to the analysis S3 bucket. Create an S3 event notification for the analysis S3 bucket. Configure Lambda and SageMaker Pipelines as destinations of the event notification. Configure s3:ObjectCreated:Put as the event type.
- B. Create a Lambda function to copy the files to the analysis S3 bucket. Configure the analysis S3 bucket to send event notifications to Amazon EventBridge (Amazon CloudWatch Events). Configure an ObjectCreated rule in EventBridge (CloudWatch Events). Configure Lambda and SageMaker Pipelines as targets for the rule.
- C. Configure S3 replication between the S3 buckets. Create an S3 event notification for the analysis S3 bucket. Configure Lambda and SageMaker Pipelines as destinations of the event notification. Configure s3:ObjectCreated:Put as the event type.
- D. Configure S3 replication between the S3 buckets. Configure the analysis S3 bucket to send event notifications to Amazon EventBridge (Amazon CloudWatch Events). Configure an ObjectCreated rule in EventBridge (CloudWatch Events). Configure Lambda and SageMaker Pipelines as targets for the rule.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 replication automatically copies objects from the initial bucket to the analysis bucket with no custom Lambda code. EventBridge can then listen for `ObjectCreated` events on the analysis bucket and trigger both Lambda and SageMaker Pipelines. This is the least operational overhead. Options A and B require a custom Lambda copy function (AWS Docs: S3 Replication, EventBridge for S3).
</details>

---

**Q140.** A solutions architect needs to help a company optimize the cost of running an application on AWS. The application will use Amazon EC2 instances, AWS Fargate, and AWS Lambda for compute within the architecture. The EC2 instances will run the data ingestion layer of the application. EC2 usage will be sporadic and unpredictable. Workloads that run on EC2 instances can be interrupted at any time. The application front end will run on Fargate, and Lambda will serve the API layer. The front-end utilization and API layer utilization will be predictable over the course of the next year. Which combination of purchasing options will provide the MOST cost-effective solution for hosting this application? (Choose two.)

- A. Use Spot Instances for the data ingestion layer
- B. Use On-Demand Instances for the data ingestion layer
- C. Purchase a 1-year Compute Savings Plan for the front end and API layer.
- D. Purchase 1-year All Upfront Reserved instances for the data ingestion layer.
- E. Purchase a 1-year EC2 instance Savings Plan for the front end and API layer.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* The data ingestion layer is sporadic, unpredictable, and interruptible -> Spot Instances (A) are cheapest. Fargate and Lambda are serverless; a Compute Savings Plan (C) applies to Fargate and Lambda usage (and also EC2) providing a discount for predictable usage, and is flexible across instance families/regions. Option E (EC2 Instance Savings Plan) does not cover Fargate/Lambda. (AWS Docs: Compute Savings Plan).
</details>

---

**Q141.** A company runs a web-based portal that provides users with global breaking news, local alerts, and weather updates. The portal delivers each user a personalized view by using a mixture of static and dynamic content. Content is served over HTTPS through an API server running on an Amazon EC2 instance behind an Application Load Balancer (ALB). The company wants the portal to provide this content to its users across the world as quickly as possible. How should a solutions architect design the application to ensure the LEAST amount of latency for all users?

- A. Deploy the application stack in a single AWS Region. Use Amazon CloudFront to serve all static and dynamic content by specifying the ALB as an origin.
- B. Deploy the application stack in two AWS Regions. Use an Amazon Route 53 latency routing policy to serve all content from the ALB in the closest Region.
- C. Deploy the application stack in a single AWS Region. Use Amazon CloudFront to serve the static content. Serve the dynamic content directly from the ALB.
- D. Deploy the application stack in two AWS Regions. Use an Amazon Route 53 geolocation routing policy to serve all content from the ALB in the closest Region.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudFront can serve both static and dynamic content via a single distribution with the ALB as an origin. It caches static content at edge locations and optimizes dynamic content paths. This provides global low latency without deploying the application stack in multiple Regions. Multi-Region (B/D) adds complexity. Option C splits static/dynamic but dynamic still from single Region (AWS Docs: CloudFront Dynamic Content Acceleration).
</details>

---

**Q142.** A gaming company is designing a highly available architecture. The application runs on a modified Linux kernel and supports only UDP-based traffic. The company needs the front-end tier to provide the best possible user experience. That tier must have low latency, route traffic to the nearest edge location, and provide static IP addresses for entry into the application endpoints. What should a solutions architect do to meet these requirements?

- A. Configure Amazon Route 53 to forward requests to an Application Load Balancer. Use AWS Lambda for the application in AWS Application Auto Scaling.
- B. Configure Amazon CloudFront to forward requests to a Network Load Balancer. Use AWS Lambda for the application in an AWS Application Auto Scaling group.
- C. Configure AWS Global Accelerator to forward requests to a Network Load Balancer. Use Amazon EC2 instances for the application in an EC2 Auto Scaling group.
- D. Configure Amazon API Gateway to forward requests to an Application Load Balancer. Use Amazon EC2 instances for the application in an EC2 Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Global Accelerator supports UDP, provides static IP addresses, routes traffic to nearest edge location, and integrates with NLBs. EC2 instances are needed for the custom Linux kernel/UDP application. ALBs (A, D) do not support UDP. CloudFront (B) does not support UDP for origins. (AWS Docs: Global Accelerator with UDP).
</details>

---

**Q143.** A company wants to migrate its existing on-premises monolithic application to AWS. The company wants to keep as much of the front-end code and the backend code as possible. However, the company wants to break the application into smaller applications. A different team will manage each application. The company needs a highly scalable solution that minimizes operational overhead. Which solution will meet these requirements?

- A. Host the application on AWS Lambda. Integrate the application with Amazon API Gateway.
- B. Host the application with AWS Amplify. Connect the application to an Amazon API Gateway API that is integrated with AWS Lambda.
- C. Host the application on Amazon EC2 instances. Set up an Application Load Balancer with EC2 instances in an Auto Scaling group as targets.
- D. Host the application on Amazon Elastic Container Service (Amazon ECS). Set up an Application Load Balancer with Amazon ECS as the target.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Breaking a monolithic app into smaller applications managed by different teams suggests a microservices architecture. ECS with containers provides isolation, scalability, and lower operational overhead than EC2 (C). Lambda (A, B) may require significant code changes. ECS allows you to containerize existing code with fewer changes (AWS Docs: Microservices on ECS).
</details>

---

**Q144.** A company recently started using Amazon Aurora as the data store for its global ecommerce application. When large reports are run, developers report that the ecommerce application is performing poorly. After reviewing metrics in Amazon CloudWatch, a solutions architect finds that the ReadIOPS and CPUUtilization metrics are spiking when monthly reports run. What is the MOST cost-effective solution?

- A. Migrate the monthly reporting to Amazon Redshift.
- B. Migrate the monthly reporting to an Aurora Replica.
- C. Migrate the Aurora database to a larger instance class.
- D. Increase the Provisioned IOPS on the Aurora instance.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Offloading the reporting queries to an Aurora Replica (read replica) removes the read load from the primary writer instance, eliminating the performance spike on the production application. This is more cost-effective than scaling up the primary instance (C/D) or migrating to Redshift (A). (AWS Docs: Aurora Replicas).
</details>

---

**Q145.** A company hosts a website analytics application on a single Amazon EC2 On-Demand Instance. The analytics software is written in PHP and uses a MySQL database. The analytics software, the web server that provides PHP, and the database server are all hosted on the EC2 instance. The application is showing signs of performance degradation during busy times and is presenting 5xx errors. The company needs to make the application scale seamlessly. Which solution will meet these requirements MOST cost-effectively?

- A. Migrate the database to an Amazon RDS for MySQL DB instance. Create an AMI of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use an Application Load Balancer to distribute the load to each EC2 instance.
- B. Migrate the database to an Amazon RDS for MySQL DB instance. Create an AMI of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use Amazon Route 53 weighted routing to distribute the load across the two EC2 instances.
- C. Migrate the database to an Amazon Aurora MySQL DB instance. Create an AWS Lambda function to stop the EC2 instance and change the instance type. Create an Amazon CloudWatch alarm to invoke the Lambda function when CPU utilization surpasses 75%.
- D. Migrate the database to an Amazon Aurora MySQL DB instance. Create an AMI of the web application. Apply the AMI to a launch template. Create an Auto Scaling group with the launch template. Configure the launch template to use a Spot Fleet. Attach an Application Load Balancer to the Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* The simplest and most cost-effective way to scale a PHP/MySQL application is to separate the database (RDS) and horizontally scale the web/application tier using an ALB and multiple EC2 instances. Option B (Route 53 weighted) doesn't provide health checks and session management as well as ALB. Option C doesn't scale out. Option D (Spot Fleet) adds complexity and potential interruption (AWS Docs: Scaling Web Application Tiers).
</details>

---

**Q146.** A company runs a stateless web application in production on a group of Amazon EC2 On-Demand Instances behind an Application Load Balancer. The application experiences heavy usage during an 8-hour period each business day. Application usage is moderate and steady overnight. Application usage is low during weekends. The company wants to minimize its EC2 costs without affecting the availability of the application. Which solution will meet these requirements?

- A. Use Spot Instances for the entire workload.
- B. Use Reserved Instances for the baseline level of usage. Use Spot instances for any additional capacity that the application needs.
- C. Use On-Demand Instances for the baseline level of usage. Use Spot Instances for any additional capacity that the application needs.
- D. Use Dedicated Instances for the baseline level of usage. Use On-Demand Instances for any additional capacity that the application needs.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Reserved Instances provide a significant discount for the baseline (moderate/steady) usage. Spot Instances can be used for the heavy 8-hour period if the application is stateless and fault-tolerant. Option A risks interruption during baseline. Option C (On-Demand for baseline) is more expensive than Reserved. Option D (Dedicated) is more expensive. (AWS Docs: EC2 Pricing Models).
</details>

---

**Q147.** A company needs to retain application log files for a critical application for 10 years. The application team regularly accesses logs from the past month for troubleshooting, but logs older than 1 month are rarely accessed. The application generates more than 10 TB of logs per month. Which storage option meets these requirements MOST cost-effectively?

- A. Store the logs in Amazon S3. Use AWS Backup to move logs more than 1 month old to S3 Glacier Deep Archive.
- B. Store the logs in Amazon S3. Use S3 Lifecycle policies to move logs more than 1 month old to S3 Glacier Deep Archive.
- C. Store the logs in Amazon CloudWatch Logs. Use AWS Backup to move logs more than 1 month old to S3 Glacier Deep Archive.
- D. Store the logs in Amazon CloudWatch Logs. Use Amazon S3 Lifecycle policies to move logs more than 1 month old to S3 Glacier Deep Archive.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 is cost-effective for large log volumes. S3 Lifecycle policies are the native, efficient way to transition objects to S3 Glacier Deep Archive after 1 month. CloudWatch Logs (C/D) is more expensive for long-term storage of 10 TB/month, and doesn't directly use S3 Lifecycle policies (AWS Docs: S3 Lifecycle Transitions).
</details>

---

**Q148.** A company has a data ingestion workflow that includes the following components: An Amazon Simple Notification Service (Amazon SNS) topic that receives notifications about new data deliveries. An AWS Lambda function that processes and stores the data. The ingestion workflow occasionally fails because of network connectivity issues. When failure occurs, the corresponding data is not ingested unless the company manually reruns the job. What should a solutions architect do to ensure that all notifications are eventually processed?

- A. Configure the Lambda function for deployment across multiple Availability Zones.
- B. Modify the Lambda function's configuration to increase the CPU and memory allocations for the function.
- C. Configure the SNS topic's retry strategy to increase both the number of retries and the wait time between retries.
- D. Configure an Amazon Simple Queue Service (Amazon SQS) queue as the on-failure destination. Modify the Lambda function to process messages in the queue.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* SNS can send messages to a dead-letter queue (SQS) if delivery to the Lambda endpoint fails. Then, the Lambda function can be triggered from the SQS queue, providing durable, retryable processing. Option C (SNS retry) has limited retries and may still fail; Options A and B don't address the fundamental decoupling/durability issue (AWS Docs: SNS Dead-Letter Queues).
</details>

---

**Q149.** A company has a service that produces event data. The company wants to use AWS to process the event data as it is received. The data is written in a specific order that must be maintained throughout processing. The company wants to implement a solution that minimizes operational overhead. How should a solutions architect accomplish this?

- A. Create an Amazon Simple Queue Service (Amazon SQS) FIFO queue to hold messages. Set up an AWS Lambda function to process messages from the queue.
- B. Create an Amazon Simple Notification Service (Amazon SNS) topic to deliver notifications containing payloads to process. Configure an AWS Lambda function as a subscriber.
- C. Create an Amazon Simple Queue Service (Amazon SQS) standard queue to hold messages. Set up an AWS Lambda function to process messages from the queue independently.
- D. Create an Amazon Simple Notification Service (Amazon SNS) topic to deliver notifications containing payloads to process. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a subscriber.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* SQS FIFO queues guarantee first-in, first-out delivery and exactly-once processing (with consumer idempotency). This maintains the required order. Lambda can be configured as an event source mapping for the FIFO queue. SNS (B/D) does not guarantee order; SQS standard (C) does not guarantee order (AWS Docs: SQS FIFO Queues).
</details>

---

**Q150.** A company is migrating an application from on-premises servers to Amazon EC2 instances. As part of the migration design requirements, a solutions architect must implement infrastructure metric alarms. The company does not need to take action if CPU utilization increases to more than 50% for a short burst of time. However, if the CPU utilization increases to more than 50% and read IOPS on the disk are high at the same time, the company needs to act as soon as possible. The solutions architect also must reduce false alarms. What should the solutions architect do to meet these requirements?

- A. Create Amazon CloudWatch composite alarms where possible.
- B. Create Amazon CloudWatch dashboards to visualize the metrics and react to issues quickly.
- C. Create Amazon CloudWatch Synthetics canaries to monitor the application and raise an alarm.
- D. Create single Amazon CloudWatch metric alarms with multiple metric thresholds where possible.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudWatch composite alarms combine multiple metric alarms (e.g., CPU > 50% AND ReadIOPS > threshold) into a single alarm, reducing false alarms. This meets the requirement for conditional alerting. Option B (dashboards) is for visualization, not alerting; Option C (canaries) is for synthetic monitoring; Option D (single alarm) cannot combine conditions natively (AWS Docs: CloudWatch Composite Alarms).
</details>

---

**Q151.** A company wants to migrate its on-premises data center to AWS. According to the company's compliance requirements, the company can use only the ap-northeast-3 Region. Company administrators are not permitted to connect VPCs to the internet. Which solutions will meet these requirements? (Choose two.)

- A. Use AWS Control Tower to implement data residency guardrails to deny internet access and deny access to all AWS Regions except ap-northeast-3.
- B. Use rules in AWS WAF to prevent internet access. Deny access to all AWS Regions except ap-northeast-3 in the AWS account settings.
- C. Use AWS Organizations to configure service control policies (SCPs) that prevent VPCs from gaining internet access. Deny access to all AWS Regions except ap-northeast-3.
- D. Create an outbound rule for the network ACL in each VPC to deny all traffic from 0.0.0.0/0. Create an IAM policy for each user to prevent the use of any AWS Region other than ap-northeast-3.
- E. Use AWS Config to activate managed rules to detect and alert for internet gateways and to detect and alert for new resources deployed outside of ap-northeast-3.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* SCPs (C) can be used to deny actions that create internet gateways or VPCs with public subnets, and to restrict access to other Regions. AWS Control Tower (A) provides guardrails (which use SCPs) for data residency and internet access. Option B (WAF) is for web app filtering, not VPC internet access; Option D (NACL) is per-VPC and easy to misconfigure; Option E (Config) only detects/alerts, doesn't prevent. (AWS Docs: SCPs for Region Restriction).
</details>

---

**Q152.** A company uses a three-tier web application to provide training to new employees. The application is accessed for only 12 hours every day. The company is using an Amazon RDS for MySQL DB instance to store information and wants to minimize costs. What should a solutions architect do to meet these requirements?

- A. Configure an IAM policy for AWS Systems Manager Session Manager. Create an IAM role for the policy. Update the trust relationship of the role. Set up automatic start and stop for the DB instance.
- B. Create an Amazon ElastiCache for Redis cache cluster that gives users the ability to access the data from the cache when the DB instance is stopped. Invalidate the cache after the DB instance is started.
- C. Launch an Amazon EC2 instance. Create an IAM role that grants access to Amazon RDS. Attach the role to the EC2 instance. Configure a cron job to start and stop the EC2 instance on the desired schedule.
- D. Create AWS Lambda functions to start and stop the DB instance. Create Amazon EventBridge (Amazon CloudWatch Events) scheduled rules to invoke the Lambda functions. Configure the Lambda functions as event targets for the rules.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Using Lambda and EventBridge to automatically start and stop the RDS instance outside the 12-hour access window minimizes costs. This is serverless and operationally efficient. Option A (Session Manager) is for access, not scheduling; Option B (ElastiCache) doesn't replace the database; Option C (EC2) requires managing an instance (AWS Docs: Stop/Start RDS with Lambda).
</details>

---

**Q153.** A company sells ringtones created from clips of popular songs. The files containing the ringtones are stored in Amazon S3 Standard and are at least 128 KB in size. The company has millions of files, but downloads are infrequent for ringtones older than 90 days. The company needs to save money on storage while keeping the most accessed files readily available for its users. Which action should the company take to meet these requirements MOST cost-effectively?

- A. Configure S3 Standard-Infrequent Access (S3 Standard-IA) storage for the initial storage tier of the objects.
- B. Move the files to S3 Intelligent-Tiering and configure it to move objects to a less expensive storage tier after 90 days.
- C. Configure S3 inventory to manage objects and move them to S3 Standard-Infrequent Access (S3 Standard-IA) after 90 days.
- D. Implement an S3 Lifecycle policy that moves the objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 90 days.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* A lifecycle policy to transition objects from S3 Standard to S3 Standard-IA after 90 days is the most straightforward and cost-effective solution. S3 Standard-IA is cheaper than Standard and suitable for infrequently accessed data. Intelligent-Tiering (B) has monitoring costs. Option A (initial in IA) would incur retrieval costs for new files. (AWS Docs: S3 Lifecycle Transitions to IA).
</details>

---

**Q154.** A company needs to save the results from a medical trial to an Amazon S3 repository. The repository must allow a few scientists to add new files and must restrict all other users to read-only access. No users can have the ability to modify or delete any files in the repository. The company must keep every file in the repository for a minimum of 1 year after its creation date. Which solution will meet these requirements?

- A. Use S3 Object Lock in governance mode with a legal hold of 1 year.
- B. Use S3 Object Lock in compliance mode with a retention period of 365 days.
- C. Use an IAM role to restrict all users from deleting or changing objects in the S3 bucket. Use an S3 bucket policy to only allow the IAM role.
- D. Configure the S3 bucket to invoke an AWS Lambda function every time an object is added. Configure the function to track the hash of the saved object so that modified objects can be marked accordingly.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Object Lock in compliance mode prevents any user (including root) from deleting or overwriting objects for the specified retention period (365 days). This meets the "no modification/deletion" requirement. Governance mode (A) allows privileged users to modify. IAM policies (C) can be bypassed by root. Lambda (D) is reactive, not preventive (AWS Docs: S3 Object Lock Compliance Mode).
</details>

---

**Q155.** A large media company hosts a web application on AWS. The company wants to start caching confidential media files so that users around the world will have reliable access to the files. The content is stored in Amazon S3 buckets. The company must deliver the content quickly, regardless of where the requests originate geographically. Which solution will meet these requirements?

- A. Use AWS DataSync to connect the S3 buckets to the web application.
- B. Deploy AWS Global Accelerator to connect the S3 buckets to the web application.
- C. Deploy Amazon CloudFront to connect the S3 buckets to CloudFront edge servers.
- D. Use Amazon Simple Queue Service (Amazon SQS) to connect the S3 buckets to the web application.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon CloudFront caches content at edge locations globally, providing fast, reliable access. It can be configured to use S3 as the origin. DataSync (A) is for data transfer; Global Accelerator (B) optimizes network path but doesn't cache; SQS (D) is a queue. (AWS Docs: CloudFront with S3 Origin).
</details>

---

**Q156.** A company produces batch data that comes from different databases. The company also produces live stream data from network sensors and application APIs. The company needs to consolidate all the data into one place for business analytics. The company needs to process the incoming data and then stage the data in different Amazon S3 buckets. Teams will later run one-time queries and import the data into a business intelligence tool to show key performance indicators (KPIs). Which combination of steps will meet these requirements with the LEAST operational overhead? (Choose two.)

- A. Use Amazon Athena for one-time queries. Use Amazon QuickSight to create dashboards for KPIs.
- B. Use Amazon Kinesis Data Analytics for one-time queries. Use Amazon QuickSight to create dashboards for KPIs.
- C. Create custom AWS Lambda functions to move the individual records from the databases to an Amazon Redshift cluster.
- D. Use an AWS Glue extract, transform, and load (ETL) job to convert the data into JSON format. Load the data into multiple Amazon OpenSearch Service (Amazon Elasticsearch Service) clusters.
- E. Use blueprints in AWS Lake Formation to identify the data that can be ingested into a data lake. Use AWS Glue to crawl the source, extract the data, and load the data into Amazon S3 in Apache Parquet format.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* A modern data lake approach: Lake Formation (E) to ingest batch and streaming data into S3 in Parquet format (efficient for analytics). Athena (A) for serverless one-time queries. QuickSight (A) for BI dashboards. This minimizes overhead. Option B (KDA) is for real-time analytics; Option C (custom Lambda) is overhead; Option D (OpenSearch) is for search/analytics, not primary data lake. (AWS Docs: Lake Formation, Athena, QuickSight).
</details>

---

**Q157.** A company stores data in an Amazon Aurora PostgreSQL DB cluster. The company must store all the data for 5 years and must delete all the data after 5 years. The company also must indefinitely keep audit logs of actions that are performed within the database. Currently, the company has automated backups configured for Aurora. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- A. Take a manual snapshot of the DB cluster.
- B. Create a lifecycle policy for the automated backups.
- C. Configure automated backup retention for 5 years.
- D. Configure an Amazon CloudWatch Logs export for the DB cluster.
- E. Use AWS Backup to take the backups and to keep the backups for 5 years.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* For data retention of 5 years, manual snapshots (A) are retained even after cluster deletion. For audit logs, export database logs to CloudWatch Logs (D) for indefinite retention. Automated backups (C) are tied to the cluster lifecycle and deleted when cluster is deleted. AWS Backup (E) could be used, but manual snapshots are simpler. (AWS Docs: Aurora Manual Snapshots, Aurora Log Exports).
</details>

---

**Q158.** A solutions architect is optimizing a website for an upcoming musical event. Videos of the performances will be streamed in real time and then will be available on demand. The event is expected to attract a global online audience. Which service will improve the performance of both the real-time and on-demand streaming?

- A. Amazon CloudFront
- B. AWS Global Accelerator
- C. Amazon Route 53
- D. Amazon S3 Transfer Acceleration

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Amazon CloudFront is a CDN that can cache on-demand content at edge locations and optimize real-time streaming paths. It supports both use cases. Global Accelerator (B) optimizes network path but doesn't cache; Route 53 (C) is DNS; S3 Transfer Acceleration (D) speeds up uploads, not downloads/streaming (AWS Docs: CloudFront for Video Streaming).
</details>

---

**Q159.** A company is running a publicly accessible serverless application that uses Amazon API Gateway and AWS Lambda. The application's traffic recently spiked due to fraudulent requests from botnets. Which steps should a solutions architect take to block requests from unauthorized users? (Choose two.)

- A. Create a usage plan with an API key that is shared with genuine users only.
- B. Integrate logic within the Lambda function to ignore the requests from fraudulent IP addresses.
- C. Implement an AWS WAF rule to target malicious requests and trigger actions to filter them out.
- D. Convert the existing public API to a private API. Update the DNS records to redirect users to the new API endpoint.
- E. Create an IAM role for each user attempting to access the API. A user will assume the role when making the API call.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* API keys (A) with usage plans can restrict access to genuine users. AWS WAF (C) can create rate-based rules and IP blacklists to block botnet traffic. Option B (Lambda logic) is inefficient and not scalable. Option D (private API) breaks public access. Option E (IAM roles) is complex for public users. (AWS Docs: API Gateway Usage Plans, AWS WAF).
</details>

---

**Q160.** An ecommerce company hosts its analytics application in the AWS Cloud. The application generates about 300 MB of data each month. The data is stored in JSON format. The company is evaluating a disaster recovery solution to back up the data. The data must be accessible in milliseconds if it is needed, and the data must be kept for 30 days. Which solution meets these requirements MOST cost-effectively?

- A. Amazon OpenSearch Service (Amazon Elasticsearch Service)
- B. Amazon S3 Glacier
- C. Amazon S3 Standard
- D. Amazon RDS for PostgreSQL

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* S3 Standard provides millisecond access, is cost-effective for 300 MB/month over 30 days (9 GB total), and requires no infrastructure. Glacier (B) has retrieval delays. OpenSearch (A) and RDS (D) are overkill and more expensive for simple backup storage. (AWS Docs: S3 Storage Classes).
</details>

---

**Q161.** A company has a small Python application that processes JSON documents and outputs the results to an on-premises SQL database. The application runs thousands of times each day. The company wants to move the application to the AWS Cloud. The company needs a highly available solution that maximizes scalability and minimizes operational overhead. Which solution will meet these requirements?

- A. Place the JSON documents in an Amazon S3 bucket. Run the Python code on multiple Amazon EC2 instances to process the documents. Store the results in an Amazon Aurora DB cluster.
- B. Place the JSON documents in an Amazon S3 bucket. Create an AWS Lambda function that runs the Python code to process the documents as they arrive in the S3 bucket. Store the results in an Amazon Aurora DB cluster.
- C. Place the JSON documents in an Amazon Elastic Block Store (Amazon EBS) volume. Use the EBS Multi-Attach feature to attach the volume to multiple Amazon EC2 instances. Run the Python code on the EC2 instances to process the documents. Store the results on an Amazon RDS DB instance.
- D. Place the JSON documents in an Amazon Simple Queue Service (Amazon SQS) queue as messages. Deploy the Python code as a container on an Amazon Elastic Container Service (Amazon ECS) cluster that is configured with the Amazon EC2 launch type. Use the container to process the SQS messages. Store the results on an Amazon RDS DB instance.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A serverless architecture (S3 event -> Lambda) maximizes scalability and minimizes operational overhead for a small Python app processing JSON. Aurora is a good database choice. Option A (EC2) and D (ECS on EC2) have more overhead. Option C (EBS Multi-Attach) is complex and not highly available. (AWS Docs: Lambda with S3).
</details>

---

**Q162.** A company wants to use high performance computing (HPC) infrastructure on AWS for financial risk modeling. The company's HPC workloads run on Linux. Each HPC workflow runs on hundreds of Amazon EC2 Spot Instances, is short-lived, and generates thousands of output files that are ultimately stored in persistent storage for analytics and long-term future use. The company seeks a cloud storage solution that permits the copying of on-premises data to long-term persistent storage to make data available for processing by all EC2 instances. The solution should also be a high performance file system that is integrated with persistent storage to read and write datasets and output files. Which combination of AWS services meets these requirements?

- A. Amazon FSx for Lustre integrated with Amazon S3
- B. Amazon FSx for Windows File Server integrated with Amazon S3
- C. Amazon S3 Glacier integrated with Amazon Elastic Block Store (Amazon EBS)
- D. Amazon S3 bucket with a VPC endpoint integrated with an Amazon Elastic Block Store (Amazon EBS) General Purpose SSD (gp2) volume

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* FSx for Lustre is a high-performance file system designed for HPC. It can be integrated with S3, allowing you to copy data from S3 for processing and write results back to S3 for long-term storage. Option B (Windows) is not Linux-based; Option C (Glacier) is not high performance; Option D (EBS) cannot be shared across hundreds of instances easily (AWS Docs: FSx for Lustre with S3).
</details>

---

**Q163.** A company is building a containerized application on premises and decides to move the application to AWS. The application will have thousands of users soon after it is deployed. The company is unsure how to manage the deployment of containers at scale. The company needs to deploy the containerized application in a highly available architecture that minimizes operational overhead. Which solution will meet these requirements?

- A. Store container images in an Amazon Elastic Container Registry (Amazon ECR) repository. Use an Amazon Elastic Container Service (Amazon ECS) cluster with the AWS Fargate launch type to run the containers. Use target tracking to scale automatically based on demand.
- B. Store container images in an Amazon Elastic Container Registry (Amazon ECR) repository. Use an Amazon Elastic Container Service (Amazon ECS) cluster with the Amazon EC2 launch type to run the containers. Use target tracking to scale automatically based on demand.
- C. Store container images in a repository that runs on an Amazon EC2 instance. Run the containers on EC2 instances that are spread across multiple Availability Zones. Monitor the average CPU utilization in Amazon CloudWatch. Launch new EC2 instances as needed.
- D. Create an Amazon EC2 Amazon Machine Image (AMI) that contains the container image. Launch EC2 instances in an Auto Scaling group across multiple Availability Zones. Use an Amazon CloudWatch alarm to scale out EC2 instances when the average CPU utilization threshold is breached.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Fargate is a serverless compute engine for containers, removing the need to manage EC2 instances. This minimizes operational overhead. ECR stores the images. Target tracking scaling provides automatic scaling. Option B (EC2 launch type) and Options C/D all require managing EC2 instances (AWS Docs: ECS Fargate).
</details>

---

**Q164.** A company has two applications: a sender application that sends messages with payloads to be processed and a processing application intended to receive the messages with payloads. The company wants to implement an AWS service to handle messages between the two applications. The sender application can send about 1,000 messages each hour. The messages may take up to 2 days to be processed. If the messages fail to process, they must be retained so that they do not impact the processing of any remaining messages. Which solution meets these requirements and is the MOST operationally efficient?

- A. Set up an Amazon EC2 instance running a Redis database. Configure both applications to use the instance. Store, process, and delete the messages, respectively.
- B. Use an Amazon Kinesis data stream to receive the messages from the sender application. Integrate the processing application with the Kinesis Client Library (KCL).
- C. Integrate the sender and processor applications with an Amazon Simple Queue Service (Amazon SQS) queue. Configure a dead-letter queue to collect the messages that failed to process.
- D. Subscribe the processing application to an Amazon Simple Notification Service (Amazon SNS) topic to receive notifications to process. Integrate the sender application to write to the SNS topic.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* SQS provides message durability, allows up to 14 days retention (meets 2 days), and supports dead-letter queues for failed messages without impacting others. This is operationally efficient. Option A (EC2 Redis) has overhead; Option B (Kinesis) is overkill for 1k messages/hour and has higher operational complexity; Option D (SNS) does not provide message persistence/retry like SQS (AWS Docs: SQS with DLQ).
</details>

---

**Q165.** A solutions architect must design a solution that uses Amazon CloudFront with an Amazon S3 origin to store a static website. The company's security policy requires that all website traffic be inspected by AWS WAF. How should the solutions architect comply with these requirements?

- A. Configure an S3 bucket policy to accept requests coming from the AWS WAF Amazon Resource Name (ARN) only.
- B. Configure Amazon CloudFront to forward all incoming requests to AWS WAF before requesting content from the S3 origin.
- C. Configure a security group that allows Amazon CloudFront IP addresses to access Amazon S3 only. Associate AWS WAF to CloudFront.
- D. Configure Amazon CloudFront and Amazon S3 to use an origin access identity (OAI) to restrict access to the S3 bucket. Enable AWS WAF on the distribution.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* You can associate an AWS WAF web ACL directly with a CloudFront distribution. Using OAI ensures S3 content is only accessible via CloudFront. WAF inspects traffic at the CloudFront edge. Option B (forward to WAF) is not correct; you associate WAF with CloudFront. Option A (S3 policy with WAF ARN) is not valid. (AWS Docs: WAF with CloudFront).
</details>

---

**Q166.** Organizers for a global event want to put daily reports online as static HTML pages. The pages are expected to generate millions of views from users around the world. The files are stored in an Amazon S3 bucket. A solutions architect has been asked to design an efficient and effective solution. Which action should the solutions architect take to accomplish this?

- A. Generate presigned URLs for the files.
- B. Use cross-Region replication to all Regions.
- C. Use the geoproximity feature of Amazon Route 53.
- D. Use Amazon CloudFront with the S3 bucket as its origin.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* CloudFront caches static HTML pages at edge locations globally, reducing latency and load on the S3 origin for millions of views. Presigned URLs (A) are for access control, not performance. Cross-Region replication (B) is expensive and doesn't cache. Geoproximity (C) is for routing, not caching. (AWS Docs: CloudFront for Static Websites).
</details>

---

**Q167.** A company runs a production application on a fleet of Amazon EC2 instances. The application reads the data from an Amazon SQS queue and processes the messages in parallel. The message volume is unpredictable and often has intermittent traffic. This application should continually process messages without any downtime. Which solution meets these requirements MOST cost-effectively?

- A. Use Spot Instances exclusively to handle the maximum capacity required.
- B. Use Reserved Instances exclusively to handle the maximum capacity required.
- C. Use Reserved Instances for the baseline capacity and use Spot Instances to handle additional capacity.
- D. Use Reserved Instances for the baseline capacity and use On-Demand Instances to handle additional capacity.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* For an application that must not have downtime, Spot Instances (which can be interrupted) are not suitable for production critical workloads. Reserved Instances cover the baseline capacity with a discount. On-Demand instances handle unpredictable spikes, providing availability without over-provisioning Reserved Instances. (AWS Docs: EC2 Pricing for Production Workloads).
</details>

---

**Q168.** A security team wants to limit access to specific services or actions in all of the team's AWS accounts. All accounts belong to a large organization in AWS Organizations. The solution must be scalable and there must be a single point where permissions can be maintained. What should a solutions architect do to accomplish this?

- A. Create an ACL to provide access to the services or actions.
- B. Create a security group to allow accounts and attach it to user groups.
- C. Create cross-account roles in each account to deny access to the services or actions.
- D. Create a service control policy in the root organizational unit to deny access to the services or actions.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Service Control Policies (SCPs) applied at the root OU (or specific OUs) in AWS Organizations allow centralized control to deny (or allow) specific actions across all member accounts. This is scalable and provides a single management point. Options A, B, C are not suitable for organization-wide, centralized denial of actions (AWS Docs: SCPs).
</details>

---

**Q169.** A company is concerned about the security of its public web application due to recent web attacks. The application uses an Application Load Balancer (ALB). A solutions architect must reduce the risk of DDoS attacks against the application. What should the solutions architect do to meet this requirement?

- A. Add an Amazon Inspector agent to the ALB.
- B. Configure Amazon Macie to prevent attacks.
- C. Enable AWS Shield Advanced to prevent attacks.
- D. Configure Amazon GuardDuty to monitor the ALB.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Shield Advanced provides enhanced DDoS protection for applications running on ALBs, CloudFront, etc. Inspector (A) is for vulnerability scanning; Macie (B) is for data security; GuardDuty (D) is for threat detection, not DDoS mitigation. (AWS Docs: AWS Shield Advanced).
</details>

---

**Q170.** A company's web application is running on Amazon EC2 instances behind an Application Load Balancer. The company recently changed its policy, which now requires the application to be accessed from one specific country only. Which configuration will meet this requirement?

- A. Configure the security group for the EC2 instances.
- B. Configure the security group on the Application Load Balancer.
- C. Configure AWS WAF on the Application Load Balancer in a VPC.
- D. Configure the network ACL for the subnet that contains the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS WAF can be associated with an ALB and can use geo-match conditions to allow or block traffic from specific countries. Security groups (A/B) and NACLs (D) operate at IP/CIDR level, not country level (without maintaining IP lists). (AWS Docs: WAF Geo Match).
</details>

---

**Q171.** A company provides an API to its users that automates inquiries for tax computations based on item prices. The company experiences a larger number of inquiries during the holiday season only that cause slower response times. A solutions architect needs to design a solution that is scalable and elastic. What should the solutions architect do to accomplish this?

- A. Provide an API hosted on an Amazon EC2 instance. The EC2 instance performs the required computations when the API request is made.
- B. Design a REST API using Amazon API Gateway that accepts the item names. API Gateway passes item names to AWS Lambda for tax computations.
- C. Create an Application Load Balancer that has two Amazon EC2 instances behind it. The EC2 instances will compute the tax on the received item names.
- D. Design a REST API using Amazon API Gateway that connects with an API hosted on an Amazon EC2 instance. API Gateway accepts and passes the item names to the EC2 instance for tax computations.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* API Gateway + Lambda provides a serverless, scalable, and elastic solution that automatically scales with demand (holiday spikes) with no infrastructure management. Options A, C, D require managing EC2 instances and scaling them. (AWS Docs: API Gateway + Lambda).
</details>

---

**Q172.** A solutions architect is creating a new Amazon CloudFront distribution for an application. Some of the information submitted by users is sensitive. The application uses HTTPS but needs another layer of security. The sensitive information should be protected throughout the entire application stack, and access to the information should be restricted to certain applications. Which action should the solutions architect take?

- A. Configure a CloudFront signed URL.
- B. Configure a CloudFront signed cookie.
- C. Configure a CloudFront field-level encryption profile.
- D. Configure CloudFront and set the Origin Protocol Policy setting to HTTPS Only for the Viewer Protocol Policy.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Field-level encryption allows you to encrypt specific data fields (e.g., credit card numbers) at the edge, so that only certain applications (with the private key) can decrypt them. Signed URLs/cookies (A, B) control access to content, not field-level protection. Option D is basic HTTPS (already used). (AWS Docs: CloudFront Field-Level Encryption).
</details>

---

**Q173.** A gaming company hosts a browser-based application on AWS. The users of the application consume a large number of videos and images that are stored in Amazon S3. This content is the same for all users. The application has increased in popularity, and millions of users worldwide accessing these media files. The company wants to provide the files to the users while reducing the load on the origin. Which solution meets these requirements MOST cost-effectively?

- A. Deploy an AWS Global Accelerator accelerator in front of the web servers.
- B. Deploy an Amazon CloudFront web distribution in front of the S3 bucket.
- C. Deploy an Amazon ElastiCache for Redis instance in front of the web servers.
- D. Deploy an Amazon ElastiCache for Memcached instance in front of the web servers.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* CloudFront caches static content (videos/images) at edge locations, reducing load on the S3 origin and providing low latency globally. Global Accelerator (A) doesn't cache. ElastiCache (C, D) is for caching database queries, not static files, and would require custom code. (AWS Docs: CloudFront for Static Content).
</details>

---

**Q174.** A company has a multi-tier application that runs six front-end web servers in an Amazon EC2 Auto Scaling group in a single Availability Zone behind an Application Load Balancer (ALB). A solutions architect needs to modify the infrastructure to be highly available without modifying the application. Which architecture should the solutions architect choose that provides high availability?

- A. Create an Auto Scaling group that uses three instances across each of two Regions.
- B. Modify the Auto Scaling group to use three instances across each of two Availability Zones.
- C. Create an Auto Scaling template that can be used to quickly create more instances in another Region.
- D. Change the ALB in front of the Amazon EC2 instances in a round-robin configuration to balance traffic to the web tier.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To achieve high availability without modifying the application, distribute the Auto Scaling group instances across multiple Availability Zones within the same Region. Multi-Region (A) would require application changes. Option C doesn't provide HA automatically; Option D doesn't address single AZ failure. (AWS Docs: Auto Scaling Group Multi-AZ).
</details>

---

**Q175.** An ecommerce company has an order-processing application that uses Amazon API Gateway and an AWS Lambda function. The application stores data in an Amazon Aurora PostgreSQL database. During a recent sales event, a sudden surge in customer orders occurred. Some customers experienced timeouts, and the application did not process the orders of those customers. A solutions architect determined that the CPU utilization and memory utilization were high on the database because of a large number of open connections. The solutions architect needs to prevent the timeout errors while making the least possible changes to the application. Which solution will meet these requirements?

- A. Configure provisioned concurrency for the Lambda function. Modify the database to be a global database in multiple AWS Regions.
- B. Use Amazon RDS Proxy to create a proxy for the database. Modify the Lambda function to use the RDS Proxy endpoint instead of the database endpoint.
- C. Create a read replica for the database in a different AWS Region. Use query string parameters in API Gateway to route traffic to the read replica.
- D. Migrate the data from Aurora PostgreSQL to Amazon DynamoDB by using AWS Database Migration Service (AWS DMS). Modify the Lambda function to use the DynamoDB table.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* RDS Proxy manages connection pooling, reducing the number of open connections to the database, preventing timeouts during surges. The change is minimal (just update the endpoint). Provisioned concurrency (A) helps Lambda cold starts, not DB connections. Read replica (C) doesn't reduce connections on the writer. Migrating to DynamoDB (D) is a major change. (AWS Docs: RDS Proxy).
</details>

---

**Q176.** An application runs on Amazon EC2 instances in private subnets. The application needs to access an Amazon DynamoDB table. What is the MOST secure way to access the table while ensuring that the traffic does not leave the AWS network?

- A. Use a VPC endpoint for DynamoDB.
- B. Use a NAT gateway in a public subnet.
- C. Use a NAT instance in a private subnet.
- D. Use the internet gateway attached to the VPC.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* A VPC endpoint (Gateway endpoint for DynamoDB) allows private, secure access to DynamoDB without traffic leaving the AWS network or traversing the internet. NAT gateway/instance (B, C) routes traffic to the internet. IGW (D) is for internet access. (AWS Docs: DynamoDB VPC Endpoint).
</details>

---

**Q177.** An entertainment company is using Amazon DynamoDB to store media metadata. The application is read intensive and experiencing delays. The company does not have staff to handle additional operational overhead and needs to improve the performance efficiency of DynamoDB without reconfiguring the application. What should a solutions architect recommend to meet this requirement?

- A. Use Amazon ElastiCache for Redis.
- B. Use Amazon DynamoDB Accelerator (DAX).
- C. Replicate data by using DynamoDB global tables.
- D. Use Amazon ElastiCache for Memcached with Auto Discovery enabled.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* DAX is a fully managed, in-memory cache for DynamoDB that requires no application reconfiguration (just change the endpoint). It reduces read latency significantly. ElastiCache (A, D) requires application changes. Global tables (C) are for multi-region replication, not read latency. (AWS Docs: DynamoDB DAX).
</details>

---

**Q178.** A company's infrastructure consists of Amazon EC2 instances and an Amazon RDS DB instance in a single AWS Region. The company wants to back up its data in a separate Region. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Backup to copy EC2 backups and RDS backups to the separate Region.
- B. Use Amazon Data Lifecycle Manager (Amazon DLM) to copy EC2 backups and RDS backups to the separate Region.
- C. Create Amazon Machine Images (AMIs) of the EC2 instances. Copy the AMIs to the separate Region. Create a read replica for the RDS DB instance in the separate Region.
- D. Create Amazon Elastic Block Store (Amazon EBS) snapshots. Copy the EBS snapshots to the separate Region. Create RDS snapshots. Export the RDS snapshots to Amazon S3. Configure S3 Cross-Region Replication (CRR) to the separate Region.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Backup supports cross-Region backup copying for both EC2 (via AMIs or snapshots) and RDS with minimal configuration. DLM (B) only handles EBS snapshots, not RDS. Option C and D require more manual steps and operational overhead. (AWS Docs: AWS Backup Cross-Region).
</details>

---

**Q179.** A solutions architect needs to securely store a database user name and password that an application uses to access an Amazon RDS DB instance. The application that accesses the database runs on an Amazon EC2 instance. The solutions architect wants to create a secure parameter in AWS Systems Manager Parameter Store. What should the solutions architect do to meet this requirement?

- A. Create an IAM role that has read access to the Parameter Store parameter. Allow Decrypt access to an AWS Key Management Service (AWS KMS) key that is used to encrypt the parameter. Assign this IAM role to the EC2 instance.
- B. Create an IAM policy that allows read access to the Parameter Store parameter. Allow Decrypt access to an AWS Key Management Service (AWS KMS) key that is used to encrypt the parameter. Assign this IAM policy to the EC2 instance.
- C. Create an IAM trust relationship between the Parameter Store parameter and the EC2 instance. Specify Amazon RDS as a principal in the trust policy.
- D. Create an IAM trust relationship between the DB instance and the EC2 instance. Specify Systems Manager as a principal in the trust policy.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* You create an IAM role with permissions to read the Parameter Store parameter and decrypt the KMS key. Assign this role to the EC2 instance (via instance profile). You cannot attach a policy directly to an instance (B). Trust relationships (C, D) are for roles, not parameters. (AWS Docs: Parameter Store with IAM Role).
</details>

---

**Q180.** A company is designing a cloud communications platform that is driven by APIs. The application is hosted on Amazon EC2 instances behind a Network Load Balancer (NLB). The company uses Amazon API Gateway to provide external users with access to the application through APIs. The company wants to protect the platform against web exploits like SQL injection and also wants to detect and mitigate large, sophisticated DDoS attacks. Which combination of solutions provides the MOST protection? (Choose two.)

- A. Use AWS WAF to protect the NLB.
- B. Use AWS Shield Advanced with the NLB.
- C. Use AWS WAF to protect Amazon API Gateway.
- D. Use Amazon GuardDuty with AWS Shield Standard.
- E. Use AWS Shield Standard with Amazon API Gateway.

<details>
<summary>Show Answer</summary>

**Answer: B, C**

*Explanation:* AWS WAF can protect API Gateway (C) from web exploits (SQL injection, XSS). AWS Shield Advanced (B) provides enhanced DDoS protection for the NLB (and API Gateway). Shield Standard (E) is free but less effective. WAF cannot be directly associated with an NLB (A) (only with ALB, API Gateway, CloudFront). GuardDuty (D) is for threat detection, not mitigation. (AWS Docs: WAF with API Gateway, Shield Advanced).
</details>

---

**Q181.** A company has a legacy data processing application that runs on Amazon EC2 instances. Data is processed sequentially, but the order of results does not matter. The application uses a monolithic architecture. The only way that the company can scale the application to meet increased demand is to increase the size of the instances. The company's developers have decided to rewrite the application to use a microservices architecture on Amazon Elastic Container Service (Amazon ECS). What should a solutions architect recommend for communication between the microservices?

- A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Add code to the data producers, and send data to the queue. Add code to the data consumers to process data from the queue.
- B. Create an Amazon Simple Notification Service (Amazon SNS) topic. Add code to the data producers, and publish notifications to the topic. Add code to the data consumers to subscribe to the topic.
- C. Create an AWS Lambda function to pass messages. Add code to the data producers to call the Lambda function with a data object. Add code to the data consumers to receive a data object that is passed from the Lambda function.
- D. Create an Amazon DynamoDB table. Enable DynamoDB Streams. Add code to the data producers to insert data into the table. Add code to the data consumers to use the DynamoDB Streams API to detect new table entries and retrieve the data.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* SQS provides decoupled, asynchronous communication between microservices. Order doesn't matter, so a standard queue is fine. SNS (B) is for pub/sub, not point-to-point task distribution. Lambda (C) introduces potential scaling limits. DynamoDB Streams (D) is more complex for simple task queuing. (AWS Docs: SQS for Decoupling Microservices).
</details>

---

**Q182.** A company wants to migrate its MySQL database from on premises to AWS. The company recently experienced a database outage that significantly impacted the business. To ensure this does not happen again, the company wants a reliable database solution on AWS that minimizes data loss and stores every transaction on at least two nodes. Which solution meets these requirements?

- A. Create an Amazon RDS DB instance with synchronous replication to three nodes in three Availability Zones.
- B. Create an Amazon RDS MySQL DB instance with Multi-AZ functionality enabled to synchronously replicate the data.
- C. Create an Amazon RDS MySQL DB instance and then create a read replica in a separate AWS Region that synchronously replicates the data.
- D. Create an Amazon EC2 instance with a MySQL engine installed that triggers an AWS Lambda function to synchronously replicate the data to an Amazon RDS MySQL DB instance.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* RDS Multi-AZ deployments provide synchronous replication to a standby instance in a different Availability Zone, minimizing data loss and providing automatic failover. This meets "at least two nodes" and reliability. Option A (3 nodes) is not standard RDS; Option C (cross-Region read replica) is asynchronous; Option D (custom) has high overhead. (AWS Docs: RDS Multi-AZ).
</details>

---

**Q183.** A company is building a new dynamic ordering website. The company wants to minimize server maintenance and patching. The website must be highly available and must scale read and write capacity as quickly as possible to meet changes in user demand. Which solution will meet these requirements?

- A. Host static content in Amazon S3. Host dynamic content by using Amazon API Gateway and AWS Lambda. Use Amazon DynamoDB with on-demand capacity for the database. Configure Amazon CloudFront to deliver the website content.
- B. Host static content in Amazon S3. Host dynamic content by using Amazon API Gateway and AWS Lambda. Use Amazon Aurora with Aurora Auto Scaling for the database. Configure Amazon CloudFront to deliver the website content.
- C. Host all the website content on Amazon EC2 instances. Create an Auto Scaling group to scale the EC2 instances. Use an Application Load Balancer to distribute traffic. Use Amazon DynamoDB with provisioned write capacity for the database.
- D. Host all the website content on Amazon EC2 instances. Create an Auto Scaling group to scale the EC2 instances. Use an Application Load Balancer to distribute traffic. Use Amazon Aurora with Aurora Auto Scaling for the database.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Serverless architecture (S3, CloudFront, API Gateway, Lambda, DynamoDB on-demand) minimizes server maintenance, scales instantly, and handles read/write spikes. Aurora (B) is serverless? Aurora Serverless exists, but DynamoDB on-demand is simpler for scaling. Options C/D require EC2 maintenance. (AWS Docs: Serverless Web App).
</details>

---

**Q184.** A company has an AWS account used for software engineering. The AWS account has access to the company's on-premises data center through a pair of AWS Direct Connect connections. All non-VPC traffic routes to the virtual private gateway. A development team recently created an AWS Lambda function through the console. The development team needs to allow the function to access a database that runs in a private subnet in the company's data center. Which solution will meet these requirements?

- A. Configure the Lambda function to run in the VPC with the appropriate security group.
- B. Set up a VPN connection from AWS to the data center. Route the traffic from the Lambda function through the VPN.
- C. Update the route tables in the VPC to allow the Lambda function to access the on-premises data center through Direct Connect.
- D. Create an Elastic IP address. Configure the Lambda function to send traffic through the Elastic IP address without an elastic network interface.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* To access resources in a private subnet (on-premises via Direct Connect), the Lambda function must be configured to run within the VPC. This attaches an elastic network interface (ENI) to the function, allowing it to route traffic via the VPC's route tables (which include the Direct Connect route). Option B (VPN) is redundant; Option C (route tables) is needed but the primary action is configuring Lambda for VPC; Option D is incorrect. (AWS Docs: Lambda VPC).
</details>

---

**Q185.** A company runs an application using Amazon ECS. The application creates resized versions of an original image and then makes Amazon S3 API calls to store the resized images in Amazon S3. How can a solutions architect ensure that the application has permission to access Amazon S3?

- A. Update the S3 role in AWS IAM to allow read/write access from Amazon ECS, and then relaunch the container.
- B. Create an IAM role with S3 permissions, and then specify that role as the taskRoleArn in the task definition.
- C. Create a security group that allows access from Amazon ECS to Amazon S3, and update the launch configuration used by the ECS cluster.
- D. Create an IAM user with S3 permissions, and then relaunch the Amazon EC2 instances for the ECS cluster while logged in as this account.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* In ECS, you specify an IAM role (`taskRoleArn`) in the task definition. The containers then inherit this role's permissions (e.g., to call S3 APIs). Option A (S3 role) doesn't exist; Security groups (C) are for network access, not API permissions; IAM user on EC2 instances (D) is less secure and not the ECS pattern. (AWS Docs: ECS Task IAM Role).
</details>

---

**Q186.** A company has a Windows-based application that must be migrated to AWS. The application requires the use of a shared Windows file system attached to multiple Amazon EC2 Windows instances that are deployed across multiple Availability Zones. What should a solutions architect do to meet this requirement?

- A. Configure AWS Storage Gateway in volume gateway mode. Mount the volume to each Windows instance.
- B. Configure Amazon FSx for Windows File Server. Mount the Amazon FSx file system to each Windows instance.
- C. Configure a file system by using Amazon Elastic File System (Amazon EFS). Mount the EFS file system to each Windows instance.
- D. Configure an Amazon Elastic Block Store (Amazon EBS) volume with the required size. Attach each EC2 instance to the volume. Mount the file system within the volume to each Windows instance.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon FSx for Windows File Server provides a fully managed, highly available Windows file system (SMB) that can be mounted by multiple EC2 Windows instances across AZs. EFS (C) is NFS, not Windows-native. EBS (D) cannot be attached to multiple instances (unless using Multi-Attach with specific constraints). Storage Gateway (A) is a hybrid solution. (AWS Docs: FSx for Windows File Server).
</details>

---

**Q187.** A company is developing an ecommerce application that will consist of a load-balanced front end, a container-based application, and a relational database. A solutions architect needs to create a highly available solution that operates with as little manual intervention as possible. Which solutions meet these requirements? (Choose two.)

- A. Create an Amazon RDS DB instance in Multi-AZ mode.
- B. Create an Amazon RDS DB instance and one or more replicas in another Availability Zone.
- C. Create an Amazon EC2 instance-based Docker cluster to handle the dynamic application load.
- D. Create an Amazon Elastic Container Service (Amazon ECS) cluster with a Fargate launch type to handle the dynamic application load.
- E. Create an Amazon Elastic Container Service (Amazon ECS) cluster with an Amazon EC2 launch type to handle the dynamic application load.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* RDS Multi-AZ (A) provides high availability for the relational database with automatic failover (minimal intervention). ECS with Fargate (D) is serverless, minimizing operational overhead for the container-based application. Option B (read replicas) are for scaling reads, not HA failover. Option C (EC2-based Docker) and E (ECS with EC2) require managing EC2 instances. (AWS Docs: RDS Multi-AZ, Fargate).
</details>

---

**Q188.** A company uses Amazon S3 as its data lake. The company has a new partner that must use SFTP to upload data files. A solutions architect needs to implement a highly available SFTP solution that minimizes operational overhead. Which solution will meet these requirements?

- A. Use AWS Transfer Family to configure an SFTP-enabled server with a publicly accessible endpoint. Choose the S3 data lake as the destination.
- B. Use Amazon S3 File Gateway as an SFTP server. Expose the S3 File Gateway endpoint URL to the new partner. Share the S3 File Gateway endpoint with the new partner.
- C. Launch an Amazon EC2 instance in a private subnet in a VPC. Instruct the new partner to upload files to the EC2 instance by using a VPN. Run a cron job script on the EC2 instance to upload files to the S3 data lake.
- D. Launch Amazon EC2 instances in a private subnet in a VPC. Place a Network Load Balancer (NLB) in front of the EC2 instances. Create an SFTP listener port for the NLB. Share the NLB hostname with the new partner. Run a cron job script on the EC2 instances to upload files to the S3 data lake.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Transfer Family provides a fully managed, highly available SFTP service that can write directly to an S3 data lake, minimizing operational overhead. Options B (File Gateway doesn't support SFTP), C and D require managing EC2 instances and scripts. (AWS Docs: AWS Transfer Family for SFTP).
</details>

---

**Q189.** A company needs to store contract documents. A contract lasts for 5 years. During the 5-year period, the company must ensure that the documents cannot be overwritten or deleted. The company needs to encrypt the documents at rest and rotate the encryption keys automatically every year. Which combination of steps should a solutions architect take to meet these requirements with the LEAST operational overhead? (Choose two.)

- A. Store the documents in Amazon S3. Use S3 Object Lock in governance mode.
- B. Store the documents in Amazon S3. Use S3 Object Lock in compliance mode.
- C. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure key rotation.
- D. Use server-side encryption with AWS Key Management Service (AWS KMS) customer managed keys. Configure key rotation.
- E. Use server-side encryption with AWS Key Management Service (AWS KMS) customer provided (imported) keys. Configure key rotation.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* S3 Object Lock compliance mode (B) prevents any deletion/overwrite for the retention period. SSE-KMS with customer managed keys (D) allows automatic annual key rotation (via KMS). Governance mode (A) allows privileged users to bypass. SSE-S3 (C) does not support key rotation. Imported keys (E) require manual rotation. (AWS Docs: S3 Object Lock Compliance, SSE-KMS Rotation).
</details>

---

**Q190.** A company has a web application that is based on Java and PHP. The company plans to move the application from on premises to AWS. The company needs the ability to test new site features frequently. The company also needs a highly available and managed solution that requires minimum operational overhead. Which solution will meet these requirements?

- A. Create an Amazon S3 bucket. Enable static web hosting on the S3 bucket. Upload the static content to the S3 bucket. Use AWS Lambda to process all dynamic content.
- B. Deploy the web application to an AWS Elastic Beanstalk environment. Use URL swapping to switch between multiple Elastic Beanstalk environments for feature testing.
- C. Deploy the web application to Amazon EC2 instances that are configured with Java and PHP. Use Auto Scaling groups and an Application Load Balancer to manage the website's availability.
- D. Containerize the web application. Deploy the web application to Amazon EC2 instances. Use the AWS Load Balancer Controller to dynamically route traffic between containers that contain the new site features for testing.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Elastic Beanstalk is a managed service that supports Java and PHP, provides high availability (Multi-AZ), and allows easy feature testing via environment swapping (e.g., blue/green). Option A (S3/Lambda) is for static sites; Option C (EC2) has more overhead; Option D (EC2/containers) also has overhead. (AWS Docs: Elastic Beanstalk).
</details>

---

**Q191.** A company has an ordering application that stores customer information in Amazon RDS for MySQL. During regular business hours, employees run one-time queries for reporting purposes. Timeouts are occurring during order processing because the reporting queries are taking a long time to run. The company needs to eliminate the timeouts without preventing employees from performing queries. What should a solutions architect do to meet these requirements?

- A. Create a read replica. Move reporting queries to the read replica.
- B. Create a read replica. Distribute the ordering application to the primary DB instance and the read replica.
- C. Migrate the ordering application to Amazon DynamoDB with on-demand capacity.
- D. Schedule the reporting queries for non-peak hours.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Creating a read replica offloads the long-running reporting queries from the primary DB instance, eliminating timeouts on order processing. The application continues to use the primary for writes/reads; reporting uses the replica. Option B would incorrectly split application traffic; Option C is a major migration; Option D doesn't solve the issue during business hours. (AWS Docs: RDS Read Replicas).
</details>

---

**Q192.** A hospital wants to create digital copies for its large collection of historical written records. The hospital will continue to add hundreds of new documents each day. The hospital's data team will scan the documents and will upload the documents to the AWS Cloud. A solutions architect must implement a solution to analyze the documents, extract the medical information, and store the documents so that an application can run SQL queries on the data. The solution must maximize scalability and operational efficiency. Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

- A. Write the document information to an Amazon EC2 instance that runs a MySQL database.
- B. Write the document information to an Amazon S3 bucket. Use Amazon Athena to query the data.
- C. Create an Auto Scaling group of Amazon EC2 instances to run a custom application that processes the scanned files and extracts the medical information.
- D. Create an AWS Lambda function that runs when new documents are uploaded. Use Amazon Rekognition to convert the documents to raw text. Use Amazon Transcribe Medical to detect and extract relevant medical information from the text.
- E. Create an AWS Lambda function that runs when new documents are uploaded. Use Amazon Textract to convert the documents to raw text. Use Amazon Comprehend Medical to detect and extract relevant medical information from the text.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* Store scanned documents in S3 (B). Use Lambda triggered by S3 events to process documents. Amazon Textract (E) extracts text from scanned documents. Amazon Comprehend Medical (E) detects medical information. Store extracted data in S3 and use Athena (B) to run SQL queries. This is serverless and scalable. Option D uses Rekognition (image analysis) and Transcribe (audio), not text extraction from documents. (AWS Docs: Textract, Comprehend Medical, Athena).
</details>

---

**Q193.** A company is running a batch application on Amazon EC2 instances. The application consists of a backend with multiple Amazon RDS databases. The application is causing a high number of reads on the databases. A solutions architect must reduce the number of database reads while ensuring high availability. What should the solutions architect do to meet this requirement?

- A. Add Amazon RDS read replicas.
- B. Use Amazon ElastiCache for Redis.
- C. Use Amazon Route 53 DNS caching.
- D. Use Amazon ElastiCache for Memcached.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* ElastiCache for Redis is an in-memory cache that can store frequent query results, reducing reads to the databases. It supports high availability (Multi-AZ). Read replicas (A) offload reads but don't reduce the total number of reads hitting the database (they distribute them). DNS caching (C) is irrelevant. Memcached (D) doesn't have built-in high availability like Redis. (AWS Docs: ElastiCache for Redis).
</details>

---

**Q194.** A company needs to run a critical application on AWS. The company needs to use Amazon EC2 for the application's database. The database must be highly available and must fail over automatically if a disruptive event occurs. Which solution will meet these requirements?

- A. Launch two EC2 instances, each in a different Availability Zone in the same AWS Region. Install the database on both EC2 instances. Configure the EC2 instances as a cluster. Set up database replication.
- B. Launch an EC2 instance in an Availability Zone. Install the database on the EC2 instance. Use an Amazon Machine Image (AMI) to back up the data. Use AWS CloudFormation to automate provisioning of the EC2 instance if a disruptive event occurs.
- C. Launch two EC2 instances, each in a different AWS Region. Install the database on both EC2 instances. Set up database replication. Fail over the database to a second Region.
- D. Launch an EC2 instance in an Availability Zone. Install the database on the EC2 instance. Use an Amazon Machine Image (AMI) to back up the data. Use EC2 automatic recovery to recover the instance if a disruptive event occurs.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* For high availability and automatic failover with EC2-based database, you need to set up a cluster (e.g., using database native replication) across multiple AZs. Option D (automatic recovery) only recovers within the same AZ and doesn't provide HA (it has downtime). Option B/C have higher RTO or complexity. Option A is the standard self-managed HA pattern. (AWS Docs: EC2 High Availability).
</details>

---

**Q195.** A company's order system sends requests from clients to Amazon EC2 instances. The EC2 instances process the orders and then store the orders in a database on Amazon RDS. Users report that they must reprocess orders when the system fails. The company wants a resilient solution that can process orders automatically if a system outage occurs. What should a solutions architect do to meet these requirements?

- A. Move the EC2 instances into an Auto Scaling group. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to target an Amazon Elastic Container Service (Amazon ECS) task.
- B. Move the EC2 instances into an Auto Scaling group behind an Application Load Balancer (ALB). Update the order system to send messages to the ALB endpoint.
- C. Move the EC2 instances into an Auto Scaling group. Configure the order system to send messages to an Amazon Simple Queue Service (Amazon SQS) queue. Configure the EC2 instances to consume messages from the queue.
- D. Create an Amazon Simple Notification Service (Amazon SNS) topic. Create an AWS Lambda function, and subscribe the function to the SNS topic. Configure the order system to send messages to the SNS topic. Send a command to the EC2 instances to process the messages by using AWS Systems Manager Run Command.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Using SQS decouples the order system from the processing instances. If an instance fails, the message remains in the queue and another instance will process it. Auto Scaling ensures replacement of failed instances. This provides resilience and automatic reprocessing. Option B (direct ALB) doesn't provide message durability; Option D (SNS to Lambda to Run Command) is overly complex. (AWS Docs: SQS for Resiliency).
</details>

---

**Q196.** A company runs an application on a large fleet of Amazon EC2 instances. The application reads and writes entries into an Amazon DynamoDB table. The size of the DynamoDB table continuously grows, but the application needs only data from the last 30 days. The company needs a solution that minimizes cost and development effort. Which solution meets these requirements?

- A. Use an AWS CloudFormation template to deploy the complete solution. Redeploy the CloudFormation stack every 30 days, and delete the original stack.
- B. Use an EC2 instance that runs a monitoring application from AWS Marketplace. Configure the monitoring application to use Amazon DynamoDB Streams to store the timestamp when a new item is created in the table. Use a script that runs on the EC2 instance to delete items that have a timestamp that is older than 30 days.
- C. Configure Amazon DynamoDB Streams to invoke an AWS Lambda function when a new item is created in the table. Configure the Lambda function to delete items in the table that are older than 30 days.
- D. Extend the application to add an attribute that has a value of the current timestamp plus 30 days to each new item that is created in the table. Configure DynamoDB to use the attribute as the TTL attribute.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* DynamoDB Time to Live (TTL) is the most efficient and cost-effective way to automatically delete items older than 30 days. It requires no custom code (just add a TTL attribute and enable it on the table). Option A (CloudFormation) is not automatic; Options B and C require custom code and development effort. (AWS Docs: DynamoDB TTL).
</details>

---

**Q197.** A company has a Microsoft .NET application that runs on an on-premises Windows Server. The application stores data by using an Oracle Database Standard Edition server. The company is planning a migration to AWS and wants to minimize development changes while moving the application. The AWS application environment should be highly available. Which combination of actions should the company take to meet these requirements? (Choose two.)

- A. Refactor the application as serverless with AWS Lambda functions running .NET Core.
- B. Rehost the application in AWS Elastic Beanstalk with the .NET platform in a Multi-AZ deployment.
- C. Replatform the application to run on Amazon EC2 with the Amazon Linux Amazon Machine Image (AMI).
- D. Use AWS Database Migration Service (AWS DMS) to migrate from the Oracle database to Amazon DynamoDB in a Multi-AZ deployment.
- E. Use AWS Database Migration Service (AWS DMS) to migrate from the Oracle database to Oracle on Amazon RDS in a Multi-AZ deployment.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* Rehosting the .NET app on Elastic Beanstalk (B) minimizes code changes and provides high availability (Multi-AZ). Migrating the Oracle database to RDS for Oracle (E) with Multi-AZ minimizes database changes and provides HA. Refactoring (A) requires code changes. Replatforming to Linux (C) would require changes. Migrating to DynamoDB (D) is a major change. (AWS Docs: AWS DMS for Oracle, Elastic Beanstalk for .NET).
</details>

---

**Q198.** A company runs a containerized application on a Kubernetes cluster in an on-premises data center. The company is using a MongoDB database for data storage. The company wants to migrate some of these environments to AWS, but no code changes or deployment method changes are possible at this time. The company needs a solution that minimizes operational overhead. Which solution meets these requirements?

- A. Use Amazon Elastic Container Service (Amazon ECS) with Amazon EC2 worker nodes for compute and MongoDB on EC2 for data storage.
- B. Use Amazon Elastic Container Service (Amazon ECS) with AWS Fargate for compute and Amazon DynamoDB for data storage.
- C. Use Amazon Elastic Kubernetes Service (Amazon EKS) with Amazon EC2 worker nodes for compute and Amazon DynamoDB for data storage.
- D. Use Amazon Elastic Kubernetes Service (Amazon EKS) with AWS Fargate for compute and Amazon DocumentDB (with MongoDB compatibility) for data storage.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* To avoid code/deployment changes, the company needs a managed Kubernetes service (EKS) and a MongoDB-compatible database (Amazon DocumentDB). EKS with Fargate minimizes operational overhead (no node management). Option A uses ECS (different orchestration), requires code changes. Options B/C use DynamoDB (not MongoDB-compatible). (AWS Docs: Amazon DocumentDB, EKS on Fargate).
</details>

---

**Q199.** A telemarketing company is designing its customer call center functionality on AWS. The company needs a solution that provides multiple speaker recognition and generates transcript files. The company wants to query the transcript files to analyze the business patterns. The transcript files must be stored for 7 years for auditing purposes. Which solution will meet these requirements?

- A. Use Amazon Rekognition for multiple speaker recognition. Store the transcript files in Amazon S3. Use machine learning models for transcript file analysis.
- B. Use Amazon Transcribe for multiple speaker recognition. Use Amazon Athena for transcript file analysis.
- C. Use Amazon Translate for multiple speaker recognition. Store the transcript files in Amazon Redshift. Use SQL queries for transcript file analysis.
- D. Use Amazon Rekognition for multiple speaker recognition. Store the transcript files in Amazon S3. Use Amazon Textract for transcript file analysis.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon Transcribe supports speaker identification (multiple speakers) and generates transcripts. Store transcripts in S3. Use Amazon Athena to run SQL queries on the transcripts for business analysis. Rekognition (A, D) is for image/video, not audio. Translate (C) is for translation, not speaker recognition. (AWS Docs: Amazon Transcribe Speaker Identification, Athena on S3).
</details>

---

**Q200.** A company hosts its application on AWS. The company uses Amazon Cognito to manage users. When users log in to the application, the application fetches required data from Amazon DynamoDB by using a REST API that is hosted in Amazon API Gateway. The company wants an AWS managed solution that will control access to the REST API to reduce development efforts. Which solution will meet these requirements with the LEAST operational overhead?

- A. Configure an AWS Lambda function to be an authorizer in API Gateway to validate which user made the request.
- B. For each user, create and assign an API key that must be sent with each request. Validate the key by using an AWS Lambda function.
- C. Send the user's email address in the header with every request. Invoke an AWS Lambda function to validate that the user with that email address has proper access.
- D. Configure an Amazon Cognito user pool authorizer in API Gateway to allow Amazon Cognito to validate each request.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* API Gateway supports native Cognito user pool authorizers, which integrate directly with Cognito to validate JWTs and control access. This is a managed solution with no custom Lambda code, reducing development effort. Options A, B, C all require custom Lambda authorizers (more overhead). (AWS Docs: API Gateway Cognito Authorizer).
</details>

---

**Q201.** A company is developing a marketing communications service that targets mobile app users. The company needs to send confirmation messages with Short Message Service (SMS) to its users. The users must be able to reply to the SMS messages. The company must store the responses for a year for analysis. What should a solutions architect do to meet these requirements?

- A. Create an Amazon Connect contact flow to send the SMS messages. Use AWS Lambda to process the responses.
- B. Build an Amazon Pinpoint journey. Configure Amazon Pinpoint to send events to an Amazon Kinesis data stream for analysis and archiving.
- C. Use Amazon Simple Queue Service (Amazon SQS) to distribute the SMS messages. Use AWS Lambda to process the responses.
- D. Create an Amazon Simple Notification Service (Amazon SNS) FIFO topic. Subscribe an Amazon Kinesis data stream to the SNS topic for analysis and archiving.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon Pinpoint supports two-way SMS messaging and can send events (including responses) to Kinesis Data Streams for real-time analysis and archiving (to S3). This is a managed solution. Connect (A) is a contact center. SQS (C) and SNS (D) are not designed for two-way SMS campaigns. (AWS Docs: Amazon Pinpoint Two-Way SMS).
</details>

---

**Q202.** A company is planning to move its data to an Amazon S3 bucket. The data must be encrypted when it is stored in the S3 bucket. Additionally, the encryption key must be automatically rotated every year. Which solution will meet these requirements with the LEAST operational overhead?

- A. Move the data to the S3 bucket. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use the built-in key rotation behavior of SSE-S3 encryption keys.
- B. Create an AWS Key Management Service (AWS KMS) customer managed key. Enable automatic key rotation. Set the S3 bucket's default encryption behavior to use the customer managed KMS key. Move the data to the S3 bucket.
- C. Create an AWS Key Management Service (AWS KMS) customer managed key. Set the S3 bucket's default encryption behavior to use the customer managed KMS key. Move the data to the S3 bucket. Manually rotate the KMS key every year.
- D. Encrypt the data with customer key material before moving the data to the S3 bucket. Create an AWS Key Management Service (AWS KMS) key without key material. Import the customer key material into the KMS key. Enable automatic key rotation.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* SSE-KMS with a customer managed key allows you to enable automatic annual key rotation (KMS rotates the backing key). This meets requirements with minimal overhead. SSE-S3 (A) does not provide key rotation visibility/control. Option C requires manual rotation. Option D (imported key material) cannot be automatically rotated by KMS. (AWS Docs: KMS Key Rotation).
</details>

---

**Q203.** The customers of a finance company request appointments with financial advisors by sending text messages. A web application that runs on Amazon EC2 instances accepts the appointment requests. The text messages are published to an Amazon Simple Queue Service (Amazon SQS) queue through the web application. Another application that runs on EC2 instances then sends meeting invitations and meeting confirmation email messages to the customers. After successful scheduling, this application stores the meeting information in an Amazon DynamoDB database. As the company expands, customers report that their meeting invitations are taking longer to arrive. What should a solutions architect recommend to resolve this issue?

- A. Add a DynamoDB Accelerator (DAX) cluster in front of the DynamoDB database.
- B. Add an Amazon API Gateway API in front of the web application that accepts the appointment requests.
- C. Add an Amazon CloudFront distribution. Set the origin as the web application that accepts the appointment requests.
- D. Add an Auto Scaling group for the application that sends meeting invitations. Configure the Auto Scaling group to scale based on the depth of the SQS queue.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* The delay is likely due to the EC2 instances processing the SQS queue being overloaded. Adding an Auto Scaling group for the consumer application and scaling based on queue depth (ApproximateNumberOfMessages) will process invitations faster. DAX (A) helps DynamoDB read latency, but the issue is processing. API Gateway/CloudFront (B/C) don't address the processing bottleneck. (AWS Docs: SQS Queue-Based Scaling).
</details>

---

**Q204.** An online retail company has more than 50 million active customers and receives more than 25,000 orders each day. The company collects purchase data for customers and stores this data in Amazon S3. Additional customer data is stored in Amazon RDS. The company wants to make all the data available to various teams so that the teams can perform analytics. The solution must provide the ability to manage fine-grained permissions for the data and must minimize operational overhead. Which solution will meet these requirements?

- A. Migrate the purchase data to write directly to Amazon RDS. Use RDS access controls to limit access.
- B. Schedule an AWS Lambda function to periodically copy data from Amazon RDS to Amazon S3. Create an AWS Glue crawler. Use Amazon Athena to query the data. Use S3 policies to limit access.
- C. Create a data lake by using AWS Lake Formation. Create an AWS Glue JDBC connection to Amazon RDS. Register the S3 bucket in Lake Formation. Use Lake Formation access controls to limit access.
- D. Create an Amazon Redshift cluster. Schedule an AWS Lambda function to periodically copy data from Amazon S3 and Amazon RDS to Amazon Redshift. Use Amazon Redshift access controls to limit access.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Lake Formation simplifies building a data lake on S3, allows fine-grained permissions (row/cell-level), and integrates with Glue to catalog RDS data. It minimizes operational overhead for managing analytics access. Option A (RDS) is not scalable for analytics. Option B uses S3 policies (coarse). Option D (Redshift) adds complexity and cost. (AWS Docs: AWS Lake Formation).
</details>

---

**Q205.** A company hosts a marketing website in an on-premises data center. The website consists of static documents and runs on a single server. An administrator updates the website content infrequently and uses an SFTP client to upload new documents. The company decides to host its website on AWS and to use Amazon CloudFront. The company's solutions architect creates a CloudFront distribution. The solutions architect must design the most cost-effective and resilient architecture for website hosting to serve as the CloudFront origin. Which solution will meet these requirements?

- A. Create a virtual server by using Amazon Lightsail. Configure the web server in the Lightsail instance. Upload website content by using an SFTP client.
- B. Create an AWS Auto Scaling group for Amazon EC2 instances. Use an Application Load Balancer. Upload website content by using an SFTP client.
- C. Create a private Amazon S3 bucket. Use an S3 bucket policy to allow access from a CloudFront origin access identity (OAI). Upload website content by using the AWS CLI.
- D. Create a public Amazon S3 bucket. Configure AWS Transfer for SFTP. Configure the S3 bucket for website hosting. Upload website content by using the SFTP client.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* S3 is the most cost-effective and resilient origin for static websites. Using a private bucket with OAI ensures content is only accessible via CloudFront. Uploading via AWS CLI (or console) is acceptable. Option A (Lightsail) and B (EC2) have higher cost and operational overhead. Option D (public bucket) is insecure. (AWS Docs: CloudFront with S3 OAI).
</details>

---

**Q206.** A company wants to manage Amazon Machine Images (AMIs). The company currently copies AMIs to the same AWS Region where the AMIs were created. The company needs to design an application that captures AWS API calls and sends alerts whenever the Amazon EC2 CreateImage API operation is called within the company's account. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an AWS Lambda function to query AWS CloudTrail logs and to send an alert when a CreateImage API call is detected.
- B. Configure AWS CloudTrail with an Amazon Simple Notification Service (Amazon SNS) notification that occurs when updated logs are sent to Amazon S3. Use Amazon Athena to create a new table and to query on CreateImage when an API call is detected.
- C. Create an Amazon EventBridge (Amazon CloudWatch Events) rule for the CreateImage API call. Configure the target as an Amazon Simple Notification Service (Amazon SNS) topic to send an alert when a CreateImage API call is detected.
- D. Configure an Amazon Simple Queue Service (Amazon SQS) FIFO queue as a target for AWS CloudTrail logs. Create an AWS Lambda function to send an alert to an Amazon Simple Notification Service (Amazon SNS) topic when a CreateImage API call is detected.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* EventBridge can directly match specific API calls (like `CreateImage`) and trigger an SNS topic, with no custom code or log parsing. This is the least operational overhead. Options A, B, D require custom Lambda or Athena queries. (AWS Docs: EventBridge for EC2 API Calls).
</details>

---

**Q207.** A company owns an asynchronous API that is used to ingest user requests and, based on the request type, dispatch requests to the appropriate microservice for processing. The company is using Amazon API Gateway to deploy the API front end, and an AWS Lambda function that invokes Amazon DynamoDB to store user requests before dispatching them to the processing microservices. The company provisioned as much DynamoDB throughput as its budget allows, but the company is still experiencing availability issues and is losing user requests. What should a solutions architect do to address this issue without impacting existing users?

- A. Add throttling on the API Gateway with server-side throttling limits.
- B. Use DynamoDB Accelerator (DAX) and Lambda to buffer writes to DynamoDB.
- C. Create a secondary index in DynamoDB for the table with the user requests.
- D. Use the Amazon Simple Queue Service (Amazon SQS) queue and Lambda to buffer writes to DynamoDB.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* The issue is that DynamoDB writes are throttling. Using an SQS queue as a buffer between API Gateway/Lambda and DynamoDB decouples the components and absorbs write spikes. Lambda can then write to DynamoDB at a controlled rate. Option A (throttling) would drop requests. DAX (B) is for reads. Secondary index (C) doesn't fix write throttling. (AWS Docs: SQS as Buffer for DynamoDB Writes).
</details>

---

**Q208.** A company needs to move data from an Amazon EC2 instance to an Amazon S3 bucket. The company must ensure that no API calls and no data are routed through public internet routes. Only the EC2 instance can have access to upload data to the S3 bucket. Which solution will meet these requirements?

- A. Create an interface VPC endpoint for Amazon S3 in the subnet where the EC2 instance is located. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.
- B. Create a gateway VPC endpoint for Amazon S3 in the Availability Zone where the EC2 instance is located. Attach appropriate security groups to the endpoint. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.
- C. Run the nslookup tool from inside the EC2 instance to obtain the private IP address of the S3 bucket's service API endpoint. Create a route in the VPC route table to provide the EC2 instance with access to the S3 bucket. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.
- D. Use the AWS provided, publicly available ip-ranges.json file to obtain the private IP address of the S3 bucket's service API endpoint. Create a route in the VPC route table to provide the EC2 instance with access to the S3 bucket. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A gateway VPC endpoint for S3 (B) is the correct way to provide private, internet-free access. Interface endpoints (A) are for other services, not S3 (S3 uses gateway endpoints). Security groups can be attached to gateway endpoints. Options C and D are incorrect; S3 endpoints do not have private IPs you can nslookup. (AWS Docs: Gateway VPC Endpoints for S3).
</details>

---

**Q209.** A solutions architect is designing the architecture of a new application being deployed to the AWS Cloud. The application will run on Amazon EC2 On-Demand Instances and will automatically scale across multiple Availability Zones. The EC2 instances will scale up and down frequently throughout the day. An Application Load Balancer (ALB) will handle the load distribution. The architecture needs to support distributed session data management. The company is willing to make changes to code if needed. What should the solutions architect do to ensure that the architecture supports distributed session data management?

- A. Use Amazon ElastiCache to manage and store session data.
- B. Use session affinity (sticky sessions) of the ALB to manage session data.
- C. Use Session Manager from AWS Systems Manager to manage the session.
- D. Use the GetSessionToken API operation in AWS Security Token Service (AWS STS) to manage the session.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* For distributed session management with auto-scaling instances, externalize session storage to a shared cache like ElastiCache (Redis). Sticky sessions (B) tie a user to a specific instance, which breaks when the instance scales in or fails. Session Manager (C) is for shell access. GetSessionToken (D) is for temporary credentials. (AWS Docs: ElastiCache for Session Management).
</details>

---

**Q210.** A company offers a food delivery service that is growing rapidly. Because of the growth, the company's order processing system is experiencing scaling problems during peak traffic hours. The current architecture includes the following: - A group of Amazon EC2 instances that run in an Amazon EC2 Auto Scaling group to collect orders from the application - Another group of EC2 instances that run in an Amazon EC2 Auto Scaling group to fulfill orders. The order collection process occurs quickly, but the order fulfillment process can take longer. Data must not be lost because of a scaling event. A solutions architect must ensure that the order collection process and the order fulfillment process can both scale properly during peak traffic hours. The solution must optimize utilization of the company's AWS resources. Which solution meets these requirements?

- A. Use Amazon CloudWatch metrics to monitor the CPU of each instance in the Auto Scaling groups. Configure each Auto Scaling group's minimum capacity according to peak workload values.
- B. Use Amazon CloudWatch metrics to monitor the CPU of each instance in the Auto Scaling groups. Configure a CloudWatch alarm to invoke an Amazon Simple Notification Service (Amazon SNS) topic that creates additional Auto Scaling groups on demand.
- C. Provision two Amazon Simple Queue Service (Amazon SQS) queues: one for order collection and another for order fulfillment. Configure the EC2 instances to poll their respective queue. Scale the Auto Scaling groups based on notifications that the queues send.
- D. Provision two Amazon Simple Queue Service (Amazon SQS) queues: one for order collection and another for order fulfillment. Configure the EC2 instances to poll their respective queue. Create a metric based on a backlog per instance calculation. Scale the Auto Scaling groups based on this metric.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Using SQS queues decouples the collection and fulfillment processes. Scaling based on "backlog per instance" (e.g., ApproximateNumberOfMessages / number of instances) is the most efficient and accurate way to scale each tier based on its own workload. Option A (min capacity) wastes resources; Option C (notifications) is not a scaling metric; Option D is the documented best practice. (AWS Docs: SQS Queue-Based Scaling).
</details>

---

**Q211.** A company hosts multiple production applications. One of the applications consists of resources from Amazon EC2, AWS Lambda, Amazon RDS, Amazon Simple Notification Service (Amazon SNS), and Amazon Simple Queue Service (Amazon SQS) across multiple AWS Regions. All company resources are tagged with a tag name of "application" and a value that corresponds to each application. A solutions architect must provide the quickest solution for identifying all of the tagged components. Which solution meets these requirements?

- A. Use AWS CloudTrail to generate a list of resources with the application tag.
- B. Use the AWS CLI to query each service across all Regions to report the tagged components.
- C. Run a query in Amazon CloudWatch Logs Insights to report on the components with the application tag.
- D. Run a query with the AWS Resource Groups Tag Editor to report on the resources globally with the application tag.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Resource Groups Tag Editor allows you to search for resources across multiple services and Regions based on tags, providing a quick, centralized view. CloudTrail (A) is for API logs. AWS CLI (B) is slow and requires scripting. CloudWatch Logs (C) is for log data. (AWS Docs: Tag Editor).
</details>

---

**Q212.** A company needs to export its database once a day to Amazon S3 for other teams to access. The exported object size varies between 2 GB and 5 GB. The S3 access pattern for the data is variable and changes rapidly. The data must be immediately available and must remain accessible for up to 3 months. The company needs the most cost-effective solution that will not increase retrieval time. Which S3 storage class should the company use to meet these requirements?

- A. S3 Intelligent-Tiering
- B. S3 Glacier Instant Retrieval
- C. S3 Standard
- D. S3 Standard-Infrequent Access (S3 Standard-IA)

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Intelligent-Tiering automatically moves objects between frequent and infrequent access tiers based on changing patterns, with no retrieval delay. It is cost-effective for "variable and rapidly changing" access patterns. Standard (C) is more expensive for infrequent access. Standard-IA (D) has a minimum storage duration charge (30 days) and retrieval fees. Glacier Instant Retrieval (B) has higher retrieval costs. (AWS Docs: S3 Intelligent-Tiering).
</details>

---

**Q213.** A company is developing a new mobile app. The company must implement proper traffic filtering to protect its Application Load Balancer (ALB) against common application-level attacks, such as cross-site scripting or SQL injection. The company has minimal infrastructure and operational staff. The company needs to reduce its share of the responsibility in managing, updating, and securing servers for its AWS environment. What should a solutions architect recommend to meet these requirements?

- A. Configure AWS WAF rules and associate them with the ALB.
- B. Deploy the application using Amazon S3 with public hosting enabled.
- C. Deploy AWS Shield Advanced and add the ALB as a protected resource.
- D. Create a new ALB that directs traffic to an Amazon EC2 instance running a third-party firewall, which then passes the traffic to the current ALB.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS WAF is a managed service that protects against SQL injection, XSS, and other web exploits. It integrates directly with ALB and requires no infrastructure management. Shield Advanced (C) is for DDoS. Option B (S3) is for static sites. Option D adds complexity. (AWS Docs: AWS WAF with ALB).
</details>

---

**Q214.** A company's reporting system delivers hundreds of .csv files to an Amazon S3 bucket each day. The company must convert these files to Apache Parquet format and must store the files in a transformed data bucket. Which solution will meet these requirements with the LEAST development effort?

- A. Create an Amazon EMR cluster with Apache Spark installed. Write a Spark application to transform the data. Use EMR File System (EMRFS) to write files to the transformed data bucket.
- B. Create an AWS Glue crawler to discover the data. Create an AWS Glue extract, transform, and load (ETL) job to transform the data. Specify the transformed data bucket in the output step.
- C. Use AWS Batch to create a job definition with Bash syntax to transform the data and output the data to the transformed data bucket. Use the job definition to submit a job. Specify an array job as the job type.
- D. Create an AWS Lambda function to transform the data and output the data to the transformed data bucket. Configure an event notification for the S3 bucket. Specify the Lambda function as the destination for the event notification.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Glue provides a serverless ETL service that can convert CSV to Parquet with minimal coding (using visual ETL or simple scripts). It is designed for this exact use case. EMR (A) requires more development and cluster management. Lambda (D) has time/memory limits for large files. Batch (C) requires managing compute. (AWS Docs: AWS Glue ETL).
</details>

---

**Q215.** A company has 700 TB of backup data stored in network attached storage (NAS) in its data center. This backup data needs to be accessible for infrequent regulatory requests and must be retained for 7 years. The company has decided to migrate this backup data from its data center to AWS. The migration must be complete within 1 month. The company has 500 Mbps of dedicated bandwidth on its public internet connection available for data transfer. What should a solutions architect do to migrate and store the data at the LOWEST cost?

- A. Order AWS Snowball devices to transfer the data. Use a lifecycle policy to transition the files to Amazon S3 Glacier Deep Archive.
- B. Deploy a VPN connection between the data center and Amazon VPC. Use the AWS CLI to copy the data from on premises to Amazon S3 Glacier.
- C. Provision a 500 Mbps AWS Direct Connect connection and transfer the data to Amazon S3. Use a lifecycle policy to transition the files to Amazon S3 Glacier Deep Archive.
- D. Use AWS DataSync to transfer the data and deploy a DataSync agent on premises. Use the DataSync task to copy files from the on-premises NAS storage to Amazon S3 Glacier.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* 700 TB over 500 Mbps would take over 130 days (700 TB / (500 Mbps / 8) = ~130 days), exceeding 1 month. Snowball devices (physical transfer) are faster and do not use internet bandwidth. After transfer to S3, lifecycle policy moves to Glacier Deep Archive for lowest-cost storage. Option D (DataSync) would use bandwidth and take too long. Direct Connect (C) has high cost and setup time. (AWS Docs: Snowball for Large Transfers).
</details>

---

**Q216.** A company has a serverless website with millions of objects in an Amazon S3 bucket. The company uses the S3 bucket as the origin for an Amazon CloudFront distribution. The company did not set encryption on the S3 bucket before the objects were loaded. A solutions architect needs to enable encryption for all existing objects and for all objects that are added to the S3 bucket in the future. Which solution will meet these requirements with the LEAST amount of effort?

- A. Create a new S3 bucket. Turn on the default encryption settings for the new S3 bucket. Download all existing objects to temporary local storage. Upload the objects to the new S3 bucket.
- B. Turn on the default encryption settings for the S3 bucket. Use the S3 Inventory feature to create a .csv file that lists the unencrypted objects. Run an S3 Batch Operations job that uses the copy command to encrypt those objects.
- C. Create a new encryption key by using AWS Key Management Service (AWS KMS). Change the settings on the S3 bucket to use server-side encryption with AWS KMS managed encryption keys (SSE-KMS). Turn on versioning for the S3 bucket.
- D. Navigate to Amazon S3 in the AWS Management Console. Browse the S3 bucket's objects. Sort by the encryption field. Select each unencrypted object. Use the Modify button to apply default encryption settings to every unencrypted object in the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Batch Operations can encrypt existing unencrypted objects in-place by copying them over themselves with encryption enabled. Default encryption handles future objects. This requires minimal effort. Option A (download/upload) is slow and costly. Option C (change settings) only affects new objects. Option D is manual and not scalable for millions of objects. (AWS Docs: S3 Batch Operations for Encryption).
</details>

---

**Q217.** A company runs a global web application on Amazon EC2 instances behind an Application Load Balancer. The application stores data in Amazon Aurora. The company needs to create a disaster recovery solution and can tolerate up to 30 minutes of downtime and potential data loss. The solution does not need to handle the load when the primary infrastructure is healthy. What should a solutions architect do to meet these requirements?

- A. Deploy the application with the required infrastructure elements in place. Use Amazon Route 53 to configure active-passive failover. Create an Aurora Replica in a second AWS Region.
- B. Host a scaled-down deployment of the application in a second AWS Region. Use Amazon Route 53 to configure active-active failover. Create an Aurora Replica in the second Region.
- C. Replicate the primary infrastructure in a second AWS Region. Use Amazon Route 53 to configure active-active failover. Create an Aurora database that is restored from the latest snapshot.
- D. Back up data with AWS Backup. Use the backup to create the required infrastructure in a second AWS Region. Use Amazon Route 53 to configure active-passive failover. Create an Aurora second primary instance in the second Region.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* An active-passive failover configuration with an Aurora Replica in a second Region provides a DR solution. The replica is asynchronous, allowing some data loss (RPO). The passive site does not handle load until failover. Option B (active-active) would handle load, which is not required. Option C (active-active) is overkill. Option D (Backup) has higher RTO. (AWS Docs: Aurora Global Database for DR).
</details>

---

**Q218.** A company has a web server running on an Amazon EC2 instance in a public subnet with an Elastic IP address. The default security group is assigned to the EC2 instance. The default network ACL has been modified to block all traffic. A solutions architect needs to make the web server accessible from everywhere on port 443. Which combination of steps will accomplish this task? (Choose two.)

- A. Create a security group with a rule to allow TCP port 443 from source 0.0.0.0/0.
- B. Create a security group with a rule to allow TCP port 443 to destination 0.0.0.0/0.
- C. Update the network ACL to allow TCP port 443 from source 0.0.0.0/0.
- D. Update the network ACL to allow inbound/outbound TCP port 443 from source 0.0.0.0/0 and to destination 0.0.0.0/0.
- E. Update the network ACL to allow inbound TCP port 443 from source 0.0.0.0/0 and outbound TCP port 32768-65535 to destination 0.0.0.0/0.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* Security group (A) needs an inbound rule for HTTPS (port 443) from anywhere. Network ACLs are stateless, so you need both inbound rule for port 443 (E) and outbound rule for ephemeral ports (32768-65535) to allow return traffic. Option D (allowing port 443 outbound) is not sufficient; ephemeral ports are needed. (AWS Docs: Stateless NACL Rules).
</details>

---

**Q219.** A company's application is having performance issues. The application is stateful and needs to complete in-memory tasks on Amazon EC2 instances. The company used AWS CloudFormation to deploy infrastructure and used the M5 EC2 instance family. As traffic increased, the application performance degraded. Users are reporting delays when the users attempt to access the application. Which solution will resolve these issues in the MOST operationally efficient way?

- A. Replace the EC2 instances with T3 EC2 instances that run in an Auto Scaling group. Make the changes by using the AWS Management Console.
- B. Modify the CloudFormation templates to run the EC2 instances in an Auto Scaling group. Increase the desired capacity and the maximum capacity of the Auto Scaling group manually when an increase is necessary.
- C. Modify the CloudFormation templates. Replace the EC2 instances with R5 EC2 instances. Use Amazon CloudWatch built-in EC2 memory metrics to track the application performance for future capacity planning.
- D. Modify the CloudFormation templates. Replace the EC2 instances with R5 EC2 instances. Deploy the Amazon CloudWatch agent on the EC2 instances to generate custom application latency metrics for future capacity planning.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* The issue likely requires more memory (stateful, in-memory tasks), so switching to R5 (memory-optimized) family is appropriate. Deploying the CloudWatch agent to collect custom metrics (including memory usage) is necessary for future capacity planning, as EC2 does not provide built-in memory metrics. Option C incorrectly states built-in memory metrics. (AWS Docs: CloudWatch Agent for Memory Metrics).
</details>

---

**Q220.** A solutions architect is designing a new API using Amazon API Gateway that will receive requests from users. The volume of requests is highly variable; several hours can pass without receiving a single request. The data processing will take place asynchronously, but should be completed within a few seconds after a request is made. Which compute service should the solutions architect have the API invoke to deliver the requirements at the lowest cost?

- A. An AWS Glue job
- B. An AWS Lambda function
- C. A containerized service hosted in Amazon Elastic Kubernetes Service (Amazon EKS)
- D. A containerized service hosted in Amazon ECS with Amazon EC2

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Lambda is ideal for asynchronous processing with highly variable traffic (including zero). You pay only for invocations and duration. It can complete within seconds. Glue (A) is for ETL jobs. EKS/ECS (C/D) have fixed costs for control plane/nodes, even when idle. (AWS Docs: AWS Lambda Pricing).
</details>

---

**Q221.** A company runs an application on a group of Amazon Linux EC2 instances. For compliance reasons, the company must retain all application log files for 7 years. The log files will be analyzed by a reporting tool that must be able to access all the files concurrently. Which storage solution meets these requirements MOST cost-effectively?

- A. Amazon Elastic Block Store (Amazon EBS)
- B. Amazon Elastic File System (Amazon EFS)
- C. Amazon EC2 instance store
- D. Amazon S3

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 is the most cost-effective solution for long-term storage (7 years) of log files, and multiple reporting tools can access the files concurrently via API. EBS (A) and EFS (B) are more expensive for this volume/retention. Instance store (C) is ephemeral. (AWS Docs: S3 for Log Storage).
</details>

---

**Q222.** A company has hired an external vendor to perform work in the company's AWS account. The vendor uses an automated tool that is hosted in an AWS account that the vendor owns. The vendor does not have IAM access to the company's AWS account. How should a solutions architect grant this access to the vendor?

- A. Create an IAM role in the company's account to delegate access to the vendor's IAM role. Attach the appropriate IAM policies to the role for the permissions that the vendor requires.
- B. Create an IAM user in the company's account with a password that meets the password complexity requirements. Attach the appropriate IAM policies to the user for the permissions that the vendor requires.
- C. Create an IAM group in the company's account. Add the tool's IAM user from the vendor account to the group. Attach the appropriate IAM policies to the group for the permissions that the vendor requires.
- D. Create a new identity provider by choosing "AWS account" as the provider type in the IAM console. Supply the vendor's AWS account ID and user name. Attach the appropriate IAM policies to the new provider for the permissions that the vendor requires.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Cross-account IAM roles are the standard way to grant access to principals from another AWS account. You create a role in your account with a trust policy that allows the vendor's account to assume it. The vendor's automated tool assumes this role. Options B (IAM user) and C (group) are less secure and not cross-account. Option D (identity provider) is for federated users, not AWS accounts. (AWS Docs: Cross-Account IAM Roles).
</details>

---

**Q223.** A company has deployed a Java Spring Boot application as a pod that runs on Amazon Elastic Kubernetes Service (Amazon EKS) in private subnets. The application needs to write data to an Amazon DynamoDB table. A solutions architect must ensure that the application can interact with the DynamoDB table without exposing traffic to the internet. Which combination of steps should the solutions architect take to accomplish this goal? (Choose two.)

- A. Attach an IAM role that has sufficient privileges to the EKS pod.
- B. Attach an IAM user that has sufficient privileges to the EKS pod.
- C. Allow outbound connectivity to the DynamoDB table through the private subnets' network ACLs.
- D. Create a VPC endpoint for DynamoDB.
- E. Embed the access keys in the Java Spring Boot code.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* Create a VPC endpoint (Gateway endpoint) for DynamoDB (D) to enable private connectivity. Attach an IAM role to the EKS pod (via service account) (A) to grant permissions to write to DynamoDB. Option B (IAM user) and E (embed keys) are insecure. NACLs (C) alone don't provide private connectivity to DynamoDB. (AWS Docs: EKS Pod Identity with IAM Roles for Service Accounts, DynamoDB VPC Endpoint).
</details>

---

**Q224.** A company recently migrated its web application to AWS by rehosting the application on Amazon EC2 instances in a single AWS Region. The company wants to redesign its application architecture to be highly available and fault tolerant. Traffic must reach all running EC2 instances randomly. Which combination of steps should the company take to meet these requirements? (Choose two.)

- A. Create an Amazon Route 53 failover routing policy.
- B. Create an Amazon Route 53 weighted routing policy.
- C. Create an Amazon Route 53 multivalue answer routing policy.
- D. Launch three EC2 instances: two instances in one Availability Zone and one instance in another Availability Zone.
- E. Launch four EC2 instances: two instances in one Availability Zone and two instances in another Availability Zone.

<details>
<summary>Show Answer</summary>

**Answer: C, E**

*Explanation:* A multivalue answer routing policy (C) lets Route 53 return multiple IP addresses randomly, and it can be associated with health checks. To be fault tolerant, instances should be distributed evenly across at least two AZs (E). Option D is uneven. Weighted (B) is for splitting traffic by percentage. Failover (A) is for active-passive. (AWS Docs: Multivalue Answer Routing, Multi-AZ HA).
</details>

---

**Q225.** A media company collects and analyzes user activity data on premises. The company wants to migrate this capability to AWS. The user activity data store will continue to grow and will be petabytes in size. The company needs to build a highly available data ingestion solution that facilitates on-demand analytics of existing data and new data with SQL. Which solution will meet these requirements with the LEAST operational overhead?

- A. Send activity data to an Amazon Kinesis data stream. Configure the stream to deliver the data to an Amazon S3 bucket.
- B. Send activity data to an Amazon Kinesis Data Firehose delivery stream. Configure the stream to deliver the data to an Amazon Redshift cluster.
- C. Place activity data in an Amazon S3 bucket. Configure Amazon S3 to run an AWS Lambda function on the data as the data arrives in the S3 bucket.
- D. Create an ingestion service on Amazon EC2 instances that are spread across multiple Availability Zones. Configure the service to forward data to an Amazon RDS Multi-AZ database.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Kinesis Data Streams can ingest petabyte-scale data. Use a consumer (e.g., Firehose or Lambda) to deliver to S3. Then use Athena or Redshift Spectrum for SQL analytics. This is serverless and highly available. Option B (Firehose to Redshift) requires a Redshift cluster (operational overhead). Option C (Lambda) has time limits. Option D (EC2/RDS) has high overhead. (AWS Docs: Kinesis to S3 Data Lake).
</details>

---

**Q226.** A company collects data from thousands of remote devices by using a RESTful web services application that runs on an Amazon EC2 instance. The EC2 instance receives the raw data, transforms the raw data, and stores all the data in an Amazon S3 bucket. The number of remote devices will increase into the millions soon. The company needs a highly scalable solution that minimizes operational overhead. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- A. Use AWS Glue to process the raw data in Amazon S3.
- B. Use Amazon Route 53 to route traffic to different EC2 instances.
- C. Add more EC2 instances to accommodate the increasing amount of incoming data.
- D. Send the raw data to Amazon Simple Queue Service (Amazon SQS). Use EC2 instances to process the data.
- E. Use Amazon API Gateway to send the raw data to an Amazon Kinesis data stream. Configure Amazon Kinesis Data Firehose to use the data stream as a source to deliver the data to Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* A serverless ingestion pipeline: API Gateway -> Kinesis Data Stream -> Firehose -> S3 (E). Use Glue (A) to transform data in S3 (or use Lambda on the stream). This scales to millions of devices with minimal overhead. Options B, C, D rely on EC2 instances which require scaling management. (AWS Docs: Serverless Data Ingestion).
</details>

---

**Q227.** A company needs to retain its AWS CloudTrail logs for 3 years. The company is enforcing CloudTrail across a set of AWS accounts by using AWS Organizations from the parent account. The CloudTrail target S3 bucket is configured with S3 Versioning enabled. An S3 Lifecycle policy is in place to delete current objects after 3 years. After the fourth year of use of the S3 bucket, the S3 bucket metrics show that the number of objects has continued to rise. However, the number of new CloudTrail logs that are delivered to the S3 bucket has remained consistent. Which solution will delete objects that are older than 3 years in the MOST cost-effective manner?

- A. Configure the organization's centralized CloudTrail trail to expire objects after 3 years.
- B. Configure the S3 Lifecycle policy to delete previous versions as well as current versions.
- C. Create an AWS Lambda function to enumerate and delete objects from Amazon S3 that are older than 3 years.
- D. Configure the parent account as the owner of all objects that are delivered to the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* With versioning enabled, old versions of objects accumulate. The lifecycle policy only deletes current objects after 3 years, but previous versions remain. To delete all objects older than 3 years (including previous versions), configure the lifecycle policy to also delete noncurrent versions (e.g., `NoncurrentVersionExpiration`). Option A (CloudTrail) doesn't manage versions. Option C (Lambda) is more costly and complex. (AWS Docs: S3 Lifecycle for Noncurrent Versions).
</details>

---

**Q228.** A company has an API that receives real-time data from a fleet of monitoring devices. The API stores this data in an Amazon RDS DB instance for later analysis. The amount of data that the monitoring devices send to the API fluctuates. During periods of heavy traffic, the API often returns timeout errors. After an inspection of the logs, the company determines that the database is not capable of processing the volume of write traffic that comes from the API. A solutions architect must minimize the number of connections to the database and must ensure that data is not lost during periods of heavy traffic. Which solution will meet these requirements?

- A. Increase the size of the DB instance to an instance type that has more available memory.
- B. Modify the DB instance to be a Multi-AZ DB instance. Configure the application to write to all active RDS DB instances.
- C. Modify the API to write incoming data to an Amazon Simple Queue Service (Amazon SQS) queue. Use an AWS Lambda function that Amazon SQS invokes to write data from the queue to the database.
- D. Modify the API to write incoming data to an Amazon Simple Notification Service (Amazon SNS) topic. Use an AWS Lambda function that Amazon SNS invokes to write data from the topic to the database.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Using SQS as a buffer decouples the API from the database, absorbs write spikes, and prevents data loss. Lambda can then write to the database at a manageable rate. This also reduces direct connections to the DB. Option A (scaling up) might not handle bursts. Option B (Multi-AZ) doesn't increase write capacity. Option D (SNS) doesn't provide buffering like SQS. (AWS Docs: SQS for Database Write Buffering).
</details>

---

**Q229.** A company manages its own Amazon EC2 instances that run MySQL databases. The company is manually managing replication and scaling as demand increases or decreases. The company needs a new solution that simplifies the process of adding or removing compute capacity to or from its database tier as needed. The solution also must offer improved performance, scaling, and durability with minimal effort from operations. Which solution meets these requirements?

- A. Migrate the databases to Amazon Aurora Serverless for Aurora MySQL.
- B. Migrate the databases to Amazon Aurora Serverless for Aurora PostgreSQL.
- C. Combine the databases into one larger MySQL database. Run the larger database on larger EC2 instances.
- D. Create an EC2 Auto Scaling group for the database tier. Migrate the existing databases to the new environment.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Aurora Serverless automatically scales compute capacity based on demand and is fully managed, reducing operational overhead. It offers improved performance and durability over self-managed MySQL on EC2. Option B is PostgreSQL (not compatible). Option C/D still require manual management. (AWS Docs: Aurora Serverless).
</details>

---

**Q230.** A company is concerned that two NAT instances in use will no longer be able to support the traffic needed for the company's application. A solutions architect wants to implement a solution that is highly available, fault tolerant, and automatically scalable. What should the solutions architect recommend?

- A. Remove the two NAT instances and replace them with two NAT gateways in the same Availability Zone.
- B. Use Auto Scaling groups with Network Load Balancers for the NAT instances in different Availability Zones.
- C. Remove the two NAT instances and replace them with two NAT gateways in different Availability Zones.
- D. Replace the two NAT instances with Spot Instances in different Availability Zones and deploy a Network Load Balancer.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* NAT gateways are a managed, highly available, and scalable service. Deploying one NAT gateway in each of two different Availability Zones provides fault tolerance. This is superior to managing NAT instances (B, D). Option A (same AZ) is not fault tolerant. (AWS Docs: NAT Gateway High Availability).
</details>

---

**Q231.** An application runs on an Amazon EC2 instance that has an Elastic IP address in VPC A. The application requires access to a database in VPC B. Both VPCs are in the same AWS account. Which solution will provide the required access MOST securely?

- A. Create a DB instance security group that allows all traffic from the public IP address of the application server in VPC A.
- B. Configure a VPC peering connection between VPC A and VPC B.
- C. Make the DB instance publicly accessible. Assign a public IP address to the DB instance.
- D. Launch an EC2 instance with an Elastic IP address into VPC B. Proxy all requests through the new EC2 instance.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* VPC peering allows private, secure communication between two VPCs using private IP addresses, without traversing the internet. Option A (public IP) and C (publicly accessible) expose the database to the internet. Option D adds unnecessary complexity. (AWS Docs: VPC Peering).
</details>

---

**Q232.** A company runs demonstration environments for its customers on Amazon EC2 instances. Each environment is isolated in its own VPC. The company's operations team needs to be notified when RDP or SSH access to an environment has been established. What should a solutions architect do to meet these requirements?

- A. Configure Amazon CloudWatch Application Insights to create AWS Systems Manager Opsitems when RDP or SSH access is detected.
- B. Configure the EC2 instances with an IAM instance profile that has an IAM role with the AmazonSSMManagedInstanceCore policy attached.
- C. Publish VPC flow logs to Amazon CloudWatch Logs. Create required metric filters. Create an Amazon CloudWatch metric alarm with a notification action for when the alarm is in the ALARM state.
- D. Configure an Amazon EventBridge rule to listen for events of type EC2 Instance State-change Notification. Configure an Amazon Simple Notification Service (Amazon SNS) topic as a target. Subscribe the operations team to the topic.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* VPC Flow Logs can capture traffic on port 22 (SSH) and 3389 (RDP). Publish to CloudWatch Logs, create metric filters for these ports, and set an alarm to trigger SNS. Option A (Application Insights) is for application monitoring. Option B (SSM) doesn't detect access. Option D (state-change) detects instance start/stop, not access. (AWS Docs: VPC Flow Logs with CloudWatch Alarms).
</details>

---

**Q233.** A solutions architect has created a new AWS account and must secure AWS account root user access. Which combination of actions will accomplish this? (Choose two.)

- A. Ensure the root user uses a strong password.
- B. Enable multi-factor authentication to the root user.
- C. Store root user access keys in an encrypted Amazon S3 bucket.
- D. Add the root user to a group containing administrative permissions.
- E. Apply the required permissions to the root user with an inline policy document.

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* Best practices for securing the root user include using a strong password and enabling MFA. Access keys should not be created for the root user (C). The root user cannot be added to a group (D) or have inline policies (E); it has full permissions by default. (AWS Docs: Root User Best Practices).
</details>

---

**Q234.** A company is building a new web-based customer relationship management application. The application will use several Amazon EC2 instances that are backed by Amazon Elastic Block Store (Amazon EBS) volumes behind an Application Load Balancer (ALB). The application will also use an Amazon Aurora database. All data for the application must be encrypted at rest and in transit. Which solution will meet these requirements?

- A. Use AWS Key Management Service (AWS KMS) certificates on the ALB to encrypt data in transit. Use AWS Certificate Manager (ACM) to encrypt the EBS volumes and Aurora database storage at rest.
- B. Use the AWS root account to log in to the AWS Management Console. Upload the company's encryption certificates. While in the root account, select the option to turn on encryption for all data at rest and in transit for the account.
- C. Use AWS Key Management Service (AWS KMS) to encrypt the EBS volumes and Aurora database storage at rest. Attach an AWS Certificate Manager (ACM) certificate to the ALB to encrypt data in transit.
- D. Use BitLocker to encrypt all data at rest. Import the company's TLS certificate keys to AWS Key Management Service (AWS KMS). Attach the KMS keys to the ALB to encrypt data in transit.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* EBS volumes and Aurora storage are encrypted at rest using AWS KMS. Data in transit between client and ALB is encrypted using an SSL/TLS certificate from ACM attached to the ALB. Option A swaps KMS/ACM roles. Option B is incorrect. Option D (BitLocker) is not native. (AWS Docs: EBS Encryption, Aurora Encryption, ALB HTTPS).
</details>

---

**Q235.** A company is moving its on-premises Oracle database to Amazon Aurora PostgreSQL. The database has several applications that write to the same tables. The applications need to be migrated one by one with a month in between each migration. Management has expressed concerns that the database has a high number of reads and writes. The data must be kept in sync across both databases throughout the migration. What should a solutions architect recommend?

- A. Use AWS DataSync for the initial migration. Use AWS Database Migration Service (AWS DMS) to create a change data capture (CDC) replication task and a table mapping to select all tables.
- B. Use AWS DataSync for the initial migration. Use AWS Database Migration Service (AWS DMS) to create a full load plus change data capture (CDC) replication task and a table mapping to select all tables.
- C. Use the AWS Schema Conversion Tool with AWS Database Migration Service (AWS DMS) using a memory optimized replication instance. Create a full load plus change data capture (CDC) replication task and a table mapping to select all tables.
- D. Use the AWS Schema Conversion Tool with AWS Database Migration Service (AWS DMS) using a compute optimized replication instance. Create a full load plus change data capture (CDC) replication task and a table mapping to select the largest tables.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Migrating from Oracle to Aurora PostgreSQL requires schema conversion (AWS SCT). DMS with full load + CDC keeps databases in sync during the phased migration. Use a memory-optimized replication instance for high-change tables. DataSync (A/B) is for file transfer. Option D selects only largest tables (needs all). (AWS Docs: DMS for Heterogeneous Migrations).
</details>

---

**Q236.** A company has a three-tier application for image sharing. The application uses an Amazon EC2 instance for the front-end layer, another EC2 instance for the application layer, and a third EC2 instance for a MySQL database. A solutions architect must design a scalable and highly available solution that requires the least amount of change to the application. Which solution meets these requirements?

- A. Use Amazon S3 to host the front-end layer. Use AWS Lambda functions for the application layer. Move the database to an Amazon DynamoDB table. Use Amazon S3 to store and serve users' images.
- B. Use load-balanced Multi-AZ AWS Elastic Beanstalk environments for the front-end layer and the application layer. Move the database to an Amazon RDS DB instance with multiple read replicas to serve users' images.
- C. Use Amazon S3 to host the front-end layer. Use a fleet of EC2 instances in an Auto Scaling group for the application layer. Move the database to a memory optimized instance type to store and serve users' images.
- D. Use load-balanced Multi-AZ AWS Elastic Beanstalk environments for the front-end layer and the application layer. Move the database to an Amazon RDS Multi-AZ DB instance. Use Amazon S3 to store and serve users' images.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* This solution provides high availability and scalability with minimal changes: Elastic Beanstalk for web/app tiers (supports existing code), RDS Multi-AZ for database HA, and S3 for image storage (scalable). Option A (Lambda/DynamoDB) requires significant code changes. Option B (RDS with replicas) still uses RDS for images (not optimal). Option C lacks HA for database. (AWS Docs: Elastic Beanstalk Multi-AZ, RDS Multi-AZ).
</details>

---

**Q237.** An application running on an Amazon EC2 instance in VPC-A needs to access files in another EC2 instance in VPC-B. Both VPCs are in separate AWS accounts. The network administrator needs to design a solution to configure secure access to EC2 instance in VPC-B from VPC-A. The connectivity should not have a single point of failure or bandwidth concerns. Which solution will meet these requirements?

- A. Set up a VPC peering connection between VPC-A and VPC-B.
- B. Set up VPC gateway endpoints for the EC2 instance running in VPC-B.
- C. Attach a virtual private gateway to VPC-B and set up routing from VPC-A.
- D. Create a private virtual interface (VIF) for the EC2 instance running in VPC-B and add appropriate routes from VPC-A.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* VPC peering can be established across accounts and Regions, provides private connectivity, and has no single point of failure or bandwidth concerns (uses the AWS backbone). Gateway endpoints (B) are for AWS services, not EC2 instances. Virtual private gateway (C) requires VPN. Private VIF (D) is for Direct Connect. (AWS Docs: VPC Peering).
</details>

---

**Q238.** A company wants to experiment with individual AWS accounts for its engineer team. The company wants to be notified as soon as the Amazon EC2 instance usage for a given month exceeds a specific threshold for each account. What should a solutions architect do to meet this requirement MOST cost-effectively?

- A. Use Cost Explorer to create a daily report of costs by service. Filter the report by EC2 instances. Configure Cost Explorer to send an Amazon Simple Email Service (Amazon SES) notification when a threshold is exceeded.
- B. Use Cost Explorer to create a monthly report of costs by service. Filter the report by EC2 instances. Configure Cost Explorer to send an Amazon Simple Email Service (Amazon SES) notification when a threshold is exceeded.
- C. Use AWS Budgets to create a cost budget for each account. Set the period to monthly. Set the scope to EC2 instances. Set an alert threshold for the budget. Configure an Amazon Simple Notification Service (Amazon SNS) topic to receive a notification when a threshold is exceeded.
- D. Use AWS Cost and Usage Reports to create a report with hourly granularity. Integrate the report data with Amazon Athena. Use Amazon EventBridge to schedule an Athena query. Configure an Amazon Simple Notification Service (Amazon SNS) topic to receive a notification when a threshold is exceeded.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Budgets is the simplest and most cost-effective way to set threshold alerts for specific services (EC2) with SNS notifications. Cost Explorer (A/B) does not send alerts natively. Option D (CUR + Athena) is overkill and more expensive. (AWS Docs: AWS Budgets).
</details>

---

**Q239.** A solutions architect needs to design a new microservice for a company's application. Clients must be able to call an HTTPS endpoint to reach the microservice. The microservice also must use AWS Identity and Access Management (IAM) to authenticate calls. The solutions architect will write the logic for this microservice by using a single AWS Lambda function that is written in Go 1.x. Which solution will deploy the function in the MOST operationally efficient way?

- A. Create an Amazon API Gateway REST API. Configure the method to use the Lambda function. Enable IAM authentication on the API.
- B. Create a Lambda function URL for the function. Specify AWS_IAM as the authentication type.
- C. Create an Amazon CloudFront distribution. Deploy the function to Lambda@Edge. Integrate IAM authentication logic into the Lambda@Edge function.
- D. Create an Amazon CloudFront distribution. Deploy the function to CloudFront Functions. Specify AWS_IAM as the authentication type.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Lambda function URLs support IAM authentication natively, providing a direct HTTPS endpoint without requiring API Gateway. This is the most operationally efficient for a single function. API Gateway (A) adds extra service and configuration. Lambda@Edge (C) and CloudFront Functions (D) are for edge processing, not primary endpoints. (AWS Docs: Lambda Function URLs with IAM Auth).
</details>

---

**Q240.** A company previously migrated its data warehouse solution to AWS. The company also has an AWS Direct Connect connection. Corporate office users query the data warehouse using a visualization tool. The average size of a query returned by the data warehouse is 50 MB and each webpage sent by the visualization tool is approximately 500 KB. Result sets returned by the data warehouse are not cached. Which solution provides the LOWEST data transfer egress cost for the company?

- A. Host the visualization tool on premises and query the data warehouse directly over the internet.
- B. Host the visualization tool in the same AWS Region as the data warehouse. Access it over the internet.
- C. Host the visualization tool on premises and query the data warehouse directly over a Direct Connect connection at a location in the same AWS Region.
- D. Host the visualization tool in the same AWS Region as the data warehouse and access it over a Direct Connect connection at a location in the same Region.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Data transfer over Direct Connect between the same Region incurs lower egress costs than internet transfer. Hosting the visualization tool in the same Region as the data warehouse avoids cross-Region transfer costs. Option C would incur egress from AWS to on-premises over DX (still costs). Option D minimizes cost. (AWS Docs: Direct Connect Data Transfer Pricing).
</details>

---

**Q241.** An online learning company is migrating to the AWS Cloud. The company maintains its student records in a PostgreSQL database. The company needs a solution in which its data is available and online across multiple AWS Regions at all times. Which solution will meet these requirements with the LEAST amount of operational overhead?

- A. Migrate the PostgreSQL database to a PostgreSQL cluster on Amazon EC2 instances.
- B. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance with the Multi-AZ feature turned on.
- C. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. Create a read replica in another Region.
- D. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. Set up DB snapshots to be copied to another Region.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* A cross-Region read replica provides an online, readable copy in another Region with automatic asynchronous replication, meeting the "available and online across multiple Regions" requirement. Multi-AZ (B) is within one Region. EC2 cluster (A) has high overhead. Snapshots (D) are not online. (AWS Docs: RDS Cross-Region Read Replicas).
</details>

---

**Q242.** A company hosts its web application on AWS using seven Amazon EC2 instances. The company requires that the IP addresses of all healthy EC2 instances be returned in response to DNS queries. Which policy should be used to meet this requirement?

- A. Simple routing policy
- B. Latency routing policy
- C. Multivalue answer routing policy
- D. Geolocation routing policy

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Multivalue answer routing policy allows you to configure Route 53 to return multiple IP addresses (up to 8) for a query, and it can be associated with health checks to return only healthy IPs. Simple routing (A) returns a single IP. Latency (B) and geolocation (D) are for routing based on location. (AWS Docs: Multivalue Answer Routing).
</details>

---

**Q243.** A medical research lab produces data that is related to a new study. The lab wants to make the data available with minimum latency to clinics across the country for their on-premises, file-based applications. The data files are stored in an Amazon S3 bucket that has read-only permissions for each clinic. What should a solutions architect recommend to meet these requirements?

- A. Deploy an AWS Storage Gateway file gateway as a virtual machine (VM) on premises at each clinic.
- B. Migrate the files to each clinic's on-premises applications by using AWS DataSync for processing.
- C. Deploy an AWS Storage Gateway volume gateway as a virtual machine (VM) on premises at each clinic.
- D. Attach an Amazon Elastic File System (Amazon EFS) file system to each clinic's on-premises servers.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* An S3 File Gateway deployed on-premises at each clinic provides low-latency access to S3 data by caching frequently accessed files locally. It presents an NFS/SMB mount point for on-premises applications. Volume gateway (C) is for block storage. DataSync (B) is for transfer, not low-latency access. EFS (D) cannot be attached on-premises. (AWS Docs: S3 File Gateway).
</details>

---

**Q244.** A company is using a content management system that runs on a single Amazon EC2 instance. The EC2 instance contains both the web server and the database software. The company must make its website platform highly available and must enable the website to scale to meet user demand. What should a solutions architect recommend to meet these requirements?

- A. Move the database to Amazon RDS, and enable automatic backups. Manually launch another EC2 instance in the same Availability Zone. Configure an Application Load Balancer in the Availability Zone, and set the two instances as targets.
- B. Migrate the database to an Amazon Aurora instance with a read replica in the same Availability Zone as the existing EC2 instance. Manually launch another EC2 instance in the same Availability Zone. Configure an Application Load Balancer, and set the two EC2 instances as targets.
- C. Move the database to Amazon Aurora with a read replica in another Availability Zone. Create an Amazon Machine Image (AMI) from the EC2 instance. Configure an Application Load Balancer in two Availability Zones. Attach an Auto Scaling group that uses the AMI across two Availability Zones.
- D. Move the database to a separate EC2 instance, and schedule backups to Amazon S3. Create an Amazon Machine Image (AMI) from the original EC2 instance. Configure an Application Load Balancer in two Availability Zones. Attach an Auto Scaling group that uses the AMI across two Availability Zones.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* To achieve high availability and scalability: separate database to Aurora (managed, HA), use Auto Scaling group with ALB across multiple AZs for web servers, and use an AMI for consistent deployment. Option A/B lack Auto Scaling and multi-AZ for web. Option D uses self-managed database. (AWS Docs: Auto Scaling Multi-AZ, Aurora).
</details>

---

**Q245.** A company is launching an application on AWS. The application uses an Application Load Balancer (ALB) to direct traffic to at least two Amazon EC2 instances in a single target group. The instances are in an Auto Scaling group for each environment. The company requires a development environment and a production environment. The production environment will have periods of high traffic. Which solution will configure the development environment MOST cost-effectively?

- A. Reconfigure the target group in the development environment to have only one EC2 instance as a target.
- B. Change the ALB balancing algorithm to least outstanding requests.
- C. Reduce the size of the EC2 instances in both environments.
- D. Reduce the maximum number of EC2 instances in the development environment's Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Reducing the maximum capacity of the dev environment's Auto Scaling group ensures it cannot scale up to production levels, saving costs. Option A (one instance) reduces HA but may be acceptable; however, changing target group is not the standard way. Option B affects performance, not cost. Option C affects both environments and may impact performance. (AWS Docs: Auto Scaling Group Configuration).
</details>

---

**Q246.** A company runs a web application on Amazon EC2 instances in multiple Availability Zones. The EC2 instances are in private subnets. A solutions architect implements an internet-facing Application Load Balancer (ALB) and specifies the EC2 instances as the target group. However, the internet traffic is not reaching the EC2 instances. How should the solutions architect reconfigure the architecture to resolve this issue?

- A. Replace the ALB with a Network Load Balancer. Configure a NAT gateway in a public subnet to allow internet traffic.
- B. Move the EC2 instances to public subnets. Add a rule to the EC2 instances' security groups to allow outbound traffic to 0.0.0.0/0.
- C. Update the route tables for the EC2 instances' subnets to send 0.0.0.0/0 traffic through the internet gateway route. Add a rule to the EC2 instances' security groups to allow outbound traffic to 0.0.0.0/0.
- D. Create public subnets in each Availability Zone. Associate the public subnets with the ALB. Update the route tables for the public subnets with a route to the private subnets.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* An internet-facing ALB must be placed in public subnets. The ALB then routes traffic to the EC2 instances in private subnets. The ALB's target group can contain instances in private subnets. Option A (NLB) is not necessary. Option B (move instances to public) exposes them. Option C (IGW route for private subnets) would make them public. (AWS Docs: ALB in Public Subnets, Targets in Private Subnets).
</details>

---

**Q247.** A company has deployed a database in Amazon RDS for MySQL. Due to increased transactions, the database support team is reporting slow reads against the DB instance and recommends adding a read replica. Which combination of actions should a solutions architect take before implementing this change? (Choose two.)

- A. Enable binlog replication on the RDS primary node.
- B. Choose a failover priority for the source DB instance.
- C. Allow long-running transactions to complete on the source DB instance.
- D. Create a global table and specify the AWS Regions where the table will be available.
- E. Enable automatic backups on the source instance by setting the backup retention period to a value other than 0.

<details>
<summary>Show Answer</summary>

**Answer: C, E**

*Explanation:* Before creating a read replica, you should allow long-running transactions to complete (C) to avoid replication lag. Automatic backups must be enabled (E) (retention > 0) because RDS uses backup snapshots to seed the replica. Binlog replication (A) is automatically enabled for read replicas. Failover priority (B) is for Multi-AZ. Global tables (D) are for DynamoDB. (AWS Docs: Creating RDS Read Replicas).
</details>

---

**Q248.** A company runs analytics software on Amazon EC2 instances. The software accepts job requests from users to process data that has been uploaded to Amazon S3. Users report that some submitted data is not being processed. Amazon CloudWatch reveals that the EC2 instances have a consistent CPU utilization at or near 100%. The company wants to improve system performance and scale the system based on user load. What should a solutions architect do to meet these requirements?

- A. Create a copy of the instance. Place all instances behind an Application Load Balancer.
- B. Create an S3 VPC endpoint for Amazon S3. Update the software to reference the endpoint.
- C. Stop the EC2 instances. Modify the instance type to one with a more powerful CPU and more memory. Restart the instances.
- D. Route incoming requests to Amazon Simple Queue Service (Amazon SQS). Configure an EC2 Auto Scaling group based on queue size. Update the software to read from the queue.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* The current architecture is likely direct requests to instances, causing overload. Using SQS decouples requests, and scaling workers based on queue depth allows the system to handle variable load. Option A (copy+ALB) doesn't address scaling trigger. Option B (S3 endpoint) doesn't help compute. Option C (vertical scaling) is not elastic. (AWS Docs: SQS Queue-Based Scaling).
</details>

---

**Q249.** A company is implementing a shared storage solution for a media application that is hosted in the AWS Cloud. The company needs the ability to use SMB clients to access data. The solution must be fully managed. Which AWS solution meets these requirements?

- A. Create an AWS Storage Gateway volume gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.
- B. Create an AWS Storage Gateway tape gateway. Configure tapes to use Amazon S3. Connect the application server to the tape gateway.
- C. Create an Amazon EC2 Windows instance. Install and configure a Windows file share role on the instance. Connect the application server to the file share.
- D. Create an Amazon FSx for Windows File Server file system. Attach the file system to the origin server. Connect the application server to the file system.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Amazon FSx for Windows File Server is a fully managed Windows file system that supports SMB protocol. Option A (Volume Gateway) is block storage, not file share. Option B (Tape Gateway) is for backup. Option C (EC2) is not fully managed. (AWS Docs: FSx for Windows File Server).
</details>

---

**Q250.** A company's security team requests that network traffic be captured in VPC Flow Logs. The logs will be frequently accessed for 90 days and then accessed intermittently. What should a solutions architect do to meet these requirements when configuring the logs?

- A. Use Amazon CloudWatch as the target. Set the CloudWatch log group with an expiration of 90 days.
- B. Use Amazon Kinesis as the target. Configure the Kinesis stream to always retain the logs for 90 days.
- C. Use AWS CloudTrail as the target. Configure CloudTrail to save to an Amazon S3 bucket, and enable S3 Intelligent-Tiering.
- D. Use Amazon S3 as the target. Enable an S3 Lifecycle policy to transition the logs to S3 Standard-Infrequent Access (S3 Standard-IA) after 90 days.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* VPC Flow Logs can be delivered to S3. For frequent access first 90 days (S3 Standard), then transition to S3 Standard-IA for infrequent access after 90 days. CloudWatch Logs (A) is more expensive for high-volume flow logs. Kinesis (B) is overkill. CloudTrail (C) is for API logs. (AWS Docs: VPC Flow Logs to S3 with Lifecycle).
</details>

---

**Q251.** An Amazon EC2 instance is located in a private subnet in a new VPC. This subnet does not have outbound internet access, but the EC2 instance needs the ability to download monthly security updates from an outside vendor. What should a solutions architect do to meet these requirements?

- A. Create an internet gateway, and attach it to the VPC. Configure the private subnet route table to use the internet gateway as the default route.
- B. Create a NAT gateway, and place it in a public subnet. Configure the private subnet route table to use the NAT gateway as the default route.
- C. Create a NAT instance, and place it in the same subnet where the EC2 instance is located. Configure the private subnet route table to use the NAT instance as the default route.
- D. Create an internet gateway, and attach it to the VPC. Create a NAT instance, and place it in the same subnet where the EC2 instance is located. Configure the private subnet route table to use the internet gateway as the default route.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A NAT gateway in a public subnet allows instances in a private subnet to initiate outbound internet traffic (for updates) while preventing inbound connections. Option A (IGW directly) would make the subnet public. Option C (NAT instance in private subnet) won't work (needs public subnet). Option D incorrectly routes private subnet to IGW. (AWS Docs: NAT Gateways).
</details>

---

**Q252.** A solutions architect needs to design a system to store client case files. The files are core company assets and are important. The number of files will grow over time. The files must be simultaneously accessible from multiple application servers that run on Amazon EC2 instances. The solution must have built-in redundancy. Which solution meets these requirements?

- A. Amazon Elastic File System (Amazon EFS)
- B. Amazon Elastic Block Store (Amazon EBS)
- C. Amazon S3 Glacier Deep Archive
- D. AWS Backup

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Amazon EFS is a fully managed, scalable, and highly available (multi-AZ) file system that can be mounted by multiple EC2 instances simultaneously. EBS (B) cannot be mounted by multiple instances (except Multi-Attach with limitations). Glacier (C) is for archival, not active access. AWS Backup (D) is a backup service. (AWS Docs: Amazon EFS).
</details>

---

**Q253.** A solutions architect has created two IAM policies: Policy1 and Policy2. Both policies are attached to an IAM group. Policy1: Allows `iam:Get, iam:List, kms:List, ec2:*, ds:*, logs:Get, logs:Describe` on `*`. Policy2: Denies `ds:Delete` on `*`. A cloud engineer is added as an IAM user to the IAM group. Which action will the cloud engineer be able to perform?

- A. Deleting IAM users
- B. Deleting directories
- C. Deleting Amazon EC2 instances
- D. Deleting logs from Amazon CloudWatch Logs

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Policy1 allows `ec2:*` (including `ec2:TerminateInstances`). Policy2 denies `ds:Delete` (directory service). There is no deny on EC2 termination, so the engineer can delete EC2 instances. IAM delete is not allowed (Policy1 does not include `iam:DeleteUser`). CloudWatch logs delete (`logs:DeleteLogGroup`) is not allowed. (AWS Docs: IAM Policy Evaluation).
</details>

---

**Q254.** A company is reviewing a recent migration of a three-tier application to a VPC. The security team discovers that the principle of least privilege is not being applied to Amazon EC2 security group ingress and egress rules between the application tiers. What should a solutions architect do to correct this issue?

- A. Create security group rules using the instance ID as the source or destination.
- B. Create security group rules using the security group ID as the source or destination.
- C. Create security group rules using the VPC CIDR blocks as the source or destination.
- D. Create security group rules using the subnet CIDR blocks as the source or destination.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Security group rules referencing other security group IDs (instead of CIDR blocks) allow you to control traffic between tiers based on the security group assignment, which automatically updates when instances scale. This is a least-privilege best practice. Instance IDs (A) cannot be used directly. VPC/Subnet CIDRs (C, D) are broader than necessary. (AWS Docs: Security Group Rules Referencing).
</details>

---

**Q255.** A company has an ecommerce checkout workflow that writes an order to a database and calls a service to process the payment. Users are experiencing timeouts during the checkout process. When users resubmit the checkout form, multiple unique orders are created for the same desired transaction. How should a solutions architect refactor this workflow to prevent the creation of multiple orders?

- A. Configure the web application to send an order message to Amazon Kinesis Data Firehose. Set the payment service to retrieve the message from Kinesis Data Firehose and process the order.
- B. Create a rule in AWS CloudTrail to invoke an AWS Lambda function based on the logged application path request. Use Lambda to query the database, call the payment service, and pass in the order information.
- C. Store the order in the database. Send a message that includes the order number to Amazon Simple Notification Service (Amazon SNS). Set the payment service to poll Amazon SNS, retrieve the message, and process the order.
- D. Store the order in the database. Send a message that includes the order number to an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Set the payment service to retrieve the message and process the order. Delete the message from the queue.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Using an SQS FIFO queue with a deduplication ID based on the order ID ensures exactly-once processing, preventing duplicate orders. The workflow: store order -> send message to FIFO queue -> payment service processes and deletes. Option A (Firehose) is for analytics. Option B (CloudTrail) is not for this. Option C (SNS) does not guarantee order/duplicates. (AWS Docs: SQS FIFO Exactly-Once).
</details>

---

**Q256.** A solutions architect is implementing a document review application using an Amazon S3 bucket for storage. The solution must prevent accidental deletion of the documents and ensure that all versions of the documents are available. Users must be able to download, modify, and upload documents. Which combination of actions should be taken to meet these requirements? (Choose two.)

- A. Enable a read-only bucket ACL.
- B. Enable versioning on the bucket.
- C. Attach an IAM policy to the bucket.
- D. Enable MFA Delete on the bucket.
- E. Encrypt the bucket using AWS KMS.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* Versioning (B) ensures all versions are available. MFA Delete (D) prevents accidental deletion by requiring MFA to delete versions. Read-only ACL (A) would prevent modifications. IAM policy (C) is for access control, not versioning/deletion protection. Encryption (E) protects confidentiality, not deletion. (AWS Docs: S3 Versioning, MFA Delete).
</details>

---

**Q257.** A company is building a solution that will report Amazon EC2 Auto Scaling events across all the applications in an AWS account. The company needs to use a serverless solution to store the EC2 Auto Scaling status data in Amazon S3. The company then will use the data in Amazon S3 to provide near-real-time updates in a dashboard. The solution must not affect the speed of EC2 instance launches. How should the company move the data to Amazon S3 to meet these requirements?

- A. Use an Amazon CloudWatch metric stream to send the EC2 Auto Scaling status data to Amazon Kinesis Data Firehose. Store the data in Amazon S3.
- B. Launch an Amazon EMR cluster to collect the EC2 Auto Scaling status data and send the data to Amazon Kinesis Data Firehose. Store the data in Amazon S3.
- C. Create an Amazon EventBridge rule to invoke an AWS Lambda function on a schedule. Configure the Lambda function to send the EC2 Auto Scaling status data directly to Amazon S3.
- D. Use a bootstrap script during the launch of an EC2 instance to install Amazon Kinesis Agent. Configure Kinesis Agent to collect the EC2 Auto Scaling status data and send the data to Amazon Kinesis Data Firehose. Store the data in Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudWatch metric streams can send metrics (including Auto Scaling) to Kinesis Data Firehose, which delivers to S3. This is serverless and does not impact instance launch speed. Option B (EMR) is not serverless. Option C (Lambda on schedule) may miss events. Option D requires agent on instances. (AWS Docs: CloudWatch Metric Streams).
</details>

---

**Q258.** A company has an application that places hundreds of .csv files into an Amazon S3 bucket every hour. The files are 1 GB in size. Each time a file is uploaded, the company needs to convert the file to Apache Parquet format and place the output file into an S3 bucket. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an AWS Lambda function to download the .csv files, convert the files to Parquet format, and place the output files in an S3 bucket. Invoke the Lambda function for each S3 PUT event.
- B. Create an Apache Spark job to read the .csv files, convert the files to Parquet format, and place the output files in an S3 bucket. Create an AWS Lambda function for each S3 PUT event to invoke the Spark job.
- C. Create an AWS Glue table and an AWS Glue crawler for the S3 bucket where the application places the .csv files. Schedule an AWS Lambda function to periodically use Amazon Athena to query the AWS Glue table, convert the query results into Parquet format, and place the output files into an S3 bucket.
- D. Create an AWS Glue extract, transform, and load (ETL) job to convert the .csv files to Parquet format and place the output files into an S3 bucket. Create an AWS Lambda function for each S3 PUT event to invoke the ETL job.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Glue ETL jobs are designed for converting CSV to Parquet at scale. Triggering the job via Lambda on S3 events provides near-real-time conversion. Lambda alone (A) may hit timeout/memory limits for 1 GB files. Option B (Spark job) requires managing EMR. Option C (Athena) is for querying, not conversion. (AWS Docs: Glue ETL for CSV to Parquet).
</details>

---

**Q259.** A company is implementing new data retention policies for all databases that run on Amazon RDS DB instances. The company must retain daily backups for a minimum period of 2 years. The backups must be consistent and restorable. Which solution should a solutions architect recommend to meet these requirements?

- A. Create a backup vault in AWS Backup to retain RDS backups. Create a new backup plan with a daily schedule and an expiration period of 2 years after creation. Assign the RDS DB instances to the backup plan.
- B. Configure a backup window for the RDS DB instances for daily snapshots. Assign a snapshot retention policy of 2 years to each RDS DB instance. Use Amazon Data Lifecycle Manager (Amazon DLM) to schedule snapshot deletions.
- C. Configure database transaction logs to be automatically backed up to Amazon CloudWatch Logs with an expiration period of 2 years.
- D. Configure an AWS Database Migration Service (AWS DMS) replication task. Deploy a replication instance, and configure a change data capture (CDC) task to stream database changes to Amazon S3 as the target. Configure S3 Lifecycle policies to delete the snapshots after 2 years.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Backup provides centralized backup management with scheduled backups and lifecycle policies (expiration). It supports RDS and ensures consistent, restorable backups. Option B (DLM) is for EBS snapshots, not RDS. Option C (CloudWatch Logs) is not for database backups. Option D (DMS) is for migration, not backup. (AWS Docs: AWS Backup for RDS).
</details>

---

**Q260.** A company's compliance team needs to move its file shares to AWS. The shares run on a Windows Server SMB file share. A self-managed on-premises Active Directory controls access to the files and folders. The company wants to use Amazon FSx for Windows File Server as part of the solution. The company must ensure that the on-premises Active Directory groups restrict access to the FSx for Windows File Server SMB compliance shares, folders, and files after the move to AWS. The company has created an FSx for Windows File Server file system. Which solution will meet these requirements?

- A. Create an Active Directory Connector to connect to the Active Directory. Map the Active Directory groups to IAM groups to restrict access.
- B. Assign a tag with a Restrict tag key and a Compliance tag value. Map the Active Directory groups to IAM groups to restrict access.
- C. Create an IAM service-linked role that is linked directly to FSx for Windows File Server to restrict access.
- D. Join the file system to the Active Directory to restrict access.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* FSx for Windows File Server can be joined to a self-managed Active Directory (via AWS Directory Service or directly). Once joined, it uses AD groups for file/folder permissions natively. Option A (AD Connector) is for user authentication to AWS, not for FSx. Options B and C (IAM) are not used for SMB file permissions. (AWS Docs: FSx with On-Premises AD).
</details>

---

**Q261.** A company recently announced the deployment of its retail website to a global audience. The website runs on multiple Amazon EC2 instances behind an Elastic Load Balancer. The instances run in an Auto Scaling group across multiple Availability Zones. The company wants to provide its customers with different versions of content based on the devices that the customers use to access the website. Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

- A. Configure Amazon CloudFront to cache multiple versions of the content.
- B. Configure a host header in a Network Load Balancer to forward traffic to different instances.
- C. Configure a Lambda@Edge function to send specific objects to users based on the User-Agent header.
- D. Configure AWS Global Accelerator. Forward requests to a Network Load Balancer (NLB). Configure the NLB to set up host-based routing to different EC2 instances.
- E. Configure AWS Global Accelerator. Forward requests to a Network Load Balancer (NLB). Configure the NLB to set up path-based routing to different EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* CloudFront can cache multiple versions of content (e.g., device-specific) and Lambda@Edge can inspect the User-Agent header to serve the appropriate version. This is a standard pattern for device detection. NLBs (B, D, E) do not support host/path-based routing based on device headers. (AWS Docs: Lambda@Edge for Device Detection).
</details>

---

**Q262.** A company plans to use Amazon ElastiCache for its multi-tier web application. A solutions architect creates a Cache VPC for the ElastiCache cluster and an App VPC for the application's Amazon EC2 instances. Both VPCs are in the us-east-1 Region. The solutions architect must implement a solution to provide the application's EC2 instances with access to the ElastiCache cluster. Which solution will meet these requirements MOST cost-effectively?

- A. Create a peering connection between the VPCs. Add a route table entry for the peering connection in both VPCs. Configure an inbound rule for the ElastiCache cluster's security group to allow inbound connection from the application's security group.
- B. Create a Transit VPC. Update the VPC route tables in the Cache VPC and the App VPC to route traffic through the Transit VPC. Configure an inbound rule for the ElastiCache cluster's security group to allow inbound connection from the application's security group.
- C. Create a peering connection between the VPCs. Add a route table entry for the peering connection in both VPCs. Configure an inbound rule for the peering connection's security group to allow inbound connection from the application's security group.
- D. Create a Transit VPC. Update the VPC route tables in the Cache VPC and the App VPC to route traffic through the Transit VPC. Configure an inbound rule for the Transit VPC's security group to allow inbound connection from the application's security group.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* VPC peering is the most cost-effective way to connect two VPCs in the same Region. You need to add routes and allow traffic in the ElastiCache security group from the App VPC's security group. Transit VPC (B, D) or Transit Gateway is more expensive and complex for two VPCs. (AWS Docs: VPC Peering).
</details>

---

**Q263.** A company is building an application that consists of several microservices. The company has decided to use container technologies to deploy its software on AWS. The company needs a solution that minimizes the amount of ongoing effort for maintenance and scaling. The company cannot manage additional infrastructure. Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

- A. Deploy an Amazon Elastic Container Service (Amazon ECS) cluster.
- B. Deploy the Kubernetes control plane on Amazon EC2 instances that span multiple Availability Zones.
- C. Deploy an Amazon Elastic Container Service (Amazon ECS) service with an Amazon EC2 launch type. Specify a desired task number level of greater than or equal to 2.
- D. Deploy an Amazon Elastic Container Service (Amazon ECS) service with a Fargate launch type. Specify a desired task number level of greater than or equal to 2.
- E. Deploy Kubernetes worker nodes on Amazon EC2 instances that span multiple Availability Zones. Create a deployment that specifies two or more replicas for each microservice.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* ECS cluster (A) with Fargate launch type (D) is serverless, minimizing maintenance and scaling effort. Fargate eliminates EC2 management. Options B/E (Kubernetes on EC2) require managing control plane and worker nodes. Option C (ECS with EC2) still requires managing EC2 instances. (AWS Docs: ECS Fargate).
</details>

---

**Q264.** A company has a web application hosted over 10 Amazon EC2 instances with traffic directed by Amazon Route 53. The company occasionally experiences a timeout error when attempting to browse the application. The networking team finds that some DNS queries return IP addresses of unhealthy instances, resulting in the timeout error. What should a solutions architect implement to overcome these timeout errors?

- A. Create a Route 53 simple routing policy record for each EC2 instance. Associate a health check with each record.
- B. Create a Route 53 failover routing policy record for each EC2 instance. Associate a health check with each record.
- C. Create an Amazon CloudFront distribution with EC2 instances as its origin. Associate a health check with the EC2 instances.
- D. Create an Application Load Balancer (ALB) with a health check in front of the EC2 instances. Route to the ALB from Route 53.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* The best practice is to place an ALB (with health checks) in front of the EC2 instances and point Route 53 to the ALB. The ALB will only route traffic to healthy instances. Simple routing (A) with health checks can work but is limited to 8 IPs. Failover (B) is for active-passive. CloudFront (C) is for caching, not load balancing health checks. (AWS Docs: ALB with Route 53).
</details>

---

**Q265.** A solutions architect needs to design a highly available application consisting of web, application, and database tiers. HTTPS content delivery should be as close to the edge as possible, with the least delivery time. Which solution meets these requirements and is MOST secure?

- A. Configure a public Application Load Balancer (ALB) with multiple redundant Amazon EC2 instances in public subnets. Configure Amazon CloudFront to deliver HTTPS content using the public ALB as the origin.
- B. Configure a public Application Load Balancer with multiple redundant Amazon EC2 instances in private subnets. Configure Amazon CloudFront to deliver HTTPS content using the EC2 instances as the origin.
- C. Configure a public Application Load Balancer (ALB) with multiple redundant Amazon EC2 instances in private subnets. Configure Amazon CloudFront to deliver HTTPS content using the public ALB as the origin.
- D. Configure a public Application Load Balancer with multiple redundant Amazon EC2 instances in public subnets. Configure Amazon CloudFront to deliver HTTPS content using the EC2 instances as the origin.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* For security, EC2 instances should be in private subnets (not public). ALB must be in public subnets to receive internet traffic. CloudFront uses the ALB as origin (not directly to EC2). This provides edge caching and HTTPS termination at CloudFront. Option B uses EC2 as origin (bypasses ALB). (AWS Docs: CloudFront with ALB Origin, Private Subnets).
</details>

---

**Q266.** A company has a popular gaming platform running on AWS. The application is sensitive to latency because latency can impact the user experience and introduce unfair advantages to some players. The application is deployed in every AWS Region. It runs on Amazon EC2 instances that are part of Auto Scaling groups configured behind Application Load Balancers (ALBs). A solutions architect needs to implement a mechanism to monitor the health of the application and redirect traffic to healthy endpoints. Which solution meets these requirements?

- A. Configure an accelerator in AWS Global Accelerator. Add a listener for the port that the application listens on, and attach it to a Regional endpoint in each Region. Add the ALB as the endpoint.
- B. Create an Amazon CloudFront distribution and specify the ALB as the origin server. Configure the cache behavior to use origin cache headers. Use AWS Lambda functions to optimize the traffic.
- C. Create an Amazon CloudFront distribution and specify Amazon S3 as the origin server. Configure the cache behavior to use origin cache headers. Use AWS Lambda functions to optimize the traffic.
- D. Configure an Amazon DynamoDB database to serve as the data store for the application. Create a DynamoDB Accelerator (DAX) cluster to act as the in-memory cache for DynamoDB hosting the application data.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Global Accelerator provides low-latency routing, health checks, and automatic failover across multiple Regions. It can use ALBs as endpoints. CloudFront (B, C) is for caching, not low-latency TCP/UDP. Option D is database caching, not traffic routing. (AWS Docs: Global Accelerator Health Checks).
</details>

---

**Q267.** A company has one million users that use its mobile app. The company must analyze the data usage in near-real time. The company also must encrypt the data in near-real time and must store the data in a centralized location in Apache Parquet format for further processing. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an Amazon Kinesis data stream to store the data in Amazon S3. Create an Amazon Kinesis Data Analytics application to analyze the data. Invoke an AWS Lambda function to send the data to the Kinesis Data Analytics application.
- B. Create an Amazon Kinesis data stream to store the data in Amazon S3. Create an Amazon EMR cluster to analyze the data. Invoke an AWS Lambda function to send the data to the EMR cluster.
- C. Create an Amazon Kinesis Data Firehose delivery stream to store the data in Amazon S3. Create an Amazon EMR cluster to analyze the data.
- D. Create an Amazon Kinesis Data Firehose delivery stream to store the data in Amazon S3. Create an Amazon Kinesis Data Analytics application to analyze the data.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Kinesis Data Firehose can encrypt data, convert to Parquet, and deliver to S3. Kinesis Data Analytics can analyze the stream in near-real time. This is fully managed (serverless) with minimal overhead. Option A/B use Kinesis Data Streams (more management) and custom Lambda. Option C uses EMR (more overhead). (AWS Docs: Kinesis Data Firehose Parquet Conversion, KDA).
</details>

---

**Q268.** A gaming company has a web application that displays scores. The application runs on Amazon EC2 instances behind an Application Load Balancer. The application stores data in an Amazon RDS for MySQL database. Users are starting to experience long delays and interruptions that are caused by database read performance. The company wants to improve the user experience while minimizing changes to the application's architecture. What should a solutions architect do to meet these requirements?

- A. Use Amazon ElastiCache in front of the database.
- B. Use RDS Proxy between the application and the database.
- C. Migrate the application from EC2 instances to AWS Lambda.
- D. Migrate the database from Amazon RDS for MySQL to Amazon DynamoDB.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Adding ElastiCache as a caching layer in front of the database reduces read load and latency with minimal application changes (just cache check logic). RDS Proxy (B) helps with connection management, not read performance. Lambda (C) and DynamoDB (D) are major architectural changes. (AWS Docs: ElastiCache for Database Caching).
</details>

---

**Q269.** An ecommerce company has noticed performance degradation of its Amazon RDS based web application. The performance degradation is attributed to an increase in the number of read-only SQL queries triggered by business analysts. A solutions architect needs to solve the problem with minimal changes to the existing web application. What should the solutions architect recommend?

- A. Export the data to Amazon DynamoDB and have the business analysts run their queries.
- B. Load the data into Amazon ElastiCache and have the business analysts run their queries.
- C. Create a read replica of the primary database and have the business analysts run their queries.
- D. Copy the data into an Amazon Redshift cluster and have the business analysts run their queries.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Creating a read replica offloads read-only queries from the primary database with minimal changes (just point analysts to the replica endpoint). Option B (ElastiCache) requires application changes to load data. Option A/D require data migration. (AWS Docs: RDS Read Replicas).
</details>

---

**Q270.** A company is using a centralized AWS account to store log data in various Amazon S3 buckets. A solutions architect needs to ensure that the data is encrypted at rest before the data is uploaded to the S3 buckets. The data also must be encrypted in transit. Which solution meets these requirements?

- A. Use client-side encryption to encrypt the data that is being uploaded to the S3 buckets.
- B. Use server-side encryption to encrypt the data that is being uploaded to the S3 buckets.
- C. Create bucket policies that require the use of server-side encryption with S3 managed encryption keys (SSE-S3) for S3 uploads.
- D. Enable the security option to encrypt the S3 buckets through the use of a default AWS Key Management Service (AWS KMS) key.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Client-side encryption ensures data is encrypted before it leaves the client, satisfying "encrypted at rest before upload" and "encrypted in transit" (if using HTTPS). Server-side encryption (B, C, D) encrypts data at rest after it reaches S3, not before upload. (AWS Docs: Client-Side Encryption).
</details>

---

**Q271.** A solutions architect observes that a nightly batch processing job is automatically scaled up for 1 hour before the desired Amazon EC2 capacity is reached. The peak capacity is the same every night and the batch jobs always start at 1 AM. The solutions architect needs to find a cost-effective solution that will allow for the desired EC2 capacity to be reached quickly and allow the Auto Scaling group to scale down after the batch jobs are complete. What should the solutions architect do to meet these requirements?

- A. Increase the minimum capacity for the Auto Scaling group.
- B. Increase the maximum capacity for the Auto Scaling group.
- C. Configure scheduled scaling to scale up to the desired compute level.
- D. Change the scaling policy to add more EC2 instances during each scaling operation.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Since the peak is predictable (same time every night), scheduled scaling can pre-provision the desired capacity exactly at 1 AM, avoiding the 1-hour ramp-up time. This is cost-effective because it scales down after. Options A/B would keep instances running longer. Option D doesn't address the ramp-up delay. (AWS Docs: Scheduled Scaling for Auto Scaling).
</details>

---

**Q272.** A company serves a dynamic website from a fleet of Amazon EC2 instances behind an Application Load Balancer (ALB). The website needs to support multiple languages to serve customers around the world. The website's architecture is running in the us-west-1 Region and is exhibiting high request latency for users that are located in other parts of the world. The website needs to serve requests quickly and efficiently regardless of a user's location. However, the company does not want to recreate the existing architecture across multiple Regions. What should a solutions architect do to meet these requirements?

- A. Replace the existing architecture with a website that is served from an Amazon S3 bucket. Configure an Amazon CloudFront distribution with the S3 bucket as the origin. Set the cache behavior settings to cache based on the Accept-Language request header.
- B. Configure an Amazon CloudFront distribution with the ALB as the origin. Set the cache behavior settings to cache based on the Accept-Language request header.
- C. Create an Amazon API Gateway API that is integrated with the ALB. Configure the API to use the HTTP integration type. Set up an API Gateway stage to enable the API cache based on the Accept-Language request header.
- D. Launch an EC2 instance in each additional Region and configure NGINX to act as a cache server for that Region. Put all the EC2 instances and the ALB behind an Amazon Route 53 record set with a geolocation routing policy.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* CloudFront can cache both static and dynamic content based on headers like `Accept-Language`. Using the ALB as origin allows the existing architecture to remain in us-west-1 while CloudFront serves content globally with low latency. Option A (S3) is for static sites. Option C (API Gateway) adds complexity. Option D requires managing cache servers. (AWS Docs: CloudFront Dynamic Content Caching).
</details>

---

**Q273.** A rapidly growing ecommerce company is running its workloads in a single AWS Region. A solutions architect must create a disaster recovery (DR) strategy that includes a different AWS Region. The company wants its database to be up to date in the DR Region with the least possible latency. The remaining infrastructure in the DR Region needs to run at reduced capacity and must be able to scale up if necessary. Which solution will meet these requirements with the LOWEST recovery time objective (RTO)?

- A. Use an Amazon Aurora global database with a pilot light deployment.
- B. Use an Amazon Aurora global database with a warm standby deployment.
- C. Use an Amazon RDS Multi-AZ DB instance with a pilot light deployment.
- D. Use an Amazon RDS Multi-AZ DB instance with a warm standby deployment.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Aurora Global Database provides low-latency cross-Region replication (typically <1 second). A warm standby deployment has a fully functional but scaled-down environment in the DR Region, allowing very fast failover (RTO minutes). Pilot light (A) has higher RTO. RDS Multi-AZ (C, D) is within a Region, not cross-Region. (AWS Docs: Aurora Global Database Warm Standby).
</details>

---

**Q274.** A company runs an application on Amazon EC2 instances. The company needs to implement a disaster recovery (DR) solution for the application. The DR solution needs to have a recovery time objective (RTO) of less than 4 hours. The DR solution also needs to use the fewest possible AWS resources during normal operations. Which solution will meet these requirements in the MOST operationally efficient way?

- A. Create Amazon Machine Images (AMIs) to back up the EC2 instances. Copy the AMIs to a secondary AWS Region. Automate infrastructure deployment in the secondary Region by using AWS Lambda and custom scripts.
- B. Create Amazon Machine Images (AMIs) to back up the EC2 instances. Copy the AMIs to a secondary AWS Region. Automate infrastructure deployment in the secondary Region by using AWS CloudFormation.
- C. Launch EC2 instances in a secondary AWS Region. Keep the EC2 instances in the secondary Region active at all times.
- D. Launch EC2 instances in a secondary Availability Zone. Keep the EC2 instances in the secondary Availability Zone active at all times.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Using AMI backups copied to another Region, with CloudFormation for automated deployment, provides a cost-effective DR solution with RTO <4 hours. It uses few resources during normal operations (only storage for AMIs). Option C keeps instances running (costly). Option D is not cross-Region. Option A (Lambda + custom scripts) is less efficient than CloudFormation. (AWS Docs: Disaster Recovery with CloudFormation).
</details>

---

**Q275.** A company runs an internal browser-based application. The application runs on Amazon EC2 instances behind an Application Load Balancer. The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. The Auto Scaling group scales up to 20 instances during work hours, but scales down to 2 instances overnight. Staff are complaining that the application is very slow when the day begins, although it runs well by mid-morning. How should the scaling be changed to address the staff complaints and keep costs to a minimum?

- A. Implement a scheduled action that sets the desired capacity to 20 shortly before the office opens.
- B. Implement a step scaling action triggered at a lower CPU threshold, and decrease the cooldown period.
- C. Implement a target tracking action triggered at a lower CPU threshold, and decrease the cooldown period.
- D. Implement a scheduled action that sets the minimum and maximum capacity to 20 shortly before the office opens.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* The slow start is due to the scaling policy needing time to react to increased load. A scheduled action to increase desired capacity to 20 shortly before office opens pre-provisions instances, eliminating the ramp-up delay. Option D (min/max=20) prevents scaling down after hours, increasing cost. (AWS Docs: Scheduled Scaling for Auto Scaling).
</details>

---

**Q276.** A company has a multi-tier application deployed on several Amazon EC2 instances in an Auto Scaling group. An Amazon RDS for Oracle instance is the application's data layer that uses Oracle-specific PL/SQL functions. Traffic to the application has been steadily increasing. This is causing the EC2 instances to become overloaded and the RDS instance to run out of storage. The Auto Scaling group does not have any scaling metrics and defines the minimum healthy instance count only. The company predicts that traffic will continue to increase at a steady but unpredictable rate before leveling off. What should a solutions architect do to ensure the system can automatically scale for the increased traffic? (Choose two.)

- A. Configure storage Auto Scaling on the RDS for Oracle instance.
- B. Migrate the database to Amazon Aurora to use Auto Scaling storage.
- C. Configure an alarm on the RDS for Oracle instance for low free storage space.
- D. Configure the Auto Scaling group to use the average CPU as the scaling metric.
- E. Configure the Auto Scaling group to use the average free memory as the scaling metric.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* For database storage scaling, Aurora Auto Scaling storage is a better fit than RDS for Oracle (which does not auto-scale storage). For EC2 scaling, average CPU is a good metric for a multi-tier app. Option A (RDS for Oracle) does not have storage auto-scaling. Option E (free memory) is less reliable. (AWS Docs: Aurora Storage Auto Scaling, Auto Scaling CPU Metrics).
</details>

---

**Q277.** A company provides an online service for posting video content and transcoding it for use by any mobile platform. The application architecture uses Amazon Elastic File System (Amazon EFS) Standard to collect and store the videos so that multiple Amazon EC2 Linux instances can access the video content for processing. As the popularity of the service has grown over time, the storage costs have become too expensive. Which storage solution is MOST cost-effective?

- A. Use AWS Storage Gateway for files to store and process the video content.
- B. Use AWS Storage Gateway for volumes to store and process the video content.
- C. Use Amazon EFS for storing the video content. Once processing is complete, transfer the files to Amazon Elastic Block Store (Amazon EBS).
- D. Use Amazon S3 for storing the video content. Move the files temporarily over to an Amazon Elastic Block Store (Amazon EBS) volume attached to the server for processing.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 is the most cost-effective storage for video files. For processing, you can temporarily copy the file to an EBS volume attached to an EC2 instance, process it, and then write the result back to S3. This avoids the high cost of storing all videos in EFS. Option A/B (Storage Gateway) are for hybrid. Option C (EFS) is still expensive. (AWS Docs: S3 for Media Storage).
</details>

---

**Q278.** A company wants to create an application to store employee data in a hierarchical structured relationship. The company needs a minimum-latency response to high-traffic queries for the employee data and must protect any sensitive data. The company also needs to receive monthly email messages if any financial information is present in the employee data. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- A. Use Amazon Redshift to store the employee data in hierarchies. Unload the data to Amazon S3 every month.
- B. Use Amazon DynamoDB to store the employee data in hierarchies. Export the data to Amazon S3 every month.
- C. Configure Amazon Macie for the AWS account. Integrate Macie with Amazon EventBridge to send monthly events to AWS Lambda.
- D. Use Amazon Athena to analyze the employee data in Amazon S3. Integrate Athena with Amazon QuickSight to publish analysis dashboards and share the dashboards with users.
- E. Configure Amazon Macie for the AWS account. Integrate Macie with Amazon EventBridge to send monthly notifications through an Amazon Simple Notification Service (Amazon SNS) subscription.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* DynamoDB supports hierarchical data and low-latency queries. Export to S3 monthly for analysis. Macie (E) can detect sensitive data (financial information) and send alerts via EventBridge and SNS. Option C (Macie to Lambda) is possible, but E is more direct for monthly email. Redshift (A) is for analytics, not low-latency. (AWS Docs: DynamoDB Hierarchical Data, Macie Alerts).
</details>

---

**Q279.** A company has an application that is backed by an Amazon DynamoDB table. The company's compliance requirements specify that database backups must be taken every month, must be available for 6 months, and must be retained for 7 years. Which solution will meet these requirements?

- A. Create an AWS Backup plan to back up the DynamoDB table on the first day of each month. Specify a lifecycle policy that transitions the backup to cold storage after 6 months. Set the retention period for each backup to 7 years.
- B. Create a DynamoDB on-demand backup of the DynamoDB table on the first day of each month. Transition the backup to Amazon S3 Glacier Flexible Retrieval after 6 months. Create an S3 Lifecycle policy to delete backups that are older than 7 years.
- C. Use the AWS SDK to develop a script that creates an on-demand backup of the DynamoDB table. Set up an Amazon EventBridge rule that runs the script on the first day of each month. Create a second script that will run on the second day of each month to transition DynamoDB backups that are older than 6 months to cold storage and to delete backups that are older than 7 years.
- D. Use the AWS CLI to create an on-demand backup of the DynamoDB table. Set up an Amazon EventBridge rule that runs the command on the first day of each month with a cron expression. Specify in the command to transition the backups to cold storage after 6 months and to delete the backups after 7 years.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Backup provides native support for DynamoDB, including scheduled backups, lifecycle policies (transition to cold storage), and retention settings. This is the most operationally efficient. Options B, C, D require manual steps or custom scripts. (AWS Docs: AWS Backup for DynamoDB).
</details>

---

**Q280.** A company is using Amazon CloudFront with its website. The company has enabled logging on the CloudFront distribution, and logs are saved in one of the company's Amazon S3 buckets. The company needs to perform advanced analyses on the logs and build visualizations. What should a solutions architect do to meet these requirements?

- A. Use standard SQL queries in Amazon Athena to analyze the CloudFront logs in the S3 bucket. Visualize the results with AWS Glue.
- B. Use standard SQL queries in Amazon Athena to analyze the CloudFront logs in the S3 bucket. Visualize the results with Amazon QuickSight.
- C. Use standard SQL queries in Amazon DynamoDB to analyze the CloudFront logs in the S3 bucket. Visualize the results with AWS Glue.
- D. Use standard SQL queries in Amazon DynamoDB to analyze the CloudFront logs in the S3 bucket. Visualize the results with Amazon QuickSight.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Athena is serverless and can run SQL queries directly on CloudFront logs stored in S3. QuickSight is the BI visualization service. Glue (A, C) is for ETL, not visualization. DynamoDB (C, D) is not suitable for ad-hoc SQL analytics on logs. (AWS Docs: Athena for CloudFront Logs, QuickSight).
</details>

---

**Q281.** A company runs a fleet of web servers using an Amazon RDS for PostgreSQL DB instance. After a routine compliance check, the company sets a standard that requires a recovery point objective (RPO) of less than 1 second for all its production databases. Which solution meets these requirements?

- A. Enable a Multi-AZ deployment for the DB instance.
- B. Enable auto scaling for the DB instance in one Availability Zone.
- C. Configure the DB instance in one Availability Zone, and create multiple read replicas in a separate Availability Zone.
- D. Configure the DB instance in one Availability Zone, and configure AWS Database Migration Service (AWS DMS) change data capture (CDC) tasks.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Multi-AZ deployment provides synchronous replication to a standby instance in another AZ, ensuring zero data loss (RPO=0) and failover. RPO <1 second is achieved. Auto scaling (B) doesn't affect RPO. Read replicas (C) are asynchronous (potential data loss). DMS CDC (D) is not for RPO <1 sec. (AWS Docs: RDS Multi-AZ RPO).
</details>

---

**Q282.** A company runs a web application that is deployed on Amazon EC2 instances in the private subnet of a VPC. An Application Load Balancer (ALB) that extends across the public subnets directs web traffic to the EC2 instances. The company wants to implement new security measures to restrict inbound traffic from the ALB to the EC2 instances while preventing access from any other source inside or outside the private subnet of the EC2 instances. Which solution will meet these requirements?

- A. Configure a route in a route table to direct traffic from the internet to the private IP addresses of the EC2 instances.
- B. Configure the security group for the EC2 instances to only allow traffic that comes from the security group for the ALB.
- C. Move the EC2 instances into the public subnet. Give the EC2 instances a set of Elastic IP addresses.
- D. Configure the security group for the ALB to allow any TCP traffic on any port.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Security groups can reference other security groups as sources. By allowing inbound traffic to the EC2 instances only from the ALB's security group, you restrict access to only the ALB, regardless of IP addresses. Option A (route table) is not for access control. Option C (public subnet) reduces security. Option D (ALB security group) is too permissive. (AWS Docs: Security Group Referencing).
</details>

---

**Q283.** A research company runs experiments that are powered by a simulation application and a visualization application. The simulation application runs on Linux and outputs intermediate data to an NFS share every 5 minutes. The visualization application is a Windows desktop application that displays the simulation output and requires an SMB file system. The company maintains two synchronized file systems. This strategy is causing data duplication and inefficient resource usage. The company needs to migrate the applications to AWS without making code changes to either application. Which solution will meet these requirements?

- A. Migrate both applications to AWS Lambda. Create an Amazon S3 bucket to exchange data between the applications.
- B. Migrate both applications to Amazon Elastic Container Service (Amazon ECS). Configure Amazon FSx File Gateway for storage.
- C. Migrate the simulation application to Linux Amazon EC2 instances. Migrate the visualization application to Windows EC2 instances. Configure Amazon Simple Queue Service (Amazon SQS) to exchange data between the applications.
- D. Migrate the simulation application to Linux Amazon EC2 instances. Migrate the visualization application to Windows EC2 instances. Configure Amazon FSx for NetApp ONTAP for storage.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* FSx for NetApp ONTAP supports both NFS (for Linux) and SMB (for Windows) simultaneously, allowing a single shared file system without code changes. This eliminates data duplication. Option A (Lambda) requires code changes. Option B (FSx File Gateway) is for SMB only. Option C (SQS) is for messaging, not shared files. (AWS Docs: FSx for ONTAP Multi-Protocol).
</details>

---

**Q284.** As part of budget planning, management wants a report of AWS billed items listed by user. The data will be used to create department budgets. A solutions architect needs to determine the most efficient way to obtain this report information. Which solution meets these requirements?

- A. Run a query with Amazon Athena to generate the report.
- B. Create a report in Cost Explorer and download the report.
- C. Access the bill details from the billing dashboard and download the bill.
- D. Modify a cost budget in AWS Budgets to alert with Amazon Simple Email Service (Amazon SES).

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Cost Explorer allows you to create custom reports (including by user or usage type) and download them as CSV. This is the most efficient for budget planning. Athena (A) would require setting up CUR. Billing dashboard (C) provides aggregate bill, not by user. Budgets (D) are for alerts, not reports. (AWS Docs: Cost Explorer Reports).
</details>

---

**Q285.** A company hosts its static website by using Amazon S3. The company wants to add a contact form to its webpage. The contact form will have dynamic server-side components for users to input their name, email address, phone number, and user message. The company anticipates that there will be fewer than 100 site visits each month. Which solution will meet these requirements MOST cost-effectively?

- A. Host a dynamic contact form page in Amazon Elastic Container Service (Amazon ECS). Set up Amazon Simple Email Service (Amazon SES) to connect to any third-party email provider.
- B. Create an Amazon API Gateway endpoint with an AWS Lambda backend that makes a call to Amazon Simple Email Service (Amazon SES).
- C. Convert the static webpage to dynamic by deploying Amazon Lightsail. Use client-side scripting to build the contact form. Integrate the form with Amazon WorkMail.
- D. Create a t2.micro Amazon EC2 instance. Deploy a LAMP (Linux, Apache, MySQL, PHP/Perl/Python) stack to host the webpage. Use client-side scripting to build the contact form. Integrate the form with Amazon WorkMail.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A serverless solution (API Gateway + Lambda + SES) is the most cost-effective for low traffic (<100 visits/month). You pay only for invocations. ECS (A) and EC2 (D) have fixed costs. Lightsail (C) has a monthly fee. (AWS Docs: API Gateway + Lambda + SES for Contact Form).
</details>

---

**Q286.** A company has a static website that is hosted on Amazon CloudFront in front of Amazon S3. The static website uses a database backend. The company notices that the website does not reflect updates that have been made in the website's Git repository. The company checks the continuous integration and continuous delivery (CI/CD) pipeline between the Git repository and Amazon S3. The company verifies that the webhooks are configured properly and that the CI/CD pipeline is sending messages that indicate successful deployments. A solutions architect needs to implement a solution that displays the updates on the website. Which solution will meet these requirements?

- A. Add an Application Load Balancer.
- B. Add Amazon ElastiCache for Redis or Memcached to the database layer of the web application.
- C. Invalidate the CloudFront cache.
- D. Use AWS Certificate Manager (ACM) to validate the website's SSL certificate.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* CloudFront caches content at edge locations. When new content is deployed to S3, CloudFront may still serve old cached versions until the cache expires or is invalidated. Creating an invalidation for the updated paths will force CloudFront to fetch the new content. (AWS Docs: CloudFront Cache Invalidation).
</details>

---

**Q287.** A company wants to migrate a Windows-based application from on premises to the AWS Cloud. The application has three tiers: an application tier, a business tier, and a database tier with Microsoft SQL Server. The company wants to use specific features of SQL Server such as native backups and Data Quality Services. The company also needs to share files for processing between the tiers. How should a solutions architect design the architecture to meet these requirements?

- A. Host all three tiers on Amazon EC2 instances. Use Amazon FSx File Gateway for file sharing between the tiers.
- B. Host all three tiers on Amazon EC2 instances. Use Amazon FSx for Windows File Server for file sharing between the tiers.
- C. Host the application tier and the business tier on Amazon EC2 instances. Host the database tier on Amazon RDS. Use Amazon Elastic File System (Amazon EFS) for file sharing between the tiers.
- D. Host the application tier and the business tier on Amazon EC2 instances. Host the database tier on Amazon RDS. Use a Provisioned IOPS SSD (io2) Amazon Elastic Block Store (Amazon EBS) volume for file sharing between the tiers.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To use specific SQL Server features (native backups, DQS), you need EC2 (not RDS) for the database tier. FSx for Windows File Server provides a fully managed Windows file share (SMB) for sharing files between tiers. Option A (FSx File Gateway) is a hybrid solution. Option C (RDS) does not support all SQL Server features. Option D (EBS) cannot be shared across instances. (AWS Docs: SQL Server on EC2, FSx for Windows).
</details>

---

**Q288.** A company is migrating a Linux-based web server group to AWS. The web servers must access files in a shared file store for some content. The company must not make any changes to the application. What should a solutions architect do to meet these requirements?

- A. Create an Amazon S3 Standard bucket with access to the web servers.
- B. Configure an Amazon CloudFront distribution with an Amazon S3 bucket as the origin.
- C. Create an Amazon Elastic File System (Amazon EFS) file system. Mount the EFS file system on all web servers.
- D. Configure a General Purpose SSD (gp3) Amazon Elastic Block Store (Amazon EBS) volume. Mount the EBS volume to all web servers.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* EFS provides a shared, POSIX-compliant file system that can be mounted on multiple Linux EC2 instances without application changes. S3 (A) is not a file system. CloudFront (B) is a CDN. EBS (D) cannot be mounted on multiple instances (except Multi-Attach with limitations). (AWS Docs: Amazon EFS).
</details>

---

**Q289.** A company has an AWS Lambda function that needs read access to an Amazon S3 bucket that is located in the same AWS account. Which solution will meet these requirements in the MOST secure manner?

- A. Apply an S3 bucket policy that grants read access to the S3 bucket.
- B. Apply an IAM role to the Lambda function. Apply an IAM policy to the role to grant read access to the S3 bucket.
- C. Embed an access key and a secret key in the Lambda function's code to grant the required IAM permissions for read access to the S3 bucket.
- D. Apply an IAM role to the Lambda function. Apply an IAM policy to the role to grant read access to all S3 buckets in the account.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* The best practice is to assign an IAM role to the Lambda function (execution role) with a policy granting read access specifically to the required S3 bucket. Option A (bucket policy) could work but is less direct. Option C (embed keys) is insecure. Option D (all buckets) violates least privilege. (AWS Docs: Lambda Execution Role).
</details>

---

**Q290.** A company hosts a web application on multiple Amazon EC2 instances. The EC2 instances are in an Auto Scaling group that scales in response to user demand. The company wants to optimize cost savings without making a long-term commitment. Which EC2 instance purchasing option should a solutions architect recommend to meet these requirements?

- A. Dedicated Instances only
- B. On-Demand Instances only
- C. A mix of On-Demand Instances and Spot Instances
- D. A mix of On-Demand Instances and Reserved Instances

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* To optimize cost without long-term commitment, use a mix of On-Demand (for baseline) and Spot Instances (for fault-tolerant, scalable workloads). Spot offers significant discounts but can be interrupted. Reserved Instances (D) require a 1-3 year commitment. Dedicated (A) is more expensive. (AWS Docs: Spot Instances for Cost Optimization).
</details>

---

**Q291.** A media company uses Amazon CloudFront for its publicly available streaming video content. The company wants to secure the video content that is hosted in Amazon S3 by controlling who has access. Some of the company's users are using a custom HTTP client that does not support cookies. Some of the company's users are unable to change the hardcoded URLs that they are using for access. Which services or methods will meet these requirements with the LEAST impact to the users? (Choose two.)

- A. Signed cookies
- B. Signed URLs
- C. AWS AppSync
- D. JSON Web Token (JWT)
- E. AWS Secrets Manager

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* CloudFront signed URLs and signed cookies are used to restrict access to content. Signed URLs (B) work well for users who cannot change hardcoded URLs (just replace the URL). Signed cookies (A) work for users whose clients support cookies. Option C (AppSync) is for GraphQL. Option D (JWT) is not native to CloudFront. Option E (Secrets Manager) is for secrets. (AWS Docs: CloudFront Signed URLs/Cookies).
</details>

---

**Q292.** A company is preparing a new data platform that will ingest real-time streaming data from multiple sources. The company needs to transform the data before writing the data to Amazon S3. The company needs the ability to use SQL to query the transformed data. Which solutions will meet these requirements? (Choose two.)

- A. Use Amazon Kinesis Data Streams to stream the data. Use Amazon Kinesis Data Analytics to transform the data. Use Amazon Kinesis Data Firehose to write the data to Amazon S3. Use Amazon Athena to query the transformed data from Amazon S3.
- B. Use Amazon Managed Streaming for Apache Kafka (Amazon MSK) to stream the data. Use AWS Glue to transform the data and to write the data to Amazon S3. Use Amazon Athena to query the transformed data from Amazon S3.
- C. Use AWS Database Migration Service (AWS DMS) to ingest the data. Use Amazon EMR to transform the data and to write the data to Amazon S3. Use Amazon Athena to query the transformed data from Amazon S3.
- D. Use Amazon Managed Streaming for Apache Kafka (Amazon MSK) to stream the data. Use Amazon Kinesis Data Analytics to transform the data and to write the data to Amazon S3. Use the Amazon RDS query editor to query the transformed data from Amazon S3.
- E. Use Amazon Kinesis Data Streams to stream the data. Use AWS Glue to transform the data. Use Amazon Kinesis Data Firehose to write the data to Amazon S3. Use the Amazon RDS query editor to query the transformed data from Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* Option A is a fully managed serverless pipeline: KDS -> KDA (transform) -> Firehose -> S3, with Athena for SQL queries. Option B uses MSK and Glue for transformation, then Athena. Options C (DMS) is for migration, not streaming. Options D/E use RDS query editor (not for S3). (AWS Docs: KDA for Streaming Transformations, Athena for S3).
</details>

---

**Q293.** A company has an on-premises volume backup solution that has reached its end of life. The company wants to use AWS as part of a new backup solution and wants to maintain local access to all the data while it is backed up on AWS. The company wants to ensure that the data backed up on AWS is automatically and securely transferred. Which solution meets these requirements?

- A. Use AWS Snowball to migrate data out of the on-premises solution to Amazon S3. Configure on-premises systems to mount the Snowball S3 endpoint to provide local access to the data.
- B. Use AWS Snowball Edge to migrate data out of the on-premises solution to Amazon S3. Use the Snowball Edge file interface to provide on-premises systems with local access to the data.
- C. Use AWS Storage Gateway and configure a cached volume gateway. Run the Storage Gateway software appliance on premises and configure a percentage of data to cache locally. Mount the gateway storage volumes to provide local access to the data.
- D. Use AWS Storage Gateway and configure a stored volume gateway. Run the Storage Gateway software appliance on premises and map the gateway storage volumes to on-premises storage. Mount the gateway storage volumes to provide local access to the data.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Stored volume gateway stores all data on-premises and asynchronously backs up to S3. It provides low-latency local access to the entire dataset. Cached volume gateway (C) stores only hot data locally. Snowball (A, B) is for one-time transfer, not ongoing backup. (AWS Docs: Storage Gateway Stored Volumes).
</details>

---

**Q294.** An application that is hosted on Amazon EC2 instances needs to access an Amazon S3 bucket. Traffic must not traverse the internet. How should a solutions architect configure access to meet these requirements?

- A. Create a private hosted zone by using Amazon Route 53.
- B. Set up a gateway VPC endpoint for Amazon S3 in the VPC.
- C. Configure the EC2 instances to use a NAT gateway to access the S3 bucket.
- D. Establish an AWS Site-to-Site VPN connection between the VPC and the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A gateway VPC endpoint for S3 provides private connectivity from the VPC to S3 without traversing the internet. NAT gateway (C) routes to internet. VPN (D) is for on-premises, not needed. Route 53 (A) is DNS. (AWS Docs: S3 VPC Endpoint).
</details>

---

**Q295.** An ecommerce company stores terabytes of customer data in the AWS Cloud. The data contains personally identifiable information (PII). The company wants to use the data in three applications. Only one of the applications needs to process the PII. The PII must be removed before the other two applications process the data. Which solution will meet these requirements with the LEAST operational overhead?

- A. Store the data in an Amazon DynamoDB table. Create a proxy application layer to intercept and process the data that each application requests.
- B. Store the data in an Amazon S3 bucket. Process and transform the data by using S3 Object Lambda before returning the data to the requesting application.
- C. Process the data and store the transformed data in three separate Amazon S3 buckets so that each application has its own custom dataset. Point each application to its respective S3 bucket.
- D. Process the data and store the transformed data in three separate Amazon DynamoDB tables so that each application has its own custom dataset. Point each application to its respective DynamoDB table.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Object Lambda allows you to add custom code (e.g., redact PII) to modify data as it is retrieved from S3, without storing multiple copies. This is the least overhead. Options C and D require storing three copies of the data. Option A (DynamoDB + proxy) is more complex. (AWS Docs: S3 Object Lambda for PII Redaction).
</details>

---

**Q296.** A development team has launched a new application that is hosted on Amazon EC2 instances inside a development VPC. A solutions architect needs to create a new VPC in the same account. The new VPC will be peered with the development VPC. The VPC CIDR block for the development VPC is 192.168.0.0/24. The solutions architect needs to create a CIDR block for the new VPC. The CIDR block must be valid for a VPC peering connection to the development VPC. What is the SMALLEST CIDR block that meets these requirements?

- A. 10.0.1.0/32
- B. 192.168.0.0/24
- C. 192.168.1.0/32
- D. 10.0.1.0/24

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* VPC CIDR blocks must not overlap for peering. 192.168.0.0/24 is the development VPC, so new VPC must use a different range. The smallest valid VPC CIDR is /28, but among options, /24 is the smallest valid CIDR that is not overlapping. /32 (A, C) is not a valid VPC CIDR. Option B overlaps. Option D (10.0.1.0/24) is non-overlapping and valid. (AWS Docs: VPC Peering CIDR Requirements).
</details>

---

**Q297.** A company deploys an application on five Amazon EC2 instances. An Application Load Balancer (ALB) distributes traffic to the instances by using a target group. The average CPU usage on each of the instances is below 10% most of the time, with occasional surges to 65%. A solutions architect needs to implement a solution to automate the scalability of the application. The solution must optimize the cost of the architecture and must ensure that the application has enough CPU resources when surges occur. Which solution will meet these requirements?

- A. Create an Amazon CloudWatch alarm that enters the ALARM state when the CPUUtilization metric is less than 20%. Create an AWS Lambda function that the CloudWatch alarm invokes to terminate one of the EC2 instances in the ALB target group.
- B. Create an EC2 Auto Scaling group. Select the existing ALB as the load balancer and the existing target group as the target group. Set a target tracking scaling policy that is based on the ASGAverageCPUUtilization metric. Set the minimum instances to 2, the desired capacity to 3, the maximum instances to 6, and the target value to 50%. Add the EC2 instances to the Auto Scaling group.
- C. Create an EC2 Auto Scaling group. Select the existing ALB as the load balancer and the existing target group as the target group. Set the minimum instances to 2, the desired capacity to 3, and the maximum instances to 6. Add the EC2 instances to the Auto Scaling group.
- D. Create two Amazon CloudWatch alarms. Configure the first CloudWatch alarm to enter the ALARM state when the average CPUUtilization metric is below 20%. Configure the second CloudWatch alarm to enter the ALARM state when the average CPUUtilization metric is above 50%. Configure the alarms to publish to an Amazon Simple Notification Service (Amazon SNS) topic to send an email message. After receiving the message, log in to decrease or increase the number of EC2 instances that are running.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Target tracking scaling policy based on ASGAverageCPUUtilization (target 50%) automatically scales out/in to maintain CPU around 50%, handling surges and optimizing cost. Minimum 2 ensures HA. Option A terminates on low CPU (not good). Option C has no scaling policy. Option D requires manual intervention. (AWS Docs: Target Tracking Scaling Policies).
</details>

---

**Q298.** A company is running a critical business application on Amazon EC2 instances behind an Application Load Balancer. The EC2 instances run in an Auto Scaling group and access an Amazon RDS DB instance. The design did not pass an operational review because the EC2 instances and the DB instance are all located in a single Availability Zone. A solutions architect must update the design to use a second Availability Zone. Which solution will make the application highly available?

- A. Provision a subnet in each Availability Zone. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance with connections to each network.
- B. Provision two subnets that extend across both Availability Zones. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance with connections to each network.
- C. Provision a subnet in each Availability Zone. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance for Multi-AZ deployment.
- D. Provision a subnet that extends across both Availability Zones. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance for Multi-AZ deployment.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Subnets are per-AZ, so you need a subnet in each AZ. Auto Scaling group across both AZs. RDS Multi-AZ provides a standby in another AZ. Option A (DB connections) is not enough. Option B (subnets extending across AZs) is invalid. Option D (single subnet) cannot span AZs. (AWS Docs: Multi-AZ Architecture).
</details>

---

**Q299.** A research laboratory needs to process approximately 8 TB of data. The laboratory requires sub-millisecond latencies and a minimum throughput of 6 GBps for the storage subsystem. Hundreds of Amazon EC2 instances that run Amazon Linux will distribute and process the data. Which solution will meet the performance requirements?

- A. Create an Amazon FSx for NetApp ONTAP file system. Set each volume tiering policy to ALL. Import the raw data into the file system. Mount the file system on the EC2 instances.
- B. Create an Amazon S3 bucket to store the raw data. Create an Amazon FSx for Lustre file system that uses persistent SSD storage. Select the option to import data from and export data to Amazon S3. Mount the file system on the EC2 instances.
- C. Create an Amazon S3 bucket to store the raw data. Create an Amazon FSx for Lustre file system that uses persistent HDD storage. Select the option to import data from and export data to Amazon S3. Mount the file system on the EC2 instances.
- D. Create an Amazon FSx for NetApp ONTAP file system. Set each volume's tiering policy to NONE. Import the raw data into the file system. Mount the file system on the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* FSx for Lustre with persistent SSD storage delivers sub-millisecond latency and high throughput (GBps). It can be linked to S3 for data import/export. Option A/C (HDD) do not meet performance. Option D (ONTAP) is not as performant as Lustre for HPC. (AWS Docs: FSx for Lustre Performance).
</details>

---

**Q300.** A company needs to migrate a legacy application from an on-premises data center to the AWS Cloud because of hardware capacity constraints. The application runs 24 hours a day, 7 days a week. The application's database storage continues to grow over time. What should a solutions architect do to meet these requirements MOST cost-effectively?

- A. Migrate the application layer to Amazon EC2 Spot Instances. Migrate the data storage layer to Amazon S3.
- B. Migrate the application layer to Amazon EC2 Reserved Instances. Migrate the data storage layer to Amazon RDS On-Demand Instances.
- C. Migrate the application layer to Amazon EC2 Reserved Instances. Migrate the data storage layer to Amazon Aurora Reserved Instances.
- D. Migrate the application layer to Amazon EC2 On-Demand Instances. Migrate the data storage layer to Amazon RDS Reserved Instances.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Application runs 24/7 -> Reserved Instances for cost savings. Database storage grows over time -> RDS On-Demand allows scaling storage independently. Aurora Reserved (C) is more expensive than RDS for this use case. Spot (A) is not for 24/7 critical apps. (AWS Docs: EC2 Reserved Instances, RDS On-Demand).
</details>

---

**Q301.** A university research laboratory needs to migrate 30 TB of data from an on-premises Windows file server to Amazon FSx for Windows File Server. The laboratory has a 1 Gbps network link that many other departments in the university share. The laboratory wants to implement a data migration service that will maximize the performance of the data transfer. However, the laboratory needs to be able to control the amount of bandwidth that the service uses to minimize the impact on other departments. The data migration must take place within the next 5 days. Which AWS solution will meet these requirements?

- A. AWS Snowcone
- B. Amazon FSx File Gateway
- C. AWS DataSync
- D. AWS Transfer Family

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS DataSync supports bandwidth throttling, allowing you to limit the impact on other departments. It can transfer 30 TB over 1 Gbps in <5 days (30 TB * 8 / 1 Gbps = 240,000 seconds = ~2.8 days). Snowcone (A) is for offline transfer. FSx File Gateway (B) is for hybrid access, not migration. Transfer Family (D) is for SFTP. (AWS Docs: DataSync Bandwidth Throttling).
</details>

---

**Q302.** A company wants to create a mobile app that allows users to stream slow-motion video clips on their mobile devices. Currently, the app captures video clips and uploads the video clips in raw format into an Amazon S3 bucket. The app retrieves these video clips directly from the S3 bucket. However, the videos are large in their raw format. Users are experiencing issues with buffering and playback on mobile devices. The company wants to implement solutions to maximize the performance and scalability of the app while minimizing operational overhead. Which combination of solutions will meet these requirements? (Choose two.)

- A. Deploy Amazon CloudFront for content delivery and caching.
- B. Use AWS DataSync to replicate the video files across AWS Regions in other S3 buckets.
- C. Use Amazon Elastic Transcoder to convert the video files to more appropriate formats.
- D. Deploy an Auto Scaling group of Amazon EC2 instances in Local Zones for content delivery and caching.
- E. Deploy an Auto Scaling group of Amazon EC2 instances to convert the video files to more appropriate formats.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* CloudFront (A) provides edge caching and delivery for video streaming, reducing buffering. Amazon Elastic Transcoder (C) converts raw videos to formats optimized for mobile devices (e.g., HLS, MPEG-DASH). DataSync (B) is for transfer, not streaming. EC2-based solutions (D, E) have higher operational overhead. (AWS Docs: CloudFront for Video, Elastic Transcoder).
</details>

---

**Q303.** A company is launching a new application deployed on an Amazon Elastic Container Service (Amazon ECS) cluster and is using the Fargate launch type for ECS tasks. The company is monitoring CPU and memory usage because it is expecting high traffic to the application upon its launch. However, the company wants to reduce costs when utilization decreases. What should a solutions architect recommend?

- A. Use Amazon EC2 Auto Scaling to scale at certain periods based on previous traffic patterns.
- B. Use an AWS Lambda function to scale Amazon ECS based on metric breaches that trigger an Amazon CloudWatch alarm.
- C. Use Amazon EC2 Auto Scaling with simple scaling policies to scale when ECS metric breaches trigger an Amazon CloudWatch alarm.
- D. Use AWS Application Auto Scaling with target tracking policies to scale when ECS metric breaches trigger an Amazon CloudWatch alarm.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Application Auto Scaling supports ECS services with Fargate. Target tracking scaling policies automatically adjust desired task count based on CPU/memory utilization, scaling up during high traffic and down during low traffic to optimize costs. EC2 Auto Scaling (A, C) is not for ECS/Fargate. Lambda (B) adds overhead. (AWS Docs: Application Auto Scaling for ECS).
</details>

---

**Q304.** A company recently created a disaster recovery site in a different AWS Region. The company needs to transfer large amounts of data back and forth between NFS file systems in the two Regions on a periodic basis. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS DataSync.
- B. Use AWS Snowball devices.
- C. Set up an SFTP server on Amazon EC2.
- D. Use AWS Database Migration Service (AWS DMS).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS DataSync supports cross-Region data transfer for NFS file systems with scheduling and incremental transfers, minimizing overhead. Snowball (B) is for offline/one-time transfers. SFTP server (C) requires management. DMS (D) is for databases. (AWS Docs: DataSync Cross-Region).
</details>

---

**Q305.** A company is designing a shared storage solution for a gaming application that is hosted in the AWS Cloud. The company needs the ability to use SMB clients to access data. The solution must be fully managed. Which AWS solution meets these requirements?

- A. Create an AWS DataSync task that shares the data as a mountable file system. Mount the file system to the application server.
- B. Create an Amazon EC2 Windows instance. Install and configure a Windows file share role on the instance. Connect the application server to the file share.
- C. Create an Amazon FSx for Windows File Server file system. Attach the file system to the origin server. Connect the application server to the file system.
- D. Create an Amazon S3 bucket. Assign an IAM role to the application to grant access to the S3 bucket. Mount the S3 bucket to the application server.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* FSx for Windows File Server is a fully managed SMB file system. DataSync (A) is for transfer. EC2 Windows (B) is not fully managed. S3 (D) is object storage, not SMB. (AWS Docs: FSx for Windows File Server).
</details>

---

**Q306.** A company wants to run an in-memory database for a latency-sensitive application that runs on Amazon EC2 instances. The application processes more than 100,000 transactions each minute and requires high network throughput. A solutions architect needs to provide a cost-effective network design that minimizes data transfer charges. Which solution meets these requirements?

- A. Launch all EC2 instances in the same Availability Zone within the same AWS Region. Specify a placement group with cluster strategy when launching EC2 instances.
- B. Launch all EC2 instances in different Availability Zones within the same AWS Region. Specify a placement group with partition strategy when launching EC2 instances.
- C. Deploy an Auto Scaling group to launch EC2 instances in different Availability Zones based on a network utilization target.
- D. Deploy an Auto Scaling group with a step scaling policy to launch EC2 instances in different Availability Zones.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* For high network throughput and low latency between instances, a cluster placement group within a single Availability Zone minimizes network hops and reduces data transfer charges (no cross-AZ charges). Option B (different AZs) incurs cross-AZ charges. Options C/D are about scaling, not network design. (AWS Docs: Cluster Placement Groups).
</details>

---

**Q307.** A company that primarily runs its application servers on premises has decided to migrate to AWS. The company wants to minimize its need to scale its Internet Small Computer Systems Interface (iSCSI) storage on premises. The company wants only its recently accessed data to remain stored locally. Which AWS solution should the company use to meet these requirements?

- A. Amazon S3 File Gateway
- B. AWS Storage Gateway Tape Gateway
- C. AWS Storage Gateway Volume Gateway stored volumes
- D. AWS Storage Gateway Volume Gateway cached volumes

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Volume Gateway cached volumes store recently accessed data locally (on-premises) for low-latency access, while the primary data is stored in S3. This minimizes on-premises storage scaling. S3 File Gateway (A) is for file shares. Tape Gateway (B) is for backup. Stored volumes (C) store all data locally. (AWS Docs: Cached Volumes).
</details>

---

**Q308.** A company has multiple AWS accounts that use consolidated billing. The company runs several active high performance Amazon RDS for Oracle On-Demand DB instances for 90 days. The company's finance team has access to AWS Trusted Advisor in the consolidated billing account and all other AWS accounts. The finance team needs to use the appropriate AWS account to access the Trusted Advisor check recommendations for RDS. The finance team must review the appropriate Trusted Advisor check to reduce RDS costs. Which combination of steps should the finance team take to meet these requirements? (Choose two.)

- A. Use the Trusted Advisor recommendations from the account where the RDS instances are running.
- B. Use the Trusted Advisor recommendations from the consolidated billing account to see all RDS instance checks at the same time.
- C. Review the Trusted Advisor check for Amazon RDS Reserved Instance Optimization.
- D. Review the Trusted Advisor check for Amazon RDS Idle DB Instances.
- E. Review the Trusted Advisor check for Amazon Redshift Reserved Node Optimization.

<details>
<summary>Show Answer</summary>

**Answer: C, D**

*Explanation:* Trusted Advisor provides checks for RDS Reserved Instance Optimization (C) and RDS Idle DB Instances (D) to help reduce costs. The finance team should review these. Option A (account where RDS runs) is correct for viewing checks, but the question asks for which checks. Option B is incorrect (consolidated billing account does not show all checks). Option E is for Redshift. (AWS Docs: Trusted Advisor RDS Checks).
</details>

---

**Q309.** A solutions architect needs to optimize storage costs. The solutions architect must identify any Amazon S3 buckets that are no longer being accessed or are rarely accessed. Which solution will accomplish this goal with the LEAST operational overhead?

- A. Analyze bucket access patterns by using the S3 Storage Lens dashboard for advanced activity metrics.
- B. Analyze bucket access patterns by using the S3 dashboard in the AWS Management Console.
- C. Turn on the Amazon CloudWatch BucketSizeBytes metric for buckets. Analyze bucket access patterns by using the metrics data with Amazon Athena.
- D. Turn on AWS CloudTrail for S3 object monitoring. Analyze bucket access patterns by using CloudTrail logs that are integrated with Amazon CloudWatch Logs.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Storage Lens provides organization-wide visibility into storage usage and activity trends, including identifying rarely accessed buckets, with minimal setup. Option B (S3 dashboard) lacks advanced analytics. Options C and D require additional configuration and overhead. (AWS Docs: S3 Storage Lens).
</details>

---

**Q310.** A company sells datasets to customers who do research in artificial intelligence and machine learning (AI/ML). The datasets are large, formatted files that are stored in an Amazon S3 bucket in the us-east-1 Region. The company hosts a web application that the customers use to purchase access to a given dataset. The web application is deployed on multiple Amazon EC2 instances behind an Application Load Balancer. After a purchase is made, customers receive an S3 signed URL that allows access to the files. The customers are distributed across North America and Europe. The company wants to reduce the cost that is associated with data transfers and wants to maintain or improve performance. What should a solutions architect do to meet these requirements?

- A. Configure S3 Transfer Acceleration on the existing S3 bucket. Direct customer requests to the S3 Transfer Acceleration endpoint. Continue to use S3 signed URLs for access control.
- B. Deploy an Amazon CloudFront distribution with the existing S3 bucket as the origin. Direct customer requests to the CloudFront URL. Switch to CloudFront signed URLs for access control.
- C. Set up a second S3 bucket in the eu-central-1 Region with S3 Cross-Region Replication between the buckets. Direct customer requests to the closest Region. Continue to use S3 signed URLs for access control.
- D. Modify the web application to enable streaming of the datasets to end users. Configure the web application to read the data from the existing S3 bucket. Implement access control directly in the application.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* CloudFront caches data at edge locations, reducing data transfer costs (lower egress rates than S3) and improving performance. Switching to CloudFront signed URLs maintains access control. Option A (Transfer Acceleration) is for uploads. Option C (second bucket + replication) incurs replication costs. Option D (streaming via app) adds overhead. (AWS Docs: CloudFront for Data Distribution).
</details>

---

**Q311.** A company is using AWS to design a web application that will process insurance quotes. Users will request quotes from the application. Quotes must be separated by quote type, must be responded to within 24 hours, and must not get lost. The solution must maximize operational efficiency and must minimize maintenance. Which solution meets these requirements?

- A. Create multiple Amazon Kinesis data streams based on the quote type. Configure the web application to send messages to the proper data stream. Configure each backend group of application servers to use the Kinesis Client Library (KCL) to pool messages from its own data stream.
- B. Create an AWS Lambda function and an Amazon Simple Notification Service (Amazon SNS) topic for each quote type. Subscribe the Lambda function to its associated SNS topic. Configure the application to publish requests for quotes to the appropriate SNS topic.
- C. Create a single Amazon Simple Notification Service (Amazon SNS) topic. Subscribe Amazon Simple Queue Service (Amazon SQS) queues to the SNS topic. Configure SNS message filtering to publish messages to the proper SQS queue based on the quote type. Configure each backend application server to use its own SQS queue.
- D. Create multiple Amazon Kinesis Data Firehose delivery streams based on the quote type to deliver data streams to an Amazon OpenSearch Service cluster. Configure the application to send messages to the proper delivery stream. Configure each backend group of application servers to search for the messages from OpenSearch Service and process them accordingly.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* SNS with SQS fan-out and message filtering provides durable, scalable, and decoupled processing by quote type. This minimizes maintenance (fully managed). Option A (Kinesis) has more overhead. Option B (SNS to Lambda) may lose messages if Lambda fails. Option D (Firehose to OpenSearch) is for analytics, not queuing. (AWS Docs: SNS Message Filtering, SQS).
</details>

---

**Q312.** A company has an application that runs on several Amazon EC2 instances. Each EC2 instance has multiple Amazon Elastic Block Store (Amazon EBS) data volumes attached to it. The application's EC2 instance configuration and data need to be backed up nightly. The application also needs to be recoverable in a different AWS Region. Which solution will meet these requirements in the MOST operationally efficient way?

- A. Write an AWS Lambda function that schedules nightly snapshots of the application's EBS volumes and copies the snapshots to a different Region.
- B. Create a backup plan by using AWS Backup to perform nightly backups. Copy the backups to another Region. Add the application's EC2 instances as resources.
- C. Create a backup plan by using AWS Backup to perform nightly backups. Copy the backups to another Region. Add the application's EBS volumes as resources.
- D. Write an AWS Lambda function that schedules nightly snapshots of the application's EBS volumes and copies the snapshots to a different Availability Zone.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Backup can back up EC2 instances (including EBS volumes) and copy backups cross-Region with a single backup plan. This is more operationally efficient than custom Lambda (A, D). Option C (EBS volumes only) misses instance configuration. (AWS Docs: AWS Backup for EC2).
</details>

---

**Q313.** A company is building a mobile app on AWS. The company wants to expand its reach to millions of users. The company needs to build a platform so that authorized users can watch the company's content on their mobile devices. What should a solutions architect recommend to meet these requirements?

- A. Publish content to a public Amazon S3 bucket. Use AWS Key Management Service (AWS KMS) keys to stream content.
- B. Set up IPsec VPN between the mobile app and the AWS environment to stream content.
- C. Use Amazon CloudFront. Provide signed URLs to stream content.
- D. Set up AWS Client VPN between the mobile app and the AWS environment to stream content.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* CloudFront signed URLs allow authorized users to access content securely while providing global edge delivery for streaming. VPN (B, D) adds latency and complexity. Public S3 bucket (A) is insecure. (AWS Docs: CloudFront Signed URLs for Streaming).
</details>

---

**Q314.** A company has an on-premises MySQL database used by the global sales team with infrequent access patterns. The sales team requires the database to have minimal downtime. A database administrator wants to migrate this database to AWS without selecting a particular instance type in anticipation of more users in the future. Which service should a solutions architect recommend?

- A. Amazon Aurora MySQL
- B. Amazon Aurora Serverless for MySQL
- C. Amazon Redshift Spectrum
- D. Amazon RDS for MySQL

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Aurora Serverless for MySQL automatically scales compute capacity based on demand and does not require selecting an instance type. It is suitable for infrequent access patterns and minimizes downtime during scaling. Aurora provisioned (A) requires instance selection. Redshift Spectrum (C) is for analytics. RDS (D) requires instance selection. (AWS Docs: Aurora Serverless).
</details>

---

**Q315.** A company experienced a breach that affected several applications in its on-premises data center. The attacker took advantage of vulnerabilities in the custom applications that were running on the servers. The company is now migrating its applications to run on Amazon EC2 instances. The company wants to implement a solution that actively scans for vulnerabilities on the EC2 instances and sends a report that details the findings. Which solution will meet these requirements?

- A. Deploy AWS Shield to scan the EC2 instances for vulnerabilities. Create an AWS Lambda function to log any findings to AWS CloudTrail.
- B. Deploy Amazon Macie and AWS Lambda functions to scan the EC2 instances for vulnerabilities. Log any findings to AWS CloudTrail.
- C. Turn on Amazon GuardDuty. Deploy the GuardDuty agents to the EC2 instances. Configure an AWS Lambda function to automate the generation and distribution of reports that detail the findings.
- D. Turn on Amazon Inspector. Deploy the Amazon Inspector agent to the EC2 instances. Configure an AWS Lambda function to automate the generation and distribution of reports that detail the findings.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Amazon Inspector is designed to scan EC2 instances for software vulnerabilities and unintended network exposure. It can integrate with Lambda for automated reporting. Shield (A) is for DDoS. Macie (B) is for data security. GuardDuty (C) is for threat detection, not vulnerability scanning. (AWS Docs: Amazon Inspector).
</details>

---

**Q316.** A company uses an Amazon EC2 instance to run a script to poll for and process messages in an Amazon Simple Queue Service (Amazon SQS) queue. The company wants to reduce operational costs while maintaining its ability to process a growing number of messages that are added to the queue. What should a solutions architect recommend to meet these requirements?

- A. Increase the size of the EC2 instance to process messages faster.
- B. Use Amazon EventBridge to turn off the EC2 instance when the instance is underutilized.
- C. Migrate the script on the EC2 instance to an AWS Lambda function with the appropriate runtime.
- D. Use AWS Systems Manager Run Command to run the script on demand.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Migrating to Lambda (with SQS event source mapping) removes the need for a continuously running EC2 instance, reducing costs. Lambda scales automatically with message volume. Increasing EC2 size (A) increases cost. EventBridge (B) doesn't solve the polling cost. Run Command (D) is for remote execution, not queue processing. (AWS Docs: Lambda with SQS).
</details>

---

**Q317.** A company uses a legacy application to produce data in CSV format. The legacy application stores the output data in Amazon S3. The company is deploying a new commercial off-the-shelf (COTS) application that can perform complex SQL queries to analyze data that is stored in Amazon Redshift and Amazon S3 only. However, the COTS application cannot process the .csv files that the legacy application produces. The company cannot update the legacy application to produce data in another format. The company needs to implement a solution so that the COTS application can use the data that the legacy application produces. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an AWS Glue extract, transform, and load (ETL) job that runs on a schedule. Configure the ETL job to process the .csv files and store the processed data in Amazon Redshift.
- B. Develop a Python script that runs on Amazon EC2 instances to convert the .csv files to .sql files. Invoke the Python script on a cron schedule to store the output files in Amazon S3.
- C. Create an AWS Lambda function and an Amazon DynamoDB table. Use an S3 event to invoke the Lambda function. Configure the Lambda function to perform an extract, transform, and load (ETL) job to process the .csv files and store the processed data in the DynamoDB table.
- D. Use Amazon EventBridge to launch an Amazon EMR cluster on a weekly schedule. Configure the EMR cluster to perform an extract, transform, and load (ETL) job to process the .csv files and store the processed data in an Amazon Redshift table.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Glue ETL can read CSV from S3, transform it (e.g., to Parquet or other format), and write to Redshift (which the COTS app can query). This is serverless and has less overhead than EC2 scripts (B), Lambda (C), or EMR (D). (AWS Docs: Glue ETL to Redshift).
</details>

---

**Q318.** A company recently migrated its entire IT environment to the AWS Cloud. The company discovers that users are provisioning oversized Amazon EC2 instances and modifying security group rules without using the appropriate change control process. A solutions architect must devise a strategy to track and audit these inventory and configuration changes. Which actions should the solutions architect take to meet these requirements? (Choose two.)

- A. Enable AWS CloudTrail and use it for auditing.
- B. Use data lifecycle policies for the Amazon EC2 instances.
- C. Enable AWS Trusted Advisor and reference the security dashboard.
- D. Enable AWS Config and create rules for auditing and compliance purposes.
- E. Restore previous resource configurations with an AWS CloudFormation template.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* CloudTrail (A) records API calls (who made changes). AWS Config (D) tracks configuration changes over time and can evaluate compliance against rules. Trusted Advisor (C) is for best practices, not auditing. Lifecycle policies (B) are for backups. CloudFormation (E) is for provisioning, not auditing. (AWS Docs: CloudTrail for Auditing, AWS Config for Compliance).
</details>

---

**Q319.** A company has hundreds of Amazon EC2 Linux-based instances in the AWS Cloud. Systems administrators have used shared SSH keys to manage the instances. After a recent audit, the company's security team is mandating the removal of all shared keys. A solutions architect must design a solution that provides secure access to the EC2 instances. Which solution will meet this requirement with the LEAST amount of administrative overhead?

- A. Use AWS Systems Manager Session Manager to connect to the EC2 instances.
- B. Use AWS Security Token Service (AWS STS) to generate one-time SSH keys on demand.
- C. Allow shared SSH access to a set of bastion instances. Configure all other instances to allow only SSH access from the bastion instances.
- D. Use an Amazon Cognito custom authorizer to authenticate users. Invoke an AWS Lambda function to generate a temporary SSH key.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Session Manager provides secure, auditable, keyless shell access to EC2 instances via IAM. This eliminates SSH keys entirely and has the least administrative overhead. Options B and D still involve SSH keys and custom code. Option C still uses shared keys on bastion. (AWS Docs: Session Manager).
</details>

---

**Q320.** A company is using a fleet of Amazon EC2 instances to ingest data from on-premises data sources. The data is in JSON format and ingestion rates can be as high as 1 MB/s. When an EC2 instance is rebooted, the data in-flight is lost. The company's data science team wants to query ingested data in near-real time. Which solution provides near-real-time data querying that is scalable with minimal data loss?

- A. Publish data to Amazon Kinesis Data Streams. Use Kinesis Data Analytics to query the data.
- B. Publish data to Amazon Kinesis Data Firehose with Amazon Redshift as the destination. Use Amazon Redshift to query the data.
- C. Store ingested data in an EC2 instance store. Publish data to Amazon Kinesis Data Firehose with Amazon S3 as the destination. Use Amazon Athena to query the data.
- D. Store ingested data in an Amazon Elastic Block Store (Amazon EBS) volume. Publish data to Amazon ElastiCache for Redis. Subscribe to the Redis channel to query the data.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Kinesis Data Streams provides durable, scalable ingestion with data retention. Kinesis Data Analytics allows near-real-time SQL queries on the stream. This minimizes data loss (stream stores data). Option B (Firehose to Redshift) has latency. Option C (instance store) loses data on reboot. Option D (Redis) is not durable. (AWS Docs: Kinesis Data Analytics).
</details>

---

**Q321.** What should a solutions architect do to ensure that all objects uploaded to an Amazon S3 bucket are encrypted?

- A. Update the bucket policy to deny if the PutObject does not have an s3:x-amz-acl header set.
- B. Update the bucket policy to deny if the PutObject does not have an s3:x-amz-acl header set to private.
- C. Update the bucket policy to deny if the PutObject does not have an aws:SecureTransport header set to true.
- D. Update the bucket policy to deny if the PutObject does not have an x-amz-server-side-encryption header set.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* To enforce encryption, create a bucket policy that denies `PutObject` requests that do not include the `x-amz-server-side-encryption` header (with a value like `AES256` or `aws:kms`). Option A/B (ACL) are for permissions. Option C (SecureTransport) enforces HTTPS, not encryption at rest. (AWS Docs: S3 Encryption Enforcing Policy).
</details>

---

**Q322.** A solutions architect is designing a multi-tier application for a company. The application's users upload images from a mobile device. The application generates a thumbnail of each image and returns a message to the user to confirm that the image was uploaded successfully. The thumbnail generation can take up to 60 seconds, but the company wants to provide a faster response time to its users to notify them that the original image was received. The solutions architect must design the application to asynchronously dispatch requests to the different application tiers. What should the solutions architect do to meet these requirements?

- A. Write a custom AWS Lambda function to generate the thumbnail and alert the user. Use the image upload process as an event source to invoke the Lambda function.
- B. Create an AWS Step Functions workflow. Configure Step Functions to handle the orchestration between the application tiers and alert the user when thumbnail generation is complete.
- C. Create an Amazon Simple Queue Service (Amazon SQS) message queue. As images are uploaded, place a message on the SQS queue for thumbnail generation. Alert the user through an application message that the image was received.
- D. Create Amazon Simple Notification Service (Amazon SNS) notification topics and subscriptions. Use one subscription with the application to generate the thumbnail after the image upload is complete. Use a second subscription to message the user's mobile app by way of a push notification after thumbnail generation is complete.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Using SQS to queue the thumbnail generation task allows the application to immediately acknowledge the upload to the user, providing fast response. Thumbnail generation happens asynchronously. Option A (Lambda) might be synchronous. Option B (Step Functions) adds complexity. Option D (SNS) would still need to wait for thumbnail generation to notify user. (AWS Docs: Asynchronous Processing with SQS).
</details>

---

**Q323.** A company's facility has badge readers at every entrance throughout the building. When badges are scanned, the readers send a message over HTTPS to indicate who attempted to access that particular entrance. A solutions architect must design a system to process these messages from the sensors. The solution must be highly available, and the results must be made available for the company's security team to analyze. Which system architecture should the solutions architect recommend?

- A. Launch an Amazon EC2 instance to serve as the HTTPS endpoint and to process the messages. Configure the EC2 instance to save the results to an Amazon S3 bucket.
- B. Create an HTTPS endpoint in Amazon API Gateway. Configure the API Gateway endpoint to invoke an AWS Lambda function to process the messages and save the results to an Amazon DynamoDB table.
- C. Use Amazon Route 53 to direct incoming sensor messages to an AWS Lambda function. Configure the Lambda function to process the messages and save the results to an Amazon DynamoDB table.
- D. Create a gateway VPC endpoint for Amazon S3. Configure a Site-to-Site VPN connection from the facility network to the VPC so that sensor data can be written directly to an S3 bucket by way of the VPC endpoint.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* API Gateway provides a managed HTTPS endpoint. Lambda processes messages serverlessly. DynamoDB provides scalable storage for analysis. This is highly available and scalable. Option A (EC2) has single point of failure. Option C (Route 53 to Lambda) cannot directly invoke Lambda without API Gateway. Option D (S3 endpoint) doesn't provide HTTPS endpoint for sensors. (AWS Docs: API Gateway + Lambda + DynamoDB).
</details>

---

**Q324.** A company wants to implement a disaster recovery plan for its primary on-premises file storage volume. The file storage volume is mounted from an Internet Small Computer Systems Interface (iSCSI) device on a local storage server. The file storage volume holds hundreds of terabytes (TB) of data. The company wants to ensure that end users retain immediate access to all file types from the on-premises systems without experiencing latency. Which solution will meet these requirements with the LEAST amount of change to the company's existing infrastructure?

- A. Provision an Amazon S3 File Gateway as a virtual machine (VM) that is hosted on premises. Set the local cache to 10 TB. Modify existing applications to access the files through the NFS protocol. To recover from a disaster, provision an Amazon EC2 instance and mount the S3 bucket that contains the files.
- B. Provision an AWS Storage Gateway tape gateway. Use a data backup solution to back up all existing data to a virtual tape library. Configure the data backup solution to run nightly after the initial backup is complete. To recover from a disaster, provision an Amazon EC2 instance and restore the data to an Amazon Elastic Block Store (Amazon EBS) volume from the volumes in the virtual tape library.
- C. Provision an AWS Storage Gateway Volume Gateway cached volume. Set the local cache to 10 TB. Mount the Volume Gateway cached volume to the existing file server by using iSCSI, and copy all files to the storage volume. Configure scheduled snapshots of the storage volume. To recover from a disaster, restore a snapshot to an Amazon Elastic Block Store (Amazon EBS) volume and attach the EBS volume to an Amazon EC2 instance.
- D. Provision an AWS Storage Gateway Volume Gateway stored volume with the same amount of disk space as the existing file storage volume. Mount the Volume Gateway stored volume to the existing file server by using iSCSI, and copy all files to the storage volume. Configure scheduled snapshots of the storage volume. To recover from a disaster, restore a snapshot to an Amazon Elastic Block Store (Amazon EBS) volume and attach the EBS volume to an Amazon EC2 instance.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* A cached volume gateway allows you to keep recently accessed data locally for low latency while storing the full dataset in S3. It presents an iSCSI target, minimizing changes. Stored volumes (D) would require as much on-premises storage as the original. Option A (S3 File Gateway) uses NFS, not iSCSI. Option B (tape gateway) is for backup, not primary storage. (AWS Docs: Cached Volumes for DR).
</details>

---

**Q325.** A company is hosting a web application from an Amazon S3 bucket. The application uses Amazon Cognito as an identity provider to authenticate users and return a JSON Web Token (JWT) that provides access to protected resources that are stored in another S3 bucket. Upon deployment of the application, users report errors and are unable to access the protected content. A solutions architect must resolve this issue by providing proper permissions so that users can access the protected content. Which solution meets these requirements?

- A. Update the Amazon Cognito identity pool to assume the proper IAM role for access to the protected content.
- B. Update the S3 ACL to allow the application to access the protected content.
- C. Redeploy the application to Amazon S3 to prevent eventually consistent reads in the S3 bucket from affecting the ability of users to access the protected content.
- D. Update the Amazon Cognito pool to use custom attribute mappings within the identity pool and grant users the proper permissions to access the protected content.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* In an Amazon Cognito identity pool, you map authenticated users (via JWT) to an IAM role. This role should have permissions to read the protected S3 bucket. Updating the identity pool to assume the correct IAM role resolves the access issue. Option B (ACL) is not secure. Option C (consistency) is not the issue. Option D (custom attributes) is not the primary fix. (AWS Docs: Cognito Identity Pools IAM Roles).
</details>

---

**Q326.** An image hosting company uploads its large assets to Amazon S3 Standard buckets. The company uses multipart upload in parallel by using S3 APIs and overrides if the same object is uploaded again. For the first 30 days after upload, the objects will be accessed frequently. The objects will be used less frequently after 30 days, but the access patterns for each object will be inconsistent. The company must optimize its S3 storage costs while maintaining high availability and resiliency of stored assets. Which combination of actions should a solutions architect recommend to meet these requirements? (Choose two.)

- A. Move assets to S3 Intelligent-Tiering after 30 days.
- B. Configure an S3 Lifecycle policy to clean up incomplete multipart uploads.
- C. Configure an S3 Lifecycle policy to clean up expired object delete markers.
- D. Move assets to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days.
- E. Move assets to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 30 days.

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* S3 Intelligent-Tiering (A) automatically moves objects between frequent and infrequent tiers based on changing access patterns, optimizing cost while maintaining HA. Cleaning up incomplete multipart uploads (B) prevents storage charges for failed uploads. Option D (Standard-IA) has a minimum storage duration charge and retrieval fees. Option E (One Zone-IA) is not AZ resilient. (AWS Docs: Intelligent-Tiering, Multipart Upload Cleanup).
</details>

---

**Q327.** A solutions architect must secure a VPC network that hosts Amazon EC2 instances. The EC2 instances contain highly sensitive data and run in a private subnet. According to company policy, the EC2 instances that run in the VPC can access only approved third-party software repositories on the internet for software product updates that use the third party's URL. Other internet traffic must be blocked. Which solution meets these requirements?

- A. Update the route table for the private subnet to route the outbound traffic to an AWS Network Firewall firewall. Configure domain list rule groups.
- B. Set up an AWS WAF web ACL. Create a custom set of rules that filter traffic requests based on source and destination IP address range sets.
- C. Implement strict inbound security group rules. Configure an outbound rule that allows traffic only to the authorized software repositories on the internet by specifying the URLs.
- D. Configure an Application Load Balancer (ALB) in front of the EC2 instances. Direct all outbound traffic to the ALB. Use a URL-based rule listener in the ALB's target group for outbound access to the internet.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Network Firewall supports domain list rule groups, allowing you to allow or block traffic based on domain names (URLs). This is the correct service for outbound URL filtering. WAF (B) is for inbound web requests. Security groups (C) do not support URL-based rules. ALB (D) is for inbound traffic distribution. (AWS Docs: Network Firewall Domain Lists).
</details>

---

**Q328.** A company is hosting a three-tier ecommerce application in the AWS Cloud. The company hosts the website on Amazon S3 and integrates the website with an API that handles sales requests. The company hosts the API on three Amazon EC2 instances behind an Application Load Balancer (ALB). The API consists of static and dynamic front-end content along with backend workers that process sales requests asynchronously. The company is expecting a significant and sudden increase in the number of sales requests during events for the launch of new products. What should a solutions architect recommend to ensure that all the requests are processed successfully?

- A. Add an Amazon CloudFront distribution for the dynamic content. Increase the number of EC2 instances to handle the increase in traffic.
- B. Add an Amazon CloudFront distribution for the static content. Place the EC2 instances in an Auto Scaling group to launch new instances based on network traffic.
- C. Add an Amazon CloudFront distribution for the dynamic content. Add an Amazon ElastiCache instance in front of the ALB to reduce traffic for the API to handle.
- D. Add an Amazon CloudFront distribution for the static content. Add an Amazon Simple Queue Service (Amazon SQS) queue to receive requests from the website for later processing by the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* For handling sudden spikes in sales requests, use SQS to buffer requests and decouple the frontend from the backend workers. EC2 instances in an Auto Scaling group can process from the queue. CloudFront for static content improves performance. Option A (dynamic content CDN) doesn't help with backend processing. Option B (Auto Scaling based on network traffic) is less direct. Option C (ElastiCache) is for caching. (AWS Docs: SQS for Spike Handling).
</details>

---

**Q329.** A security audit reveals that Amazon EC2 instances are not being patched regularly. A solutions architect needs to provide a solution that will run regular security scans across a large fleet of EC2 instances. The solution should also patch the EC2 instances on a regular schedule and provide a report of each instance's patch status. Which solution will meet these requirements?

- A. Set up Amazon Macie to scan the EC2 instances for software vulnerabilities. Set up a cron job on each EC2 instance to patch the instance on a regular schedule.
- B. Turn on Amazon GuardDuty in the account. Configure GuardDuty to scan the EC2 instances for software vulnerabilities. Set up AWS Systems Manager Session Manager to patch the EC2 instances on a regular schedule.
- C. Set up Amazon Detective to scan the EC2 instances for software vulnerabilities. Set up an Amazon EventBridge scheduled rule to patch the EC2 instances on a regular schedule.
- D. Turn on Amazon Inspector in the account. Configure Amazon Inspector to scan the EC2 instances for software vulnerabilities. Set up AWS Systems Manager Patch Manager to patch the EC2 instances on a regular schedule.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Amazon Inspector scans EC2 instances for vulnerabilities. AWS Systems Manager Patch Manager automates patching on a schedule. This combination provides scanning, patching, and reporting. Macie (A) is for data security. GuardDuty (B) is for threat detection. Detective (C) is for investigation. (AWS Docs: Amazon Inspector, Patch Manager).
</details>

---

**Q330.** A company is planning to store data on Amazon RDS DB instances. The company must encrypt the data at rest. What should a solutions architect do to meet this requirement?

- A. Create a key in AWS Key Management Service (AWS KMS). Enable encryption for the DB instances.
- B. Create an encryption key. Store the key in AWS Secrets Manager. Use the key to encrypt the DB instances.
- C. Generate a certificate in AWS Certificate Manager (ACM). Enable SSL/TLS on the DB instances by using the certificate.
- D. Generate a certificate in AWS Identity and Access Management (IAM). Enable SSL/TLS on the DB instances by using the certificate.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* RDS encryption at rest uses AWS KMS keys. You create a KMS key and enable encryption on the DB instance. Secrets Manager (B) is for secrets. ACM (C) and IAM (D) certificates are for TLS in transit, not at rest. (AWS Docs: RDS Encryption at Rest).
</details>

---

**Q331.** A company must migrate 20 TB of data from a data center to the AWS Cloud within 30 days. The company's network bandwidth is limited to 15 Mbps and cannot exceed 70% utilization. What should a solutions architect do to meet these requirements?

- A. Use AWS Snowball.
- B. Use AWS DataSync.
- C. Use a secure VPN connection.
- D. Use Amazon S3 Transfer Acceleration.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* 20 TB over 15 Mbps at 70% utilization would take ~20 TB * 8 / (15 * 0.7 Mbps) = 160,000 / 10.5 ≈ 15,238 seconds ≈ 4.2 days? Actually calculation: 20 TB = 20,000 GB = 160,000 Gb. 15 Mbps * 0.7 = 10.5 Mbps. 160,000 Gb / 10.5 Mbps = ~15.2 million seconds = ~176 days. Exceeds 30 days. Snowball is the correct choice for large data transfer with limited bandwidth. (AWS Docs: Snowball for Large Transfers).
</details>

---

**Q332.** A company needs to provide its employees with secure access to confidential and sensitive files. The company wants to ensure that the files can be accessed only by authorized users. The files must be downloaded securely to the employees' devices. The files are stored in an on-premises Windows file server. However, due to an increase in remote usage, the file server is running out of capacity. Which solution will meet these requirements?

- A. Migrate the file server to an Amazon EC2 instance in a public subnet. Configure the security group to limit inbound traffic to the employees' IP addresses.
- B. Migrate the files to an Amazon FSx for Windows File Server file system. Integrate the Amazon FSx file system with the on-premises Active Directory. Configure AWS Client VPN.
- C. Migrate the files to Amazon S3, and create a private VPC endpoint. Create a signed URL to allow download.
- D. Migrate the files to Amazon S3, and create a public VPC endpoint. Allow employees to sign on with AWS IAM Identity Center (AWS Single Sign-On).

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* FSx for Windows File Server integrates with on-premises AD for authentication. AWS Client VPN provides secure remote access. This addresses capacity and security. Option A (EC2 in public subnet) is less secure. Option C (signed URLs) works but doesn't integrate with AD for authorization. Option D (public VPC endpoint) is not a thing. (AWS Docs: FSx with AD, Client VPN).
</details>

---

**Q333.** A company's application runs on Amazon EC2 instances behind an Application Load Balancer (ALB). The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. On the first day of every month at midnight, the application becomes much slower when the month-end financial calculation batch runs. This causes the CPU utilization of the EC2 instances to immediately peak to 100%, which disrupts the application. What should a solutions architect recommend to ensure the application is able to handle the workload and avoid downtime?

- A. Configure an Amazon CloudFront distribution in front of the ALB.
- B. Configure an EC2 Auto Scaling simple scaling policy based on CPU utilization.
- C. Configure an EC2 Auto Scaling scheduled scaling policy based on the monthly schedule.
- D. Configure Amazon ElastiCache to remove some of the workload from the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Since the peak is predictable (first day of month at midnight), a scheduled scaling policy can proactively increase capacity before the batch runs, avoiding the spike. Simple scaling based on CPU (B) would react after the spike, causing disruption. CloudFront (A) is for caching. ElastiCache (D) may help but doesn't address compute capacity. (AWS Docs: Scheduled Scaling).
</details>

---

**Q334.** A company wants to give a customer the ability to use on-premises Microsoft Active Directory to download files that are stored in Amazon S3. The customer's application uses an SFTP client to download the files. Which solution will meet these requirements with the LEAST operational overhead and no changes to the customer's application?

- A. Set up AWS Transfer Family with SFTP for Amazon S3. Configure integrated Active Directory authentication.
- B. Set up AWS Database Migration Service (AWS DMS) to synchronize the on-premises client with Amazon S3. Configure integrated Active Directory authentication.
- C. Set up AWS DataSync to synchronize between the on-premises location and the S3 location by using AWS IAM Identity Center (AWS Single Sign-On).
- D. Set up a Windows Amazon EC2 instance with SFTP to connect the on-premises client with Amazon S3. Integrate AWS Identity and Access Management (IAM).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Transfer Family supports SFTP and can integrate with on-premises Active Directory for authentication, with S3 as the backend. This requires no changes to the customer's SFTP client. DMS (B) is for databases. DataSync (C) is for transfer, not SFTP. EC2 instance (D) has operational overhead. (AWS Docs: Transfer Family with AD).
</details>

---

**Q335.** A company is experiencing sudden increases in demand. The company needs to provision large Amazon EC2 instances from an Amazon Machine Image (AMI). The instances will run in an Auto Scaling group. The company needs a solution that provides minimum initialization latency to meet the demand. Which solution meets these requirements?

- A. Use the aws ec2 register-image command to create an AMI from a snapshot. Use AWS Step Functions to replace the AMI in the Auto Scaling group.
- B. Enable Amazon Elastic Block Store (Amazon EBS) fast snapshot restore on a snapshot. Provision an AMI by using the snapshot. Replace the AMI in the Auto Scaling group with the new AMI.
- C. Enable AMI creation and define lifecycle rules in Amazon Data Lifecycle Manager (Amazon DLM). Create an AWS Lambda function that modifies the AMI in the Auto Scaling group.
- D. Use Amazon EventBridge to invoke AWS Backup lifecycle policies that provision AMIs. Configure Auto Scaling group capacity limits as an event source in EventBridge.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* EBS Fast Snapshot Restore ensures that volumes created from a snapshot are fully initialized at launch, minimizing latency. Using this for the AMI's underlying snapshot reduces initialization time when scaling. Option A (Step Functions) doesn't address initialization. DLM (C) is for managing snapshots. Option D is not directly related. (AWS Docs: Fast Snapshot Restore).
</details>

---

**Q336.** A company hosts a multi-tier web application that uses an Amazon Aurora MySQL DB cluster for storage. The application tier is hosted on Amazon EC2 instances. The company's IT security guidelines mandate that the database credentials be encrypted and rotated every 14 days. What should a solutions architect do to meet this requirement with the LEAST operational effort?

- A. Create a new AWS Key Management Service (AWS KMS) encryption key. Use AWS Secrets Manager to create a new secret that uses the KMS key with the appropriate credentials. Associate the secret with the Aurora DB cluster. Configure a custom rotation period of 14 days.
- B. Create two parameters in AWS Systems Manager Parameter Store: one for the user name as a string parameter and one that uses the SecureString type for the password. Select AWS Key Management Service (AWS KMS) encryption for the password parameter, and load these parameters in the application tier. Implement an AWS Lambda function that rotates the password every 14 days.
- C. Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon Elastic File System (Amazon EFS) file system. Mount the EFS file system in all EC2 instances of the application tier. Restrict the access to the file on the file system so that the application can read the file and that only super users can modify the file. Implement an AWS Lambda function that rotates the key in Aurora every 14 days and writes new credentials into the file.
- D. Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon S3 bucket that the application uses to load the credentials. Download the file to the application regularly to ensure that the correct credentials are used. Implement an AWS Lambda function that rotates the Aurora credentials every 14 days and uploads these credentials to the file in the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Secrets Manager natively supports automatic rotation for Amazon Aurora, including custom rotation schedules (every 14 days). This is the least operational effort. Options B, C, D require custom Lambda functions for rotation and additional infrastructure. (AWS Docs: Secrets Manager Rotation for Aurora).
</details>

---

**Q337.** A company has deployed a web application on AWS. The company hosts the backend database on Amazon RDS for MySQL with a primary DB instance and five read replicas to support scaling needs. The read replicas must lag no more than 1 second behind the primary DB instance. The database routinely runs scheduled stored procedures. As traffic on the website increases, the replicas experience additional lag during periods of peak load. A solutions architect must reduce the replication lag as much as possible. The solutions architect must minimize changes to the application code and must minimize ongoing operational overhead. Which solution will meet these requirements?

- A. Migrate the database to Amazon Aurora MySQL. Replace the read replicas with Aurora Replicas, and configure Aurora Auto Scaling. Replace the stored procedures with Aurora MySQL native functions.
- B. Deploy an Amazon ElastiCache for Redis cluster in front of the database. Modify the application to check the cache before the application queries the database. Replace the stored procedures with AWS Lambda functions.
- C. Migrate the database to a MySQL database that runs on Amazon EC2 instances. Choose large, compute optimized EC2 instances for all replica nodes. Maintain the stored procedures on the EC2 instances.
- D. Migrate the database to Amazon DynamoDB. Provision a large number of read capacity units (RCUs) to support the required throughput, and configure on-demand capacity scaling. Replace the stored procedures with DynamoDB streams.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Aurora MySQL has lower replication lag (typically <100ms) than RDS for MySQL, and Aurora Replicas support Auto Scaling. Native functions can replace stored procedures. This minimizes changes and overhead. Option B (ElastiCache) adds complexity. Option C (EC2) increases overhead. Option D (DynamoDB) requires major code changes. (AWS Docs: Aurora Replication Lag).
</details>

---

**Q338.** A solutions architect must create a disaster recovery (DR) plan for a high-volume software as a service (SaaS) platform. All data for the platform is stored in an Amazon Aurora MySQL DB cluster. The DR plan must replicate data to a secondary AWS Region. Which solution will meet these requirements MOST cost-effectively?

- A. Use MySQL binary log replication to an Aurora cluster in the secondary Region. Provision one DB instance for the Aurora cluster in the secondary Region.
- B. Set up an Aurora global database for the DB cluster. When setup is complete, remove the DB instance from the secondary Region.
- C. Use AWS Database Migration Service (AWS DMS) to continuously replicate data to an Aurora cluster in the secondary Region. Remove the DB instance from the secondary Region.
- D. Set up an Aurora global database for the DB cluster. Specify a minimum of one DB instance in the secondary Region.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Aurora Global Database provides managed cross-Region replication with a single secondary DB instance (minimum). This is cost-effective and operationally efficient. Option A (MySQL binlog) is not managed. Option B (remove instance) would make the secondary region not usable for failover. Option C (DMS) adds overhead. (AWS Docs: Aurora Global Database).
</details>

---

**Q339.** A company has a custom application with embedded credentials that retrieves information from an Amazon RDS MySQL DB instance. Management says the application must be made more secure with the least amount of programming effort. What should a solutions architect do to meet these requirements?

- A. Use AWS Key Management Service (AWS KMS) to create keys. Configure the application to load the database credentials from AWS KMS. Enable automatic key rotation.
- B. Create credentials on the RDS for MySQL database for the application user and store the credentials in AWS Secrets Manager. Configure the application to load the database credentials from Secrets Manager. Create an AWS Lambda function that rotates the credentials in Secret Manager.
- C. Create credentials on the RDS for MySQL database for the application user and store the credentials in AWS Secrets Manager. Configure the application to load the database credentials from Secrets Manager. Set up a credentials rotation schedule for the application user in the RDS for MySQL database using Secrets Manager.
- D. Create credentials on the RDS for MySQL database for the application user and store the credentials in AWS Systems Manager Parameter Store. Configure the application to load the database credentials from Parameter Store. Set up a credentials rotation schedule for the application user in the RDS for MySQL database using Parameter Store.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Secrets Manager supports automatic rotation for RDS MySQL with minimal configuration (no custom Lambda required). This is the least programming effort. Option B requires custom Lambda (more effort). Option D (Parameter Store) does not have native rotation. Option A (KMS) is for encryption, not secret storage. (AWS Docs: Secrets Manager RDS Rotation).
</details>

---

**Q340.** A media company hosts its website on AWS. The website application's architecture includes a fleet of Amazon EC2 instances behind an Application Load Balancer (ALB) and a database that is hosted on Amazon Aurora. The company's cybersecurity team reports that the application is vulnerable to SQL injection. How should the company resolve this issue?

- A. Use AWS WAF in front of the ALB. Associate the appropriate web ACLs with AWS WAF.
- B. Create an ALB listener rule to reply to SQL injections with a fixed response.
- C. Subscribe to AWS Shield Advanced to block all SQL injection attempts automatically.
- D. Set up Amazon Inspector to block all SQL injection attempts automatically.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS WAF provides protection against SQL injection attacks and can be associated with an ALB. Shield Advanced (C) is for DDoS. Inspector (D) is for vulnerability scanning. ALB listener rules (B) cannot detect SQL injection. (AWS Docs: WAF SQL Injection Protection).
</details>

---

**Q341.** A company has an Amazon S3 data lake that is governed by AWS Lake Formation. The company wants to create a visualization in Amazon QuickSight by joining the data in the data lake with operational data that is stored in an Amazon Aurora MySQL database. The company wants to enforce column-level authorization so that the company's marketing team can access only a subset of columns in the database. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon EMR to ingest the data directly from the database to the QuickSight SPICE engine. Include only the required columns.
- B. Use AWS Glue Studio to ingest the data from the database to the S3 data lake. Attach an IAM policy to the QuickSight users to enforce column-level access control. Use Amazon S3 as the data source in QuickSight.
- C. Use AWS Glue Elastic Views to create a materialized view for the database in Amazon S3. Create an S3 bucket policy to enforce column-level access control for the QuickSight users. Use Amazon S3 as the data source in QuickSight.
- D. Use a Lake Formation blueprint to ingest the data from the database to the S3 data lake. Use Lake Formation to enforce column-level access control for the QuickSight users. Use Amazon Athena as the data source in QuickSight.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Lake Formation provides column-level access control and can ingest data from Aurora via blueprints. QuickSight can connect to Athena, which queries the data lake with Lake Formation permissions. This is the most integrated and least overhead. Options A, B, C require additional steps or do not support column-level access natively. (AWS Docs: Lake Formation Column-Level Access, QuickSight with Athena).
</details>

---

**Q342.** A transaction processing company has weekly scripted batch jobs that run on Amazon EC2 instances. The EC2 instances are in an Auto Scaling group. The number of transactions can vary, but the baseline CPU utilization that is noted on each run is at least 60%. The company needs to provision the capacity 30 minutes before the jobs run. Currently, engineers complete this task by manually modifying the Auto Scaling group parameters. The company does not have the resources to analyze the required capacity trends for the Auto Scaling group counts. The company needs an automated way to modify the Auto Scaling group's desired capacity. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create a dynamic scaling policy for the Auto Scaling group. Configure the policy to scale based on the CPU utilization metric. Set the target value for the metric to 60%.
- B. Create a scheduled scaling policy for the Auto Scaling group. Set the appropriate desired capacity, minimum capacity, and maximum capacity. Set the recurrence to weekly. Set the start time to 30 minutes before the batch jobs run.
- C. Create a predictive scaling policy for the Auto Scaling group. Configure the policy to scale based on forecast. Set the scaling metric to CPU utilization. Set the target value for the metric to 60%. In the policy, set the instances to pre-launch 30 minutes before the jobs run.
- D. Create an Amazon EventBridge event to invoke an AWS Lambda function when the CPU utilization metric value for the Auto Scaling group reaches 60%. Configure the Lambda function to increase the Auto Scaling group's desired capacity and maximum capacity by 20%.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Predictive scaling uses machine learning to forecast demand and can pre-launch instances before the load arrives. This is the most automated solution for periodic batch jobs with variable volume. Scheduled scaling (B) requires knowing the desired capacity in advance. Dynamic scaling (A) reacts after load hits. Lambda (D) is reactive and complex. (AWS Docs: Predictive Scaling).
</details>

---

**Q343.** A solutions architect is designing a company's disaster recovery (DR) architecture. The company has a MySQL database that runs on an Amazon EC2 instance in a private subnet with scheduled backup. The DR design needs to include multiple AWS Regions. Which solution will meet these requirements with the LEAST operational overhead?

- A. Migrate the MySQL database to multiple EC2 instances. Configure a standby EC2 instance in the DR Region. Turn on replication.
- B. Migrate the MySQL database to Amazon RDS. Use a Multi-AZ deployment. Turn on read replication for the primary DB instance in the different Availability Zones.
- C. Migrate the MySQL database to an Amazon Aurora global database. Host the primary DB cluster in the primary Region. Host the secondary DB cluster in the DR Region.
- D. Store the scheduled backup of the MySQL database in an Amazon S3 bucket that is configured for S3 Cross-Region Replication (CRR). Use the data backup to restore the database in the DR Region.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Aurora Global Database provides managed cross-Region replication with low RTO/RPO, minimal operational overhead. Option A (EC2) has high overhead. Option B (RDS Multi-AZ) is within one Region. Option D (backup/restore) has higher RTO. (AWS Docs: Aurora Global Database DR).
</details>

---

**Q344.** A company has a Java application that uses Amazon Simple Queue Service (Amazon SQS) to parse messages. The application cannot parse messages that are larger than 256 KB in size. The company wants to implement a solution to give the application the ability to parse messages as large as 50 MB. Which solution will meet these requirements with the FEWEST changes to the code?

- A. Use the Amazon SQS Extended Client Library for Java to host messages that are larger than 256 KB in Amazon S3.
- B. Use Amazon EventBridge to post large messages from the application instead of Amazon SQS.
- C. Change the limit in Amazon SQS to handle messages that are larger than 256 KB.
- D. Store messages that are larger than 256 KB in Amazon Elastic File System (Amazon EFS). Configure Amazon SQS to reference this location in the messages.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* The SQS Extended Client Library automatically stores large message payloads in S3 and passes a reference in SQS, requiring minimal code changes. SQS does not support larger messages natively (C). EventBridge (B) would require more changes. EFS (D) is overkill and not integrated. (AWS Docs: SQS Extended Client).
</details>

---

**Q345.** A company wants to restrict access to the content of one of its main web applications and to protect the content by using authorization techniques available on AWS. The company wants to implement a serverless architecture and an authentication solution for fewer than 100 users. The solution needs to integrate with the main web application and serve web content globally. The solution must also scale as the company's user base grows while providing the lowest login latency possible. Which solution will meet these requirements MOST cost-effectively?

- A. Use Amazon Cognito for authentication. Use Lambda@Edge for authorization. Use Amazon CloudFront to serve the web application globally.
- B. Use AWS Directory Service for Microsoft Active Directory for authentication. Use AWS Lambda for authorization. Use an Application Load Balancer to serve the web application globally.
- C. Use Amazon Cognito for authentication. Use AWS Lambda for authorization. Use Amazon S3 Transfer Acceleration to serve the web application globally.
- D. Use AWS Directory Service for Microsoft Active Directory for authentication. Use Lambda@Edge for authorization. Use AWS Elastic Beanstalk to serve the web application globally.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Amazon Cognito is cost-effective for <100 users (free tier). Lambda@Edge allows authorization logic at the edge, reducing latency. CloudFront serves content globally. This is serverless and scalable. Option B (Directory Service) is more expensive. Option C (S3 Transfer Acceleration) is for uploads, not serving content. Option D (Elastic Beanstalk) is not serverless. (AWS Docs: Cognito + Lambda@Edge).
</details>

---

**Q346.** A company has an aging network-attached storage (NAS) array in its data center. The NAS array presents SMB shares and NFS shares to client workstations. The company does not want to purchase a new NAS array. The company also does not want to incur the cost of renewing the NAS array's support contract. Some of the data is accessed frequently, but much of the data is inactive. A solutions architect needs to implement a solution that migrates the data to Amazon S3, uses S3 Lifecycle policies, and maintains the same look and feel for the client workstations. The solutions architect has identified AWS Storage Gateway as part of the solution. Which type of storage gateway should the solutions architect provision to meet these requirements?

- A. Volume Gateway
- B. Tape Gateway
- C. Amazon FSx File Gateway
- D. Amazon S3 File Gateway

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 File Gateway presents SMB and NFS shares to clients, stores data in S3, and supports S3 Lifecycle policies. Volume Gateway (A) is block storage. Tape Gateway (B) is for backup. FSx File Gateway (C) is for FSx, not S3. (AWS Docs: S3 File Gateway).
</details>

---

**Q347.** A company has an application that is running on Amazon EC2 instances. A solutions architect has standardized the company on a particular instance family and various instance sizes based on the current needs of the company. The company wants to maximize cost savings for the application over the next 3 years. The company needs to be able to change the instance family and sizes in the next 6 months based on application popularity and usage. Which solution will meet these requirements MOST cost-effectively?

- A. Compute Savings Plan
- B. EC2 Instance Savings Plan
- C. Zonal Reserved Instances
- D. Standard Reserved Instances

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* A Compute Savings Plan offers flexibility across instance families and sizes (including changing them) while providing significant discounts (up to 66%). EC2 Instance Savings Plan (B) is locked to a specific instance family. Zonal/Standard Reserved Instances (C, D) are less flexible. (AWS Docs: Compute Savings Plan Flexibility).
</details>

---

**Q348.** A company collects data from a large number of participants who use wearable devices. The company stores the data in an Amazon DynamoDB table and uses applications to analyze the data. The data workload is constant and predictable. The company wants to stay at or below its forecasted budget for DynamoDB. Which solution will meet these requirements MOST cost-effectively?

- A. Use provisioned mode and DynamoDB Standard-Infrequent Access (DynamoDB Standard-IA). Reserve capacity for the forecasted workload.
- B. Use provisioned mode. Specify the read capacity units (RCUs) and write capacity units (WCUs).
- C. Use on-demand mode. Set the read capacity units (RCUs) and write capacity units (WCUs) high enough to accommodate changes in the workload.
- D. Use on-demand mode. Specify the read capacity units (RCUs) and write capacity units (WCUs) with reserved capacity.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* For constant and predictable workload, provisioned mode is most cost-effective. You specify RCUs/WCUs based on forecasted workload. On-demand (C, D) is more expensive for steady workloads. Standard-IA (A) is for infrequent access. (AWS Docs: DynamoDB Provisioned Mode).
</details>

---

**Q349.** A company stores confidential data in an Amazon Aurora PostgreSQL database in the ap-southeast-3 Region. The database is encrypted with an AWS Key Management Service (AWS KMS) customer managed key. The company was recently acquired and must securely share a backup of the database with the acquiring company's AWS account in ap-southeast-3. What should a solutions architect do to meet these requirements?

- A. Create a database snapshot. Copy the snapshot to a new unencrypted snapshot. Share the new snapshot with the acquiring company's AWS account.
- B. Create a database snapshot. Add the acquiring company's AWS account to the KMS key policy. Share the snapshot with the acquiring company's AWS account.
- C. Create a database snapshot that uses a different AWS managed KMS key. Add the acquiring company's AWS account to the KMS key alias. Share the snapshot with the acquiring company's AWS account.
- D. Create a database snapshot. Download the database snapshot. Upload the database snapshot to an Amazon S3 bucket. Update the S3 bucket policy to allow access from the acquiring company's AWS account.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To share an encrypted snapshot, you must add the recipient account to the KMS key policy (allow decrypt), then share the snapshot. Do not make it unencrypted (A). Option C (different key) won't work. Option D (download/upload) is insecure and complex. (AWS Docs: Sharing Encrypted RDS Snapshots).
</details>

---

**Q350.** A company uses a 100 GB Amazon RDS for Microsoft SQL Server Single-AZ DB instance in the us-east-1 Region to store customer transactions. The company needs high availability and automatic recovery for the DB instance. The company must also run reports on the RDS database several times a year. The report process causes transactions to take longer than usual to post to the customers' accounts. The company needs a solution that will improve the performance of the report process. Which combination of steps will meet these requirements? (Choose two.)

- A. Modify the DB instance from a Single-AZ DB instance to a Multi-AZ deployment.
- B. Take a snapshot of the current DB instance. Restore the snapshot to a new RDS deployment in another Availability Zone.
- C. Create a read replica of the DB instance in a different Availability Zone. Point all requests for reports to the read replica.
- D. Migrate the database to RDS Custom.
- E. Use RDS Proxy to limit reporting requests to the maintenance window.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* Multi-AZ (A) provides high availability and automatic failover. A read replica (C) offloads reporting queries from the primary, improving transaction performance. Option B (snapshot restore) doesn't provide HA. Option D (RDS Custom) adds overhead. Option E (RDS Proxy) manages connections but doesn't offload reporting. (AWS Docs: RDS Multi-AZ, Read Replicas).
</details>

---

**Q351.** A company is moving its data management application to AWS. The company wants to transition to an event-driven architecture. The architecture needs to be more distributed and to use serverless concepts while performing the different aspects of the workflow. The company also wants to minimize operational overhead. Which solution will meet these requirements?

- A. Build out the workflow in AWS Glue. Use AWS Glue to invoke AWS Lambda functions to process the workflow steps.
- B. Build out the workflow in AWS Step Functions. Deploy the application on Amazon EC2 instances. Use Step Functions to invoke the workflow steps on the EC2 instances.
- C. Build out the workflow in Amazon EventBridge. Use EventBridge to invoke AWS Lambda functions on a schedule to process the workflow steps.
- D. Build out the workflow in AWS Step Functions. Use Step Functions to create a state machine. Use the state machine to invoke AWS Lambda functions to process the workflow steps.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Step Functions is designed for coordinating distributed, serverless workflows using Lambda functions. It supports event-driven execution and minimizes operational overhead. Glue (A) is for ETL. EC2 (B) is not serverless. EventBridge on schedule (C) is not event-driven for workflow steps. (AWS Docs: Step Functions with Lambda).
</details>

---

**Q352.** A company is designing the network for an online multi-player game. The game uses the UDP networking protocol and will be deployed in eight AWS Regions. The network architecture needs to minimize latency and packet loss to give end users a high-quality gaming experience. Which solution will meet these requirements?

- A. Setup a transit gateway in each Region. Create inter-Region peering attachments between each transit gateway.
- B. Set up AWS Global Accelerator with UDP listeners and endpoint groups in each Region.
- C. Set up Amazon CloudFront with UDP turned on. Configure an origin in each Region.
- D. Set up a VPC peering mesh between each Region. Turn on UDP for each VPC.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Global Accelerator supports UDP, provides static IPs, and routes traffic to the optimal Region via edge locations, minimizing latency and packet loss. CloudFront (C) does not support UDP origins. Transit gateway peering (A) and VPC peering mesh (D) are complex and do not provide global latency optimization. (AWS Docs: Global Accelerator UDP).
</details>

---

**Q353.** A company hosts a three-tier web application on Amazon EC2 instances in a single Availability Zone. The web application uses a self-managed MySQL database that is hosted on an EC2 instance to store data in an Amazon Elastic Block Store (Amazon EBS) volume. The MySQL database currently uses a 1 TB Provisioned IOPS SSD (io2) EBS volume. The company expects traffic of 1,000 IOPS for both reads and writes at peak traffic. The company wants to minimize any disruptions, stabilize performance, and reduce costs while retaining the capacity for double the IOPS. The company wants to move the database tier to a fully managed solution that is highly available and fault tolerant. Which solution will meet these requirements MOST cost-effectively?

- A. Use a Multi-AZ deployment of an Amazon RDS for MySQL DB instance with an io2 Block Express EBS volume.
- B. Use a Multi-AZ deployment of an Amazon RDS for MySQL DB instance with a General Purpose SSD (gp2) EBS volume.
- C. Use Amazon S3 Intelligent-Tiering access tiers.
- D. Use two large EC2 instances to host the database in active-passive mode.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A Multi-AZ RDS for MySQL DB instance provides high availability and fault tolerance. For 1,000 IOPS (peak), a gp2/gp3 volume is sufficient and more cost-effective than io2. Option A (io2) is overkill. Option C (S3) is not a database. Option D (EC2) is not fully managed. (AWS Docs: RDS Storage Types).
</details>

---

**Q354.** A company hosts a serverless application on AWS. The application uses Amazon API Gateway, AWS Lambda, and an Amazon RDS for PostgreSQL database. The company notices an increase in application errors that result from database connection timeouts during times of peak traffic or unpredictable traffic. The company needs a solution that reduces the application failures with the least amount of change to the code. What should a solutions architect do to meet these requirements?

- A. Reduce the Lambda concurrency rate.
- B. Enable RDS Proxy on the RDS DB instance.
- C. Resize the RDS DB instance class to accept more connections.
- D. Migrate the database to Amazon DynamoDB with on-demand scaling.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* RDS Proxy manages connection pooling, reduces connection overhead, and handles bursts of connections, preventing timeouts. This requires minimal code changes (just update endpoint). Option A (reduce concurrency) would worsen throughput. Option C (resize) may not solve connection spikes. Option D (DynamoDB) requires major changes. (AWS Docs: RDS Proxy).
</details>

---

**Q355.** A company is migrating an old application to AWS. The application runs a batch job every hour and is CPU intensive. The batch job takes 15 minutes on average with an on-premises server. The server has 64 virtual CPU (vCPU) and 512 GiB of memory. Which solution will run the batch job within 15 minutes with the LEAST operational overhead?

- A. Use AWS Lambda with functional scaling.
- B. Use Amazon Elastic Container Service (Amazon ECS) with AWS Fargate.
- C. Use Amazon Lightsail with AWS Auto Scaling.
- D. Use AWS Batch on Amazon EC2.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Batch is designed for batch jobs and can provision large EC2 instances (e.g., with 64 vCPU/512 GiB) to meet the performance requirement. Lambda (A) has limits (15 min timeout, lower memory). Fargate (B) has lower vCPU limits. Lightsail (C) is for simpler workloads. (AWS Docs: AWS Batch).
</details>

---

**Q356.** A company stores its data objects in Amazon S3 Standard storage. A solutions architect has found that 75% of the data is rarely accessed after 30 days. The company needs all the data to remain immediately accessible with the same high availability and resiliency, but the company wants to minimize storage costs. Which storage solution will meet these requirements?

- A. Move the data objects to S3 Glacier Deep Archive after 30 days.
- B. Move the data objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days.
- C. Move the data objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 30 days.
- D. Move the data objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) immediately.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Standard-IA is cheaper than S3 Standard, offers immediate accessibility, and provides high availability (multi-AZ). Glacier Deep Archive (A) has retrieval delays. One Zone-IA (C, D) is not resilient to AZ loss. (AWS Docs: S3 Standard-IA).
</details>

---

**Q357.** A gaming company is moving its public scoreboard from a data center to the AWS Cloud. The company uses Amazon EC2 Windows Server instances behind an Application Load Balancer to host its dynamic application. The company needs a highly available storage solution for the application. The application consists of static files and dynamic server-side code. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- A. Store the static files on Amazon S3. Use Amazon CloudFront to cache objects at the edge.
- B. Store the static files on Amazon S3. Use Amazon ElastiCache to cache objects at the edge.
- C. Store the server-side code on Amazon Elastic File System (Amazon EFS). Mount the EFS volume on each EC2 instance to share the files.
- D. Store the server-side code on Amazon FSx for Windows File Server. Mount the FSx for Windows File Server volume on each EC2 instance to share the files.
- E. Store the server-side code on a General Purpose SSD (gp2) Amazon Elastic Block Store (Amazon EBS) volume. Mount the EBS volume on each EC2 instance to share the files.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* Static files on S3 with CloudFront (A) for edge caching. Dynamic server-side code (Windows) on FSx for Windows File Server (D) for shared, highly available storage. EFS (C) is NFS, not Windows-native. EBS (E) cannot be shared across instances easily. ElastiCache (B) is for caching, not static files. (AWS Docs: FSx for Windows, CloudFront for Static).
</details>

---

**Q358.** A social media company runs its application on Amazon EC2 instances behind an Application Load Balancer (ALB). The ALB is the origin for an Amazon CloudFront distribution. The application has more than a billion images stored in an Amazon S3 bucket and processes thousands of images each second. The company wants to resize the images dynamically and serve appropriate formats to clients. Which solution will meet these requirements with the LEAST operational overhead?

- A. Install an external image management library on an EC2 instance. Use the image management library to process the images.
- B. Create a CloudFront origin request policy. Use the policy to automatically resize images and to serve the appropriate format based on the User-Agent HTTP header in the request.
- C. Use a Lambda@Edge function with an external image management library. Associate the Lambda@Edge function with the CloudFront behaviors that serve the images.
- D. Create a CloudFront response headers policy. Use the policy to automatically resize images and to serve the appropriate format based on the User-Agent HTTP header in the request.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Lambda@Edge can run image resizing logic at the edge, close to users, with minimal operational overhead (serverless). Origin request policies (B) and response headers policies (D) do not perform image processing. EC2 instance (A) has more overhead. (AWS Docs: Lambda@Edge for Image Resizing).
</details>

---

**Q359.** A hospital needs to store patient records in an Amazon S3 bucket. The hospital's compliance team must ensure that all protected health information (PHI) is encrypted in transit and at rest. The compliance team must administer the encryption key for data at rest. Which solution will meet these requirements?

- A. Create a public SSL/TLS certificate in AWS Certificate Manager (ACM). Associate the certificate with Amazon S3. Configure default encryption for each S3 bucket to use server-side encryption with AWS KMS keys (SSE-KMS). Assign the compliance team to manage the KMS keys.
- B. Use the aws:SecureTransport condition on S3 bucket policies to allow only encrypted connections over HTTPS (TLS). Configure default encryption for each S3 bucket to use server-side encryption with S3 managed encryption keys (SSE-S3). Assign the compliance team to manage the SSE-S3 keys.
- C. Use the aws:SecureTransport condition on S3 bucket policies to allow only encrypted connections over HTTPS (TLS). Configure default encryption for each S3 bucket to use server-side encryption with AWS KMS keys (SSE-KMS). Assign the compliance team to manage the KMS keys.
- D. Use the aws:SecureTransport condition on S3 bucket policies to allow only encrypted connections over HTTPS (TLS). Use Amazon Macie to protect the sensitive data that is stored in Amazon S3. Assign the compliance team to manage Macie.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* The `aws:SecureTransport` condition enforces encryption in transit (HTTPS). SSE-KMS with customer managed keys allows the compliance team to administer the keys for at-rest encryption. Option A (ACM cert for S3) is not needed; S3 uses HTTPS by default. Option B (SSE-S3) does not give the team key administration. Option D (Macie) is for data discovery, not encryption. (AWS Docs: S3 Encryption in Transit, SSE-KMS).
</details>

---

**Q360.** A company uses Amazon API Gateway to run a private gateway with two REST APIs in the same VPC. The BuyStock RESTful web service calls the CheckFunds RESTful web service to ensure that enough funds are available before a stock can be purchased. The company has noticed in the VPC flow logs that the BuyStock RESTful web service calls the CheckFunds RESTful web service over the internet instead of through the VPC. A solutions architect must implement a solution so that the APIs communicate through the VPC. Which solution will meet these requirements with the FEWEST changes to the code?

- A. Add an X-API-Key header in the HTTP header for authorization.
- B. Use an interface endpoint.
- C. Use a gateway endpoint.
- D. Add an Amazon Simple Queue Service (Amazon SQS) queue between the two REST APIs.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To keep traffic within the VPC when one API calls another, you can create an interface VPC endpoint for API Gateway. Then configure the caller API to use that endpoint instead of the public endpoint. This requires minimal code changes (just change the endpoint URL). Gateway endpoint (C) is for S3 and DynamoDB. (AWS Docs: API Gateway Interface Endpoint).
</details>

---

**Q361.** A company hosts a multiplayer gaming application on AWS. The company wants the application to read data with sub-millisecond latency and run one-time queries on historical data. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon RDS for data that is frequently accessed. Run a periodic custom script to export the data to an Amazon S3 bucket.
- B. Store the data directly in an Amazon S3 bucket. Implement an S3 Lifecycle policy to move older data to S3 Glacier Deep Archive for longterm storage. Run one-time queries on the data in Amazon S3 by using Amazon Athena.
- C. Use Amazon DynamoDB with DynamoDB Accelerator (DAX) for data that is frequently accessed. Export the data to an Amazon S3 bucket by using DynamoDB table export. Run one-time queries on the data in Amazon S3 by using Amazon Athena.
- D. Use Amazon DynamoDB for data that is frequently accessed. Turn on streaming to Amazon Kinesis Data Streams. Use Amazon Kinesis Data Firehose to read the data from Kinesis Data Streams. Store the records in an Amazon S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* DynamoDB with DAX provides sub-millisecond read latency. DynamoDB export to S3 (incremental) allows Athena to run one-time queries on historical data with minimal overhead. Option A (RDS) may not achieve sub-millisecond. Option B (S3 only) doesn't provide sub-millisecond. Option D (streaming) adds complexity. (AWS Docs: DAX, DynamoDB Export to S3).
</details>

---

**Q362.** A company uses a payment processing system that requires messages for a particular payment ID to be received in the same order that they were sent. Otherwise, the payments might be processed incorrectly. Which actions should a solutions architect take to meet this requirement? (Choose two.)

- A. Write the messages to an Amazon DynamoDB table with the payment ID as the partition key.
- B. Write the messages to an Amazon Kinesis data stream with the payment ID as the partition key.
- C. Write the messages to an Amazon ElastiCache for Memcached cluster with the payment ID as the key.
- D. Write the messages to an Amazon Simple Queue Service (Amazon SQS) queue. Set the message attribute to use the payment ID.
- E. Write the messages to an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Set the message group to use the payment ID.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* Kinesis Data Streams (B) with partition key = payment ID ensures messages with the same key go to the same shard and are ordered. SQS FIFO (E) with message group ID = payment ID guarantees FIFO delivery within the group. DynamoDB (A) does not guarantee order. Memcached (C) is not durable. SQS standard (D) does not guarantee order. (AWS Docs: Kinesis Ordering, SQS FIFO).
</details>

---

**Q363.** A company is building a game system that needs to send unique events to separate leaderboard, matchmaking, and authentication services concurrently. The company needs an AWS event-driven system that guarantees the order of the events. Which solution will meet these requirements?

- A. Amazon EventBridge event bus
- B. Amazon Simple Notification Service (Amazon SNS) FIFO topics
- C. Amazon Simple Notification Service (Amazon SNS) standard topics
- D. Amazon Simple Queue Service (Amazon SQS) FIFO queues

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* SNS FIFO topics support fan-out to multiple subscribers (leaderboard, matchmaking, authentication) while preserving the order of events within a message group. EventBridge (A) does not guarantee order. SNS standard (C) does not guarantee order. SQS FIFO (D) is for point-to-point, not fan-out. (AWS Docs: SNS FIFO Topics).
</details>

---

**Q364.** A hospital is designing a new application that gathers symptoms from patients. The hospital has decided to use Amazon Simple Queue Service (Amazon SQS) and Amazon Simple Notification Service (Amazon SNS) in the architecture. A solutions architect is reviewing the infrastructure design. Data must be encrypted at rest and in transit. Only authorized personnel of the hospital should be able to access the data. Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

- A. Turn on server-side encryption on the SQS components. Update the default key policy to restrict key usage to a set of authorized principals.
- B. Turn on server-side encryption on the SNS components by using an AWS Key Management Service (AWS KMS) customer managed key. Apply a key policy to restrict key usage to a set of authorized principals.
- C. Turn on encryption on the SNS components. Update the default key policy to restrict key usage to a set of authorized principals. Set a condition in the topic policy to allow only encrypted connections over TLS.
- D. Turn on server-side encryption on the SQS components by using an AWS Key Management Service (AWS KMS) customer managed key. Apply a key policy to restrict key usage to a set of authorized principals. Set a condition in the queue policy to allow only encrypted connections over TLS.
- E. Turn on server-side encryption on the SQS components by using an AWS Key Management Service (AWS KMS) customer managed key. Apply an IAM policy to restrict key usage to a set of authorized principals. Set a condition in the queue policy to allow only encrypted connections over TLS.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* Both SNS and SQS support server-side encryption using customer managed KMS keys with key policies to restrict access. Additionally, setting a condition for `aws:SecureTransport` ensures encryption in transit. Option A uses default key policy (not customer managed). Option C (default key policy). Option E uses IAM policy instead of key policy. (AWS Docs: SQS SSE, SNS SSE).
</details>

---

**Q365.** A company runs a web application that is backed by Amazon RDS. A new database administrator caused data loss by accidentally editing information in a database table. To help recover from this type of incident, the company wants the ability to restore the database to its state from 5 minutes before any change within the last 30 days. Which feature should the solutions architect include in the design to meet this requirement?

- A. Read replicas
- B. Manual snapshots
- C. Automated backups
- D. Multi-AZ deployments

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Automated backups in RDS provide point-in-time recovery (PITR), allowing you to restore to any point within the retention period (up to 35 days), down to second granularity. This meets the requirement of restoring to 5 minutes before any change. Read replicas (A) and Multi-AZ (D) are for HA, not PITR. Manual snapshots (B) are point-in-time but not continuous. (AWS Docs: RDS PITR).
</details>

---

**Q366.** A company's web application consists of an Amazon API Gateway API in front of an AWS Lambda function and an Amazon DynamoDB database. The Lambda function handles the business logic, and the DynamoDB table hosts the data. The application uses Amazon Cognito user pools to identify the individual users of the application. A solutions architect needs to update the application so that only users who have a subscription can access premium content. Which solution will meet this requirement with the LEAST operational overhead?

- A. Enable API caching and throttling on the API Gateway API.
- B. Set up AWS WAF on the API Gateway API. Create a rule to filter users who have a subscription.
- C. Apply fine-grained IAM permissions to the premium content in the DynamoDB table.
- D. Implement API usage plans and API keys to limit the access of users who do not have a subscription.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* API Gateway usage plans with API keys allow you to control access based on subscription tiers. This is a standard, managed feature with low overhead. Cognito (already used) can be integrated with API keys via custom authorizers, but usage plans are simpler. Option A (caching/throttling) doesn't control access. Option B (WAF) is for attacks. Option C (DynamoDB IAM) is more complex. (AWS Docs: API Gateway Usage Plans).
</details>

---

**Q367.** A company is using Amazon Route 53 latency-based routing to route requests to its UDP-based application for users around the world. The application is hosted on redundant servers in the company's on-premises data centers in the United States, Asia, and Europe. The company's compliance requirements state that the application must be hosted on premises. The company wants to improve the performance and availability of the application. What should a solutions architect do to meet these requirements?

- A. Configure three Network Load Balancers (NLBs) in the three AWS Regions to address the on-premises endpoints. Create an accelerator by using AWS Global Accelerator, and register the NLBs as its endpoints. Provide access to the application by using a CNAME that points to the accelerator DNS.
- B. Configure three Application Load Balancers (ALBs) in the three AWS Regions to address the on-premises endpoints. Create an accelerator by using AWS Global Accelerator, and register the ALBs as its endpoints. Provide access to the application by using a CNAME that points to the accelerator DNS.
- C. Configure three Network Load Balancers (NLBs) in the three AWS Regions to address the on-premises endpoints. In Route 53, create a latency-based record that points to the three NLBs, and use it as an origin for an Amazon CloudFront distribution. Provide access to the application by using a CNAME that points to the CloudFront DNS.
- D. Configure three Application Load Balancers (ALBs) in the three AWS Regions to address the on-premises endpoints. In Route 53, create a latency-based record that points to the three ALBs, and use it as an origin for an Amazon CloudFront distribution. Provide access to the application by using a CNAME that points to the CloudFront DNS.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Global Accelerator supports UDP and can use NLBs as endpoints. NLBs can have on-premises targets via VPN/Direct Connect. This improves performance and availability with global anycast IPs. ALBs (B, D) do not support UDP. CloudFront (C, D) does not support UDP for origins. (AWS Docs: Global Accelerator with NLB and On-Premises).
</details>

---

**Q368.** A solutions architect wants all new users to have specific complexity requirements and mandatory rotation periods for IAM user passwords. What should the solutions architect do to accomplish this?

- A. Set an overall password policy for the entire AWS account.
- B. Set a password policy for each IAM user in the AWS account.
- C. Use third-party vendor software to set password requirements.
- D. Attach an Amazon CloudWatch rule to the Create_news user event to set the password with the appropriate requirements.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* IAM allows you to set an account password policy that applies to all IAM users, including password complexity and rotation requirements. This is the simplest solution. Options B, C, D are not standard or are overly complex. (AWS Docs: IAM Password Policy).
</details>

---

**Q369.** A company has migrated an application to Amazon EC2 Linux instances. One of these EC2 instances runs several 1-hour tasks on a schedule. These tasks were written by different teams and have no common programming language. The company is concerned about performance and scalability while these tasks run on a single instance. A solutions architect needs to implement a solution to resolve these concerns. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Batch to run the tasks as jobs. Schedule the jobs by using Amazon EventBridge (Amazon CloudWatch Events).
- B. Convert the EC2 instance to a container. Use AWS App Runner to create the container on demand to run the tasks as jobs.
- C. Copy the tasks into AWS Lambda functions. Schedule the Lambda functions by using Amazon EventBridge (Amazon CloudWatch Events).
- D. Create an Amazon Machine Image (AMI) of the EC2 instance that runs the tasks. Create an Auto Scaling group with the AMI to run multiple copies of the instance.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Batch can run jobs (tasks) of any language in a managed environment, with automatic scaling and scheduling via EventBridge. This requires minimal changes (just containerize the tasks). Option B (App Runner) is for web apps. Option C (Lambda) has timeouts and language limitations. Option D (Auto Scaling) would run full instances, not just tasks. (AWS Docs: AWS Batch).
</details>

---

**Q370.** A company runs a public three-tier web application in a VPC. The application runs on Amazon EC2 instances across multiple Availability Zones. The EC2 instances that run in private subnets need to communicate with a license server over the internet. The company needs a managed solution that minimizes operational maintenance. Which solution meets these requirements?

- A. Provision a NAT instance in a public subnet. Modify each private subnet's route table with a default route that points to the NAT instance.
- B. Provision a NAT instance in a private subnet. Modify each private subnet's route table with a default route that points to the NAT instance.
- C. Provision a NAT gateway in a public subnet. Modify each private subnet's route table with a default route that points to the NAT gateway.
- D. Provision a NAT gateway in a private subnet. Modify each private subnet's route table with a default route that points to the NAT gateway.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* NAT gateway is a managed service (minimal maintenance) that allows private subnet instances to access the internet. It must be placed in a public subnet. NAT instances (A, B) require maintenance. (AWS Docs: NAT Gateway).
</details>

---

**Q371.** A company needs to create an Amazon Elastic Kubernetes Service (Amazon EKS) cluster to host a digital media streaming application. The EKS cluster will use a managed node group that is backed by Amazon Elastic Block Store (Amazon EBS) volumes for storage. The company must encrypt all data at rest by using a customer managed key that is stored in AWS Key Management Service (AWS KMS). Which combination of actions will meet this requirement with the LEAST operational overhead? (Choose two.)

- A. Use a Kubernetes plugin that uses the customer managed key to perform data encryption.
- B. After creation of the EKS cluster, locate the EBS volumes. Enable encryption by using the customer managed key.
- C. Enable EBS encryption by default in the AWS Region where the EKS cluster will be created. Select the customer managed key as the default key.
- D. Create the EKS cluster. Create an IAM role that has a policy that grants permission to the customer managed key. Associate the role with the EKS cluster.
- E. Store the customer managed key as a Kubernetes secret in the EKS cluster. Use the customer managed key to encrypt the EBS volumes.

<details>
<summary>Show Answer</summary>

**Answer: C, D**

*Explanation:* Enable EBS encryption by default in the Region (C) using the customer managed key ensures all EBS volumes (including those created by EKS) are encrypted. Create an IAM role for the EKS cluster with permissions to use the KMS key (D). Option A (plugin) and E (secret) add overhead. Option B (post-creation) is not automatic. (AWS Docs: EBS Encryption by Default, EKS with KMS).
</details>

---

**Q372.** A company wants to migrate an Oracle database to AWS. The database consists of a single table that contains millions of geographic information systems (GIS) images that are high resolution and are identified by a geographic code. When a natural disaster occurs, tens of thousands of images get updated every few minutes. Each geographic code has a single image or row that is associated with it. The company wants a solution that is highly available and scalable during such events. Which solution meets these requirements MOST cost-effectively?

- A. Store the images and geographic codes in a database table. Use Oracle running on an Amazon RDS Multi-AZ DB instance.
- B. Store the images in Amazon S3 buckets. Use Amazon DynamoDB with the geographic code as the key and the image S3 URL as the value.
- C. Store the images and geographic codes in an Amazon DynamoDB table. Configure DynamoDB Accelerator (DAX) during times of high load.
- D. Store the images in Amazon S3 buckets. Store geographic codes and image S3 URLs in a database table. Use Oracle running on an Amazon RDS Multi-AZ DB instance.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* This separates storage: S3 for large images (cost-effective, scalable) and DynamoDB for metadata (highly available, scalable). DynamoDB can handle high update rates. Option A (RDS Oracle) is expensive and less scalable. Option C (DynamoDB for images) is inefficient (large binary objects). Option D (RDS for metadata) is less scalable than DynamoDB. (AWS Docs: S3 + DynamoDB for Metadata).
</details>

---

**Q373.** A company has an application that collects data from IoT sensors on automobiles. The data is streamed and stored in Amazon S3 through Amazon Kinesis Data Firehose. The data produces trillions of S3 objects each year. Each morning, the company uses the data from the previous 30 days to retrain a suite of machine learning (ML) models. Four times each year, the company uses the data from the previous 12 months to perform analysis and train other ML models. The data must be available with minimal delay for up to 1 year. After 1 year, the data must be retained for archival purposes. Which storage solution meets these requirements MOST cost-effectively?

- A. Use the S3 Intelligent-Tiering storage class. Create an S3 Lifecycle policy to transition objects to S3 Glacier Deep Archive after 1 year.
- B. Use the S3 Intelligent-Tiering storage class. Configure S3 Intelligent-Tiering to automatically move objects to S3 Glacier Deep Archive after 1 year.
- C. Use the S3 Standard-Infrequent Access (S3 Standard-IA) storage class. Create an S3 Lifecycle policy to transition objects to S3 Glacier Deep Archive after 1 year.
- D. Use the S3 Standard storage class. Create an S3 Lifecycle policy to transition objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days, and then to S3 Glacier Deep Archive after 1 year.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Intelligent-Tiering automatically moves objects between frequent and infrequent tiers based on access patterns, optimizing cost for data accessed daily (30-day) and quarterly (12-month). A lifecycle policy moves to Glacier Deep Archive after 1 year. Option D (Standard -> IA -> Deep) has more management but Intelligent-Tiering is simpler. Option C (Standard-IA only) is not optimal for frequently accessed data. (AWS Docs: Intelligent-Tiering for ML Data).
</details>

---

**Q374.** A company is running several business applications in three separate VPCs within the us-east-1 Region. The applications must be able to communicate between VPCs. The applications also must be able to consistently send hundreds of gigabytes of data each day to a latency-sensitive application that runs in a single on-premises data center. A solutions architect needs to design a network connectivity solution that maximizes cost-effectiveness. Which solution meets these requirements?

- A. Configure three AWS Site-to-Site VPN connections from the data center to AWS. Establish connectivity by configuring one VPN connection for each VPC.
- B. Launch a third-party virtual network appliance in each VPC. Establish an IPsec VPN tunnel between the data center and each virtual appliance.
- C. Set up three AWS Direct Connect connections from the data center to a Direct Connect gateway in us-east-1. Establish connectivity by configuring each VPC to use one of the Direct Connect connections.
- D. Set up one AWS Direct Connect connection from the data center to AWS. Create a transit gateway, and attach each VPC to the transit gateway. Establish connectivity between the Direct Connect connection and the transit gateway.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* A transit gateway centralizes connectivity between VPCs and on-premises via a single Direct Connect connection, reducing cost and complexity. Options A and B (VPN) may not provide consistent high throughput for latency-sensitive apps. Option C (three DX connections) is expensive. (AWS Docs: Transit Gateway with Direct Connect).
</details>

---

**Q375.** An ecommerce company is building a distributed application that involves several serverless functions and AWS services to complete order-processing tasks. These tasks require manual approvals as part of the workflow. A solutions architect needs to design an architecture for the order-processing application. The solution must be able to combine multiple AWS Lambda functions into responsive serverless applications. The solution also must orchestrate data and services that run on Amazon EC2 instances, containers, or on-premises servers. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Step Functions to build the application.
- B. Integrate all the application components in an AWS Glue job.
- C. Use Amazon Simple Queue Service (Amazon SQS) to build the application.
- D. Use AWS Lambda functions and Amazon EventBridge events to build the application.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Step Functions is designed to orchestrate serverless functions (Lambda), EC2, containers, and on-premises tasks, and supports human approval steps. This minimizes operational overhead. Glue (B) is for ETL. SQS (C) alone cannot orchestrate workflows. Lambda + EventBridge (D) can work but Step Functions is purpose-built. (AWS Docs: Step Functions for Order Processing).
</details>

---

**Q376.** A company has launched an Amazon RDS for MySQL DB instance. Most of the connections to the database come from serverless applications. Application traffic to the database changes significantly at random intervals. At times of high demand, users report that their applications experience database connection rejection errors. Which solution will resolve this issue with the LEAST operational overhead?

- A. Create a proxy in RDS Proxy. Configure the users' applications to use the DB instance through RDS Proxy.
- B. Deploy Amazon ElastiCache for Memcached between the users' applications and the DB instance.
- C. Migrate the DB instance to a different instance class that has higher I/O capacity. Configure the users' applications to use the new DB instance.
- D. Configure Multi-AZ for the DB instance. Configure the users' applications to switch between the DB instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* RDS Proxy manages connection pooling, reduces connection overhead, and handles bursty traffic from serverless apps, preventing connection rejection errors. ElastiCache (B) caches data, not connections. Scaling instance class (C) may not solve connection spikes. Multi-AZ (D) provides HA, not connection management. (AWS Docs: RDS Proxy).
</details>

---

**Q377.** A company recently deployed a new auditing system to centralize information about operating system versions, patching, and installed software for Amazon EC2 instances. A solutions architect must ensure all instances provisioned through EC2 Auto Scaling groups successfully send reports to the auditing system as soon as they are launched and terminated. Which solution achieves these goals MOST efficiently?

- A. Use a scheduled AWS Lambda function and run a script remotely on all EC2 instances to send data to the audit system.
- B. Use EC2 Auto Scaling lifecycle hooks to run a custom script to send data to the audit system when instances are launched and terminated.
- C. Use an EC2 Auto Scaling launch configuration to run a custom script through user data to send data to the audit system when instances are launched and terminated.
- D. Run a custom script on the instance operating system to send data to the audit system. Configure the script to be invoked by the EC2 Auto Scaling group when the instance starts and is terminated.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Lifecycle hooks allow you to perform custom actions (e.g., send report to audit system) before an instance is fully in-service (launch) or before it is terminated. This is the most efficient and reliable method. User data (C) runs only at launch, not termination. Scheduled Lambda (A) may miss instances. Option D is not a standard ASG feature. (AWS Docs: Auto Scaling Lifecycle Hooks).
</details>

---

**Q378.** A company is developing a real-time multiplayer game that uses UDP for communications between the client and servers in an Auto Scaling group. Spikes in demand are anticipated during the day, so the game server platform must adapt accordingly. Developers want to store gamer scores and other non-relational data in a database solution that will scale without intervention. Which solution should a solutions architect recommend?

- A. Use Amazon Route 53 for traffic distribution and Amazon Aurora Serverless for data storage.
- B. Use a Network Load Balancer for traffic distribution and Amazon DynamoDB on-demand for data storage.
- C. Use a Network Load Balancer for traffic distribution and Amazon Aurora Global Database for data storage.
- D. Use an Application Load Balancer for traffic distribution and Amazon DynamoDB global tables for data storage.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* NLB supports UDP traffic. DynamoDB on-demand scales automatically for non-relational data (gamer scores). Aurora Serverless (A) is relational, not ideal. Aurora Global Database (C) is relational and for multi-region. ALB (D) does not support UDP. (AWS Docs: NLB for UDP, DynamoDB On-Demand).
</details>

---

**Q379.** A company hosts a frontend application that uses an Amazon API Gateway API backend that is integrated with AWS Lambda. When the API receives requests, the Lambda function loads many libraries. Then the Lambda function connects to an Amazon RDS database, processes the data, and returns the data to the frontend application. The company wants to ensure that response latency is as low as possible for all its users with the fewest number of changes to the company's operations. Which solution will meet these requirements?

- A. Establish a connection between the frontend application and the database to make queries faster by bypassing the API.
- B. Configure provisioned concurrency for the Lambda function that handles the requests.
- C. Cache the results of the queries in Amazon S3 for faster retrieval of similar datasets.
- D. Increase the size of the database to increase the number of connections Lambda can establish at one time.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Provisioned concurrency keeps Lambda functions initialized, eliminating cold start latency caused by loading libraries and establishing connections. This directly reduces response latency. Option A (bypass API) is insecure. Option C (S3 caching) adds complexity and may not reduce first request latency. Option D (scale database) doesn't address Lambda initialization. (AWS Docs: Lambda Provisioned Concurrency).
</details>

---

**Q380.** A company is migrating its on-premises workload to the AWS Cloud. The company already uses several Amazon EC2 instances and Amazon RDS DB instances. The company wants a solution that automatically starts and stops the EC2 instances and DB instances outside of business hours. The solution must minimize cost and infrastructure maintenance. Which solution will meet these requirements?

- A. Scale the EC2 instances by using elastic resize. Scale the DB instances to zero outside of business hours.
- B. Explore AWS Marketplace for partner solutions that will automatically start and stop the EC2 instances and DB instances on a schedule.
- C. Launch another EC2 instance. Configure a crontab schedule to run shell scripts that will start and stop the existing EC2 instances and DB instances on a schedule.
- D. Create an AWS Lambda function that will start and stop the EC2 instances and DB instances. Configure Amazon EventBridge to invoke the Lambda function on a schedule.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Using Lambda and EventBridge is a serverless, cost-effective way to schedule start/stop of EC2 and RDS instances with minimal maintenance. Option A (elastic resize) doesn't stop instances. Option B (Marketplace) may have cost. Option C (EC2 with crontab) adds maintenance overhead. (AWS Docs: Start/Stop Instances with Lambda).
</details>

---

**Q381.** A company hosts a three-tier web application that includes a PostgreSQL database. The database stores the metadata from documents. The company searches the metadata for key terms to retrieve documents that the company reviews in a report each month. The documents are stored in Amazon S3. The documents are usually written only once, but they are updated frequently. The reporting process takes a few hours with the use of relational queries. The reporting process must not prevent any document modifications or the addition of new documents. A solutions architect needs to implement a solution to speed up the reporting process. Which solution will meet these requirements with the LEAST amount of change to the application code?

- A. Set up a new Amazon DocumentDB (with MongoDB compatibility) cluster that includes a read replica. Scale the read replica to generate the reports.
- B. Set up a new Amazon Aurora PostgreSQL DB cluster that includes an Aurora Replica. Issue queries to the Aurora Replica to generate the reports.
- C. Set up a new Amazon RDS for PostgreSQL Multi-AZ DB instance. Configure the reporting module to query the secondary RDS node so that the reporting module does not affect the primary node.
- D. Set up a new Amazon DynamoDB table to store the documents. Use a fixed write capacity to support new document entries. Automatically scale the read capacity to support the reports.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Aurora Replicas offload read-only reporting queries from the primary writer, preventing blocking of modifications/additions. This requires minimal code changes (just point reports to replica endpoint). Option A (DocumentDB) is not PostgreSQL-compatible. Option C (RDS Multi-AZ) secondary node is not readable. Option D (DynamoDB) requires major changes. (AWS Docs: Aurora Replicas).
</details>

---

**Q382.** A company has a three-tier application on AWS that ingests sensor data from its users' devices. The traffic flows through a Network Load Balancer (NLB), then to Amazon EC2 instances for the web tier, and finally to EC2 instances for the application tier. The application tier makes calls to a database. What should a solutions architect do to improve the security of the data in transit?

- A. Configure a TLS listener. Deploy the server certificate on the NLB.
- B. Configure AWS Shield Advanced. Enable AWS WAF on the NLB.
- C. Change the load balancer to an Application Load Balancer (ALB). Enable AWS WAF on the ALB.
- D. Encrypt the Amazon Elastic Block Store (Amazon EBS) volume on the EC2 instances by using AWS Key Management Service (AWS KMS).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Configuring TLS on the NLB encrypts data in transit between client and NLB. NLB supports TLS termination. Option B (Shield/WAF) is for DDoS/web exploits, not encryption. Option C (ALB) is not required. Option D (EBS encryption) is at rest, not in transit. (AWS Docs: NLB TLS Listeners).
</details>

---

**Q383.** A company is planning to migrate a commercial off-the-shelf application from its on-premises data center to AWS. The software has a software licensing model using sockets and cores with predictable capacity and uptime requirements. The company wants to use its existing licenses, which were purchased earlier this year. Which Amazon EC2 pricing option is the MOST cost-effective?

- A. Dedicated Reserved Hosts
- B. Dedicated On-Demand Hosts
- C. Dedicated Reserved Instances
- D. Dedicated On-Demand Instances

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Dedicated Hosts allow you to use your existing per-socket/core software licenses. Reserved Hosts provide a discount (up to 70%) compared to On-Demand Hosts. This is most cost-effective for predictable capacity. Options B (On-Demand Hosts) are more expensive. Options C/D (Dedicated Instances) do not provide visibility into sockets/cores for license compliance. (AWS Docs: Dedicated Hosts for BYOL).
</details>

---

**Q384.** A company runs an application on Amazon EC2 Linux instances across multiple Availability Zones. The application needs a storage layer that is highly available and Portable Operating System Interface (POSIX)-compliant. The storage layer must provide maximum data durability and must be shareable across the EC2 instances. The data in the storage layer will be accessed frequently for the first 30 days and will be accessed infrequently after that time. Which solution will meet these requirements MOST cost-effectively?

- A. Use the Amazon S3 Standard storage class. Create an S3 Lifecycle policy to move infrequently accessed data to S3 Glacier.
- B. Use the Amazon S3 Standard storage class. Create an S3 Lifecycle policy to move infrequently accessed data to S3 Standard-Infrequent Access (S3 Standard-IA).
- C. Use the Amazon Elastic File System (Amazon EFS) Standard storage class. Create a lifecycle management policy to move infrequently accessed data to EFS Standard-Infrequent Access (EFS Standard-IA).
- D. Use the Amazon Elastic File System (Amazon EFS) One Zone storage class. Create a lifecycle management policy to move infrequently accessed data to EFS One Zone-Infrequent Access (EFS One Zone-IA).

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* EFS is POSIX-compliant, shareable across Linux instances, and highly available (multi-AZ). EFS Lifecycle Management automatically moves infrequently accessed data to EFS Standard-IA, reducing cost. S3 (A, B) is not POSIX-compliant (object storage). One Zone (D) is not highly available. (AWS Docs: EFS Lifecycle Management).
</details>

---

**Q385.** A solutions architect is creating a new VPC design. There are two public subnets for the load balancer, two private subnets for web servers, and two private subnets for MySQL. The web servers use only HTTPS. The solutions architect has already created a security group for the load balancer allowing port 443 from 0.0.0.0/0. Company policy requires that each resource has the least access required to still be able to perform its tasks. Which additional configuration strategy should the solutions architect use to meet these requirements?

- A. Create a security group for the web servers and allow port 443 from 0.0.0.0/0. Create a security group for the MySQL servers and allow port 3306 from the web servers security group.
- B. Create a network ACL for the web servers and allow port 443 from 0.0.0.0/0. Create a network ACL for the MySQL servers and allow port 3306 from the web servers security group.
- C. Create a security group for the web servers and allow port 443 from the load balancer. Create a security group for the MySQL servers and allow port 3306 from the web servers security group.
- D. Create a network ACL for the web servers and allow port 443 from the load balancer. Create a network ACL for the MySQL servers and allow port 3306 from the web servers security group.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Web servers should only accept traffic from the load balancer (not directly from internet). MySQL servers should only accept traffic from the web server security group. Security groups are stateful and support referencing other security groups. NACLs (B, D) are stateless and less precise. (AWS Docs: Security Group Referencing).
</details>

---

**Q386.** An ecommerce company is running a multi-tier application on AWS. The front-end and backend tiers both run on Amazon EC2, and the database runs on Amazon RDS for MySQL. The backend tier communicates with the RDS instance. There are frequent calls to return identical datasets from the database that are causing performance slowdowns. Which action should be taken to improve the performance of the backend?

- A. Implement Amazon SNS to store the database calls.
- B. Implement Amazon ElastiCache to cache the large datasets.
- C. Implement an RDS for MySQL read replica to cache database calls.
- D. Implement Amazon Kinesis Data Firehose to stream the calls to the database.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* ElastiCache caches frequent query results, reducing load on the database and improving performance. Read replicas (C) offload reads but do not cache; each read still hits the database. SNS (A) is for notifications. Kinesis (D) is for streaming. (AWS Docs: ElastiCache for Caching).
</details>

---

**Q387.** A new employee has joined a company as a deployment engineer. The deployment engineer will be using AWS CloudFormation templates to create multiple AWS resources. A solutions architect wants the deployment engineer to perform job activities while following the principle of least privilege. Which combination of actions should the solutions architect take to accomplish this goal? (Choose two.)

- A. Have the deployment engineer use AWS account root user credentials for performing AWS CloudFormation stack operations.
- B. Create a new IAM user for the deployment engineer and add the IAM user to a group that has the PowerUsers IAM policy attached.
- C. Create a new IAM user for the deployment engineer and add the IAM user to a group that has the AdministratorAccess IAM policy attached.
- D. Create a new IAM user for the deployment engineer and add the IAM user to a group that has an IAM policy that allows AWS CloudFormation actions only.
- E. Create an IAM role for the deployment engineer to explicitly define the permissions specific to the AWS CloudFormation stack and launch stacks using that IAM role.

<details>
<summary>Show Answer</summary>

**Answer: D, E**

*Explanation:* Create an IAM user with least privilege (only CloudFormation actions) and add to a group with that policy (D). Alternatively, use an IAM role that the engineer can assume, which defines specific permissions for the stack (E). Option A (root) is insecure. Options B (PowerUsers) and C (Admin) are too permissive. (AWS Docs: Least Privilege for CloudFormation).
</details>

---

**Q388.** A company is deploying a two-tier web application in a VPC. The web tier is using an Amazon EC2 Auto Scaling group with public subnets that span multiple Availability Zones. The database tier consists of an Amazon RDS for MySQL DB instance in separate private subnets. The web tier requires access to the database to retrieve product information. The web application is not working as intended. The web application reports that it cannot connect to the database. The database is confirmed to be up and running. All configurations for the network ACLs, security groups, and route tables are still in their default states. What should a solutions architect recommend to fix the application?

- A. Add an explicit rule to the private subnet's network ACL to allow traffic from the web tier's EC2 instances.
- B. Add a route in the VPC route table to allow traffic between the web tier's EC2 instances and the database tier.
- C. Deploy the web tier's EC2 instances and the database tier's RDS instance into two separate VPCs, and configure VPC peering.
- D. Add an inbound rule to the security group of the database tier's RDS instance to allow traffic from the web tier's security group.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Default security groups block all inbound traffic. You need to add an inbound rule to the database security group allowing traffic from the web tier's security group. Default route tables and NACLs allow all traffic, so no changes needed there. (AWS Docs: Security Group Rules).
</details>

---

**Q389.** A company has a large dataset for its online advertising business stored in an Amazon RDS for MySQL DB instance in a single Availability Zone. The company wants business reporting queries to run without impacting the write operations to the production DB instance. Which solution meets these requirements?

- A. Deploy RDS read replicas to process the business reporting queries.
- B. Scale out the DB instance horizontally by placing it behind an Elastic Load Balancer.
- C. Scale up the DB instance to a larger instance type to handle write operations and queries.
- D. Deploy the DB instance in multiple Availability Zones to process the business reporting queries.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Read replicas allow you to offload read-only reporting queries without impacting write operations on the primary. Option B (ELB) is not for databases. Option C (scaling up) may not isolate impact. Option D (Multi-AZ) is for HA, not read offloading. (AWS Docs: RDS Read Replicas).
</details>

---

**Q390.** A company hosts a three-tier ecommerce application on a fleet of Amazon EC2 instances. The instances run in an Auto Scaling group behind an Application Load Balancer (ALB). All ecommerce data is stored in an Amazon RDS for MariaDB Multi-AZ DB instance. The company wants to optimize customer session management during transactions. The application must store session data durably. Which solutions will meet these requirements? (Choose two.)

- A. Turn on the sticky sessions feature (session affinity) on the ALB.
- B. Use an Amazon DynamoDB table to store customer session information.
- C. Deploy an Amazon Cognito user pool to manage user session information.
- D. Deploy an Amazon ElastiCache for Redis cluster to store customer session information.
- E. Use AWS Systems Manager Application Manager in the application to manage user session information.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* Durable session storage can be achieved with DynamoDB (B) or ElastiCache for Redis (D) with persistence enabled. Sticky sessions (A) are not durable (instance failure loses session). Cognito (C) is for authentication. Systems Manager (E) is for management. (AWS Docs: Session Storage Options).
</details>

---

**Q391.** A company needs a backup strategy for its three-tier stateless web application. The web application runs on Amazon EC2 instances in an Auto Scaling group with a dynamic scaling policy that is configured to respond to scaling events. The database tier runs on Amazon RDS for PostgreSQL. The web application does not require temporary local storage on the EC2 instances. The company's recovery point objective (RPO) is 2 hours. The backup strategy must maximize scalability and optimize resource utilization for this environment. Which solution will meet these requirements?

- A. Take snapshots of Amazon Elastic Block Store (Amazon EBS) volumes of the EC2 instances and database every 2 hours to meet the RPO.
- B. Configure a snapshot lifecycle policy to take Amazon Elastic Block Store (Amazon EBS) snapshots. Enable automated backups in Amazon RDS to meet the RPO.
- C. Retain the latest Amazon Machine Images (AMIs) of the web and application tiers. Enable automated backups in Amazon RDS and use point-in-time recovery to meet the RPO.
- D. Take snapshots of Amazon Elastic Block Store (Amazon EBS) volumes of the EC2 instances every 2 hours. Enable automated backups in Amazon RDS and use point-in-time recovery to meet the RPO.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* For stateless web tier, retaining the latest AMI is sufficient (no need for frequent snapshots). RDS automated backups with point-in-time recovery meet the 2-hour RPO. This optimizes resource utilization (no unnecessary EBS snapshots). Options A, B, D include unnecessary EBS snapshots for stateless instances. (AWS Docs: Backup for Stateless Apps).
</details>

---

**Q392.** A company wants to deploy a new public web application on AWS. The application includes a web server tier that uses Amazon EC2 instances. The application also includes a database tier that uses an Amazon RDS for MySQL DB instance. The application must be secure and accessible for global customers that have dynamic IP addresses. How should a solutions architect configure the security groups to meet these requirements?

- A. Configure the security group for the web servers to allow inbound traffic on port 443 from 0.0.0.0/0. Configure the security group for the DB instance to allow inbound traffic on port 3306 from the security group of the web servers.
- B. Configure the security group for the web servers to allow inbound traffic on port 443 from the IP addresses of the customers. Configure the security group for the DB instance to allow inbound traffic on port 3306 from the security group of the web servers.
- C. Configure the security group for the web servers to allow inbound traffic on port 443 from the IP addresses of the customers. Configure the security group for the DB instance to allow inbound traffic on port 3306 from the IP addresses of the customers.
- D. Configure the security group for the web servers to allow inbound traffic on port 443 from 0.0.0.0/0. Configure the security group for the DB instance to allow inbound traffic on port 3306 from 0.0.0.0/0.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Web servers must allow HTTPS from anywhere (0.0.0.0/0) because customers have dynamic IPs. The database security group should only allow traffic from the web server security group, not from anywhere. Option B/C require knowing customer IPs (impossible). Option D exposes the database to the internet. (AWS Docs: Security Group Best Practices).
</details>

---

**Q393.** A payment processing company records all voice communication with its customers and stores the audio files in an Amazon S3 bucket. The company needs to capture the text from the audio files. The company must remove from the text any personally identifiable information (PII) that belongs to customers. What should a solutions architect do to meet these requirements?

- A. Process the audio files by using Amazon Kinesis Video Streams. Use an AWS Lambda function to scan for known PII patterns.
- B. When an audio file is uploaded to the S3 bucket, invoke an AWS Lambda function to start an Amazon Textract task to analyze the call recordings.
- C. Configure an Amazon Transcribe transcription job with PII redaction turned on. When an audio file is uploaded to the S3 bucket, invoke an AWS Lambda function to start the transcription job. Store the output in a separate S3 bucket.
- D. Create an Amazon Connect contact flow that ingests the audio files with transcription turned on. Embed an AWS Lambda function to scan for known PII patterns. Use Amazon EventBridge to start the contact flow when an audio file is uploaded to the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon Transcribe converts speech to text and has a built-in PII redaction feature. This is the most direct solution. Textract (B) is for documents. Kinesis Video Streams (A) is for live video. Connect (D) is a contact center service. (AWS Docs: Amazon Transcribe PII Redaction).
</details>

---

**Q394.** A company is running a multi-tier ecommerce web application in the AWS Cloud. The application runs on Amazon EC2 instances with an Amazon RDS for MySQL Multi-AZ DB instance. Amazon RDS is configured with the latest generation DB instance with 2,000 GB of storage in a General Purpose SSD (gp3) Amazon Elastic Block Store (Amazon EBS) volume. The database performance affects the application during periods of high demand. A database administrator analyzes the logs in Amazon CloudWatch Logs and discovers that the application performance always degrades when the number of read and write IOPS is higher than 20,000. What should a solutions architect do to improve the application performance?

- A. Replace the volume with a magnetic volume.
- B. Increase the number of IOPS on the gp3 volume.
- C. Replace the volume with a Provisioned IOPS SSD (io2) volume.
- D. Replace the 2,000 GB gp3 volume with two 1,000 GB gp3 volumes.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* gp3 volumes have a baseline IOPS (up to 16,000) and you can provision additional IOPS up to 20,000. To exceed 20,000 IOPS, you need io2 (Provisioned IOPS) volumes which can go much higher. Option B (increase IOPS on gp3) max is 20,000. Option A (magnetic) is worse. Option D (splitting) doesn't help a single instance. (AWS Docs: EBS Volume Types).
</details>

---

**Q395.** An IAM user made several configuration changes to AWS resources in their company's account during a production deployment last week. A solutions architect learned that a couple of security group rules are not configured as desired. The solutions architect wants to confirm which IAM user was responsible for making changes. Which service should the solutions architect use to find the desired information?

- A. Amazon GuardDuty
- B. Amazon Inspector
- C. AWS CloudTrail
- D. AWS Config

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS CloudTrail records API calls including who (IAM user/role) made the call, what action, when, and from where. This is used for auditing changes. GuardDuty (A) is threat detection. Inspector (B) is vulnerability scanning. AWS Config (D) tracks configuration state, not who made changes. (AWS Docs: CloudTrail for Auditing).
</details>

---

**Q396.** A company has implemented a self-managed DNS service on AWS. The solution consists of the following: - Amazon EC2 instances in different AWS Regions - Endpoints of a standard accelerator in AWS Global Accelerator. The company wants to protect the solution against DDoS attacks. What should a solutions architect do to meet this requirement?

- A. Subscribe to AWS Shield Advanced. Add the accelerator as a resource to protect.
- B. Subscribe to AWS Shield Advanced. Add the EC2 instances as resources to protect.
- C. Create an AWS WAF web ACL that includes a rate-based rule. Associate the web ACL with the accelerator.
- D. Create an AWS WAF web ACL that includes a rate-based rule. Associate the web ACL with the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Shield Advanced protects Global Accelerator endpoints. You should add the accelerator as a protected resource. WAF (C, D) cannot be directly associated with a standard accelerator (only with CloudFront, ALB, API Gateway). EC2 instances (B) are not directly protected by Shield Advanced unless behind a load balancer. (AWS Docs: Shield Advanced with Global Accelerator).
</details>

---

**Q397.** An ecommerce company needs to run a scheduled daily job to aggregate and filter sales records for analytics. The company stores the sales records in an Amazon S3 bucket. Each object can be up to 10 GB in size. Based on the number of sales events, the job can take up to an hour to complete. The CPU and memory usage of the job are constant and are known in advance. A solutions architect needs to minimize the amount of operational effort that is needed for the job to run. Which solution meets these requirements?

- A. Create an AWS Lambda function that has an Amazon EventBridge notification. Schedule the EventBridge event to run once a day.
- B. Create an AWS Lambda function. Create an Amazon API Gateway HTTP API, and integrate the API with the function. Create an Amazon EventBridge scheduled event that calls the API and invokes the function.
- C. Create an Amazon Elastic Container Service (Amazon ECS) cluster with an AWS Fargate launch type. Create an Amazon EventBridge scheduled event that launches an ECS task on the cluster to run the job.
- D. Create an Amazon Elastic Container Service (Amazon ECS) cluster with an Amazon EC2 launch type and an Auto Scaling group with at least one EC2 instance. Create an Amazon EventBridge scheduled event that launches an ECS task on the cluster to run the job.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Fargate is serverless and suitable for long-running jobs (up to 1 hour). Lambda (A, B) has a 15-minute timeout, insufficient for a 1-hour job. Option D (EC2 launch type) requires managing EC2 instances (more operational effort). (AWS Docs: ECS Fargate for Batch Jobs).
</details>

---

**Q398.** A company needs to transfer 600 TB of data from its on-premises network-attached storage (NAS) system to the AWS Cloud. The data transfer must be complete within 2 weeks. The data is sensitive and must be encrypted in transit. The company's internet connection can support an upload speed of 100 Mbps. Which solution meets these requirements MOST cost-effectively?

- A. Use Amazon S3 multi-part upload functionality to transfer the files over HTTPS.
- B. Create a VPN connection between the on-premises NAS system and the nearest AWS Region. Transfer the data over the VPN connection.
- C. Use the AWS Snow Family console to order several AWS Snowball Edge Storage Optimized devices. Use the devices to transfer the data to Amazon S3.
- D. Set up a 10 Gbps AWS Direct Connect connection between the company location and the nearest AWS Region. Transfer the data over a VPN connection into the Region to store the data in Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* 600 TB over 100 Mbps would take ~600 TB * 8 / 100 Mbps = 4.8 million seconds = ~55 days, exceeding 2 weeks. Snowball Edge devices can transfer large volumes quickly via physical shipping, and encryption is built-in. Direct Connect (D) setup time and cost are high. (AWS Docs: Snowball for Large Transfers).
</details>

---

**Q399.** A financial company hosts a web application on AWS. The application uses an Amazon API Gateway Regional API endpoint to give users the ability to retrieve current stock prices. The company's security team has noticed an increase in the number of API requests. The security team is concerned that HTTP flood attacks might take the application offline. A solutions architect must design a solution to protect the application from this type of attack. Which solution meets these requirements with the LEAST operational overhead?

- A. Create an Amazon CloudFront distribution in front of the API Gateway Regional API endpoint with a maximum TTL of 24 hours.
- B. Create a Regional AWS WAF web ACL with a rate-based rule. Associate the web ACL with the API Gateway stage.
- C. Use Amazon CloudWatch metrics to monitor the Count metric and alert the security team when the predefined rate is reached.
- D. Create an Amazon CloudFront distribution with Lambda@Edge in front of the API Gateway Regional API endpoint. Create an AWS Lambda function to block requests from IP addresses that exceed the predefined rate.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS WAF with a rate-based rule is the standard solution to mitigate HTTP flood attacks, with minimal operational overhead. It can be directly associated with an API Gateway stage. CloudFront (A, D) adds complexity. CloudWatch alerts (C) only notify, do not block. (AWS Docs: WAF Rate-Based Rules for API Gateway).
</details>

---

**Q400.** A meteorological startup company has a custom web application to sell weather data to its users online. The company uses Amazon DynamoDB to store its data and wants to build a new service that sends an alert to the managers of four internal teams every time a new weather event is recorded. The company does not want this new service to affect the performance of the current application. What should a solutions architect do to meet these requirements with the LEAST amount of operational overhead?

- A. Use DynamoDB transactions to write new event data to the table. Configure the transactions to notify internal teams.
- B. Have the current application publish a message to four Amazon Simple Notification Service (Amazon SNS) topics. Have each team subscribe to one topic.
- C. Enable Amazon DynamoDB Streams on the table. Use triggers to write to a single Amazon Simple Notification Service (Amazon SNS) topic to which the teams can subscribe.
- D. Add a custom attribute to each record to flag new items. Write a cron job that scans the table every minute for items that are new and notifies an Amazon Simple Queue Service (Amazon SQS) queue to which the teams can subscribe.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* DynamoDB Streams + Lambda trigger to publish to a single SNS topic is the most efficient. The four teams can subscribe to the same SNS topic. This does not affect the current application's performance (asynchronous). Option B requires modifying the current application. Option A (transactions) adds overhead. Option D (scan) is inefficient. (AWS Docs: DynamoDB Streams to SNS).
</details>

---

**Q401.** A company wants to use the AWS Cloud to make an existing application highly available and resilient. The current version of the application resides in the company's data center. The application recently experienced data loss after a database server crashed because of an unexpected power outage. The company needs a solution that avoids any single points of failure. The solution must give the application the ability to scale to meet user demand. Which solution will meet these requirements?

- A. Deploy the application servers by using Amazon EC2 instances in an Auto Scaling group across multiple Availability Zones. Use an Amazon RDS DB instance in a Multi-AZ configuration.
- B. Deploy the application servers by using Amazon EC2 instances in an Auto Scaling group in a single Availability Zone. Deploy the database on an EC2 instance. Enable EC2 Auto Recovery.
- C. Deploy the application servers by using Amazon EC2 instances in an Auto Scaling group across multiple Availability Zones. Use an Amazon RDS DB instance with a read replica in a single Availability Zone. Promote the read replica to replace the primary DB instance if the primary DB instance fails.
- D. Deploy the application servers by using Amazon EC2 instances in an Auto Scaling group across multiple Availability Zones. Deploy the primary and secondary database servers on EC2 instances across multiple Availability Zones. Use Amazon Elastic Block Store (Amazon EBS) Multi-Attach to create shared storage between the instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Multi-AZ Auto Scaling for application servers and RDS Multi-AZ for database eliminates single points of failure, provides HA, and allows scaling. Option B (single AZ) is not HA. Option C (read replica in single AZ) does not provide HA for writes. Option D (EBS Multi-Attach) is complex and not recommended for databases. (AWS Docs: Multi-AZ Architecture).
</details>

---

**Q402.** A company needs to ingest and handle large amounts of streaming data that its application generates. The application runs on Amazon EC2 instances and sends data to Amazon Kinesis Data Streams, which is configured with default settings. Every other day, the application consumes the data and writes the data to an Amazon S3 bucket for business intelligence (BI) processing. The company observes that Amazon S3 is not receiving all the data that the application sends to Kinesis Data Streams. What should a solutions architect do to resolve this issue?

- A. Update the Kinesis Data Streams default settings by modifying the data retention period.
- B. Update the application to use the Kinesis Producer Library (KPL) to send the data to Kinesis Data Streams.
- C. Update the number of Kinesis shards to handle the throughput of the data that is sent to Kinesis Data Streams.
- D. Turn on S3 Versioning within the S3 bucket to preserve every version of every object that is ingested in the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Data loss in Kinesis Data Streams often indicates insufficient shards to handle the write throughput (1 MB/sec or 1000 records/sec per shard). Increasing the number of shards resolves this. Option A (retention period) affects how long data is kept, not throughput. Option B (KPL) helps with batching but not if shards are saturated. Option D (S3 Versioning) doesn't address ingestion. (AWS Docs: Kinesis Shard Throughput).
</details>

---

**Q403.** A developer has an application that uses an AWS Lambda function to upload files to Amazon S3 and needs the required permissions to perform the task. The developer already has an IAM user with valid IAM credentials required for Amazon S3. What should a solutions architect do to grant the permissions?

- A. Add required IAM permissions in the resource policy of the Lambda function.
- B. Create a signed request using the existing IAM credentials in the Lambda function.
- C. Create a new IAM user and use the existing IAM credentials in the Lambda function.
- D. Create an IAM execution role with the required permissions and attach the IAM role to the Lambda function.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Lambda functions use an execution role (IAM role) to get permissions. You should create a role with S3 upload permissions and attach it to the function. Option A (resource policy) is for who can invoke the function, not what it can do. Option B (signed request) is unnecessary. Option C (new user) is not the Lambda pattern. (AWS Docs: Lambda Execution Role).
</details>

---

**Q404.** A company has deployed a serverless application that invokes an AWS Lambda function when new documents are uploaded to an Amazon S3 bucket. The application uses the Lambda function to process the documents. After a recent marketing campaign, the company noticed that the application did not process many of the documents. What should a solutions architect do to improve the architecture of this application?

- A. Set the Lambda function's runtime timeout value to 15 minutes.
- B. Configure an S3 bucket replication policy. Stage the documents in the S3 bucket for later processing.
- C. Deploy an additional Lambda function. Load balance the processing of the documents across the two Lambda functions.
- D. Create an Amazon Simple Queue Service (Amazon SQS) queue. Send the requests to the queue. Configure the queue as an event source for Lambda.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 event notifications can trigger Lambda directly, but if the Lambda function fails or throttles, events can be lost. Using SQS as a buffer between S3 and Lambda provides durability and allows Lambda to scale based on queue depth. Option A (timeout) doesn't prevent lost events. Option B (replication) doesn't help. Option C (two functions) still has same issue. (AWS Docs: S3 to SQS to Lambda).
</details>

---

**Q405.** A solutions architect is designing the architecture for a software demonstration environment. The environment will run on Amazon EC2 instances in an Auto Scaling group behind an Application Load Balancer (ALB). The system will experience significant increases in traffic during working hours but is not required to operate on weekends. Which combination of actions should the solutions architect take to ensure that the system can scale to meet demand? (Choose two.)

- A. Use AWS Auto Scaling to adjust the ALB capacity based on request rate.
- B. Use AWS Auto Scaling to scale the capacity of the VPC internet gateway.
- C. Launch the EC2 instances in multiple AWS Regions to distribute the load across Regions.
- D. Use a target tracking scaling policy to scale the Auto Scaling group based on instance CPU utilization.
- E. Use scheduled scaling to change the Auto Scaling group minimum, maximum, and desired capacity to zero for weekends. Revert to the default values at the start of the week.

<details>
<summary>Show Answer</summary>

**Answer: D, E**

*Explanation:* Target tracking scaling (D) handles dynamic scaling during working hours. Scheduled scaling (E) sets capacity to zero on weekends to save costs. Option A (ALB capacity) is not a thing; ALB scales automatically. Option B (IGW) is not scalable via Auto Scaling. Option C (multi-Region) is overkill for a demo environment. (AWS Docs: Target Tracking, Scheduled Scaling).
</details>

---

**Q406.** A solutions architect is designing a two-tiered architecture that includes a public subnet and a database subnet. The web servers in the public subnet must be open to the internet on port 443. The Amazon RDS for MySQL DB instance in the database subnet must be accessible only to the web servers on port 3306. Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

- A. Create a network ACL for the public subnet. Add a rule to deny outbound traffic to 0.0.0.0/0 on port 3306.
- B. Create a security group for the DB instance. Add a rule to allow traffic from the public subnet CIDR block on port 3306.
- C. Create a security group for the web servers in the public subnet. Add a rule to allow traffic from 0.0.0.0/0 on port 443.
- D. Create a security group for the DB instance. Add a rule to allow traffic from the web servers' security group on port 3306.
- E. Create a security group for the DB instance. Add a rule to deny all traffic except traffic from the web servers' security group on port 3306.

<details>
<summary>Show Answer</summary>

**Answer: C, D**

*Explanation:* Web server security group allows HTTPS from anywhere (C). DB security group allows MySQL traffic only from the web server security group (D) (not CIDR block). Option B (CIDR) is less secure. Option A (NACL deny) is not needed. Option E (deny rule) is redundant; security groups are allow-only. (AWS Docs: Security Group Referencing).
</details>

---

**Q407.** A company is implementing a shared storage solution for a gaming application that is hosted in the AWS Cloud. The company needs the ability to use Lustre clients to access data. The solution must be fully managed. Which solution meets these requirements?

- A. Create an AWS DataSync task that shares the data as a mountable file system. Mount the file system to the application server.
- B. Create an AWS Storage Gateway file gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.
- C. Create an Amazon Elastic File System (Amazon EFS) file system, and configure it to support Lustre. Attach the file system to the origin server. Connect the application server to the file system.
- D. Create an Amazon FSx for Lustre file system. Attach the file system to the origin server. Connect the application server to the file system.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* FSx for Lustre is a fully managed file system that supports Lustre clients. EFS (C) does not support Lustre. DataSync (A) is for transfer. Storage Gateway (B) does not support Lustre. (AWS Docs: FSx for Lustre).
</details>

---

**Q408.** A company runs an application that receives data from thousands of geographically dispersed remote devices that use UDP. The application processes the data immediately and sends a message back to the device if necessary. No data is stored. The company needs a solution that minimizes latency for the data transmission from the devices. The solution also must provide rapid failover to another AWS Region. Which solution will meet these requirements?

- A. Configure an Amazon Route 53 failover routing policy. Create a Network Load Balancer (NLB) in each of the two Regions. Configure the NLB to invoke an AWS Lambda function to process the data.
- B. Use AWS Global Accelerator. Create a Network Load Balancer (NLB) in each of the two Regions as an endpoint. Create an Amazon Elastic Container Service (Amazon ECS) cluster with the Fargate launch type. Create an ECS service on the cluster. Set the ECS service as the target for the NLB. Process the data in Amazon ECS.
- C. Use AWS Global Accelerator. Create an Application Load Balancer (ALB) in each of the two Regions as an endpoint. Create an Amazon Elastic Container Service (Amazon ECS) cluster with the Fargate launch type. Create an ECS service on the cluster. Set the ECS service as the target for the ALB. Process the data in Amazon ECS.
- D. Configure an Amazon Route 53 failover routing policy. Create an Application Load Balancer (ALB) in each of the two Regions. Create an Amazon Elastic Container Service (Amazon ECS) cluster with the Fargate launch type. Create an ECS service on the cluster. Set the ECS service as the target for the ALB. Process the data in Amazon ECS.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Global Accelerator supports UDP, provides rapid failover, and minimizes latency. NLBs are required for UDP (ALBs do not support UDP). Fargate ECS can process the data. Option A (Route 53 failover) is slower failover. Options C/D use ALBs which don't support UDP. (AWS Docs: Global Accelerator for UDP).
</details>

---

**Q409.** A solutions architect must migrate a Windows Internet Information Services (IIS) web application to AWS. The application currently relies on a file share hosted in the user's on-premises network-attached storage (NAS). The solutions architect has proposed migrating the IIS web servers to Amazon EC2 instances in multiple Availability Zones that are connected to the storage solution, and configuring an Elastic Load Balancer attached to the instances. Which replacement to the on-premises file share is MOST resilient and durable?

- A. Migrate the file share to Amazon RDS.
- B. Migrate the file share to AWS Storage Gateway.
- C. Migrate the file share to Amazon FSx for Windows File Server.
- D. Migrate the file share to Amazon Elastic File System (Amazon EFS).

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* FSx for Windows File Server is a fully managed, highly available (Multi-AZ), and durable Windows file share. EFS (D) is NFS, not Windows-native. Storage Gateway (B) is a hybrid solution. RDS (A) is a database. (AWS Docs: FSx for Windows File Server).
</details>

---

**Q410.** A company is deploying a new application on Amazon EC2 instances. The application writes data to Amazon Elastic Block Store (Amazon EBS) volumes. The company needs to ensure that all data that is written to the EBS volumes is encrypted at rest. Which solution will meet this requirement?

- A. Create an IAM role that specifies EBS encryption. Attach the role to the EC2 instances.
- B. Create the EBS volumes as encrypted volumes. Attach the EBS volumes to the EC2 instances.
- C. Create an EC2 instance tag that has a key of Encrypt and a value of True. Tag all instances that require encryption at the EBS level.
- D. Create an AWS Key Management Service (AWS KMS) key policy that enforces EBS encryption in the account. Ensure that the key policy is active.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To encrypt data at rest on EBS, you must create the EBS volume with encryption enabled. IAM role (A) and tags (C) do not enforce encryption. Key policy (D) is part of the solution but you still need to create encrypted volumes. (AWS Docs: EBS Encryption).
</details>

---

**Q411.** A company has a web application with sporadic usage patterns. There is heavy usage at the beginning of each month, moderate usage at the start of each week, and unpredictable usage during the week. The application consists of a web server and a MySQL database server running inside the data center. The company would like to move the application to the AWS Cloud, and needs to select a cost-effective database platform that will not require database modifications. Which solution will meet these requirements?

- A. Amazon DynamoDB
- B. Amazon RDS for MySQL
- C. MySQL-compatible Amazon Aurora Serverless
- D. MySQL deployed on Amazon EC2 in an Auto Scaling group

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Aurora Serverless for MySQL is MySQL-compatible (no code changes), automatically scales compute capacity based on usage, and is cost-effective for sporadic patterns. DynamoDB (A) is not MySQL-compatible. RDS for MySQL (B) requires provisioning for peak capacity. EC2 MySQL (D) has more overhead. (AWS Docs: Aurora Serverless).
</details>

---

**Q412.** An image-hosting company stores its objects in Amazon S3 buckets. The company wants to avoid accidental exposure of the objects in the S3 buckets to the public. All S3 objects in the entire AWS account need to remain private. Which solution will meet these requirements?

- A. Use Amazon GuardDuty to monitor S3 bucket policies. Create an automatic remediation action rule that uses an AWS Lambda function to remediate any change that makes the objects public.
- B. Use AWS Trusted Advisor to find publicly accessible S3 buckets. Configure email notifications in Trusted Advisor when a change is detected. Manually change the S3 bucket policy if it allows public access.
- C. Use AWS Resource Access Manager to find publicly accessible S3 buckets. Use Amazon Simple Notification Service (Amazon SNS) to invoke an AWS Lambda function when a change is detected. Deploy a Lambda function that programmatically remediates the change.
- D. Use the S3 Block Public Access feature on the account level. Use AWS Organizations to create a service control policy (SCP) that prevents IAM users from changing the setting. Apply the SCP to the account.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 Block Public Access at the account level prevents any bucket from becoming public. An SCP can prevent users from disabling this setting. This is a preventive control. Options A, B, C are detective or reactive. (AWS Docs: S3 Block Public Access).
</details>

---

**Q413.** An ecommerce company is experiencing an increase in user traffic. The company's store is deployed on Amazon EC2 instances as a two-tier web application consisting of a web tier and a separate database tier. As traffic increases, the company notices that the architecture is causing significant delays in sending timely marketing and order confirmation email to users. The company wants to reduce the time it spends resolving complex email delivery issues and minimize operational overhead. What should a solutions architect do to meet these requirements?

- A. Create a separate application tier using EC2 instances dedicated to email processing.
- B. Configure the web instance to send email through Amazon Simple Email Service (Amazon SES).
- C. Configure the web instance to send email through Amazon Simple Notification Service (Amazon SNS).
- D. Create a separate application tier using EC2 instances dedicated to email processing. Place the instances in an Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon SES is a managed email service that handles email delivery issues, reducing operational overhead. SNS (C) is for notifications, not email. EC2-based solutions (A, D) add operational overhead. (AWS Docs: Amazon SES).
</details>

---

**Q414.** A company has a business system that generates hundreds of reports each day. The business system saves the reports to a network share in CSV format. The company needs to store this data in the AWS Cloud in near-real time for analysis. Which solution will meet these requirements with the LEAST administrative overhead?

- A. Use AWS DataSync to transfer the files to Amazon S3. Create a scheduled task that runs at the end of each day.
- B. Create an Amazon S3 File Gateway. Update the business system to use a new network share from the S3 File Gateway.
- C. Use AWS DataSync to transfer the files to Amazon S3. Create an application that uses the DataSync API in the automation workflow.
- D. Deploy an AWS Transfer for SFTP endpoint. Create a script that checks for new files on the network share and uploads the new files by using SFTP.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 File Gateway presents an NFS/SMB share that the business system can write to directly, storing data in S3 in near-real time with minimal overhead. DataSync (A, C) would require scheduling or custom code. SFTP (D) requires scripting. (AWS Docs: S3 File Gateway).
</details>

---

**Q415.** A company is storing petabytes of data in Amazon S3 Standard. The data is stored in multiple S3 buckets and is accessed with varying frequency. The company does not know access patterns for all the data. The company needs to implement a solution for each S3 bucket to optimize the cost of S3 usage. Which solution will meet these requirements with the MOST operational efficiency?

- A. Create an S3 Lifecycle configuration with a rule to transition the objects in the S3 bucket to S3 Intelligent-Tiering.
- B. Use the S3 storage class analysis tool to determine the correct tier for each object in the S3 bucket. Move each object to the identified storage tier.
- C. Create an S3 Lifecycle configuration with a rule to transition the objects in the S3 bucket to S3 Glacier Instant Retrieval.
- D. Create an S3 Lifecycle configuration with a rule to transition the objects in the S3 bucket to S3 One Zone-Infrequent Access (S3 One Zone-IA).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Intelligent-Tiering automatically optimizes storage costs by moving objects between tiers based on access patterns, with no retrieval fees. This is the most operationally efficient for unknown access patterns. Option B (manual analysis) is not efficient. Options C/D may not be optimal for varying patterns. (AWS Docs: S3 Intelligent-Tiering).
</details>

---

**Q416.** A rapidly growing global ecommerce company is hosting its web application on AWS. The web application includes static content and dynamic content. The website stores online transaction processing (OLTP) data in an Amazon RDS database. The website's users are experiencing slow page loads. Which combination of actions should a solutions architect take to resolve this issue? (Choose two.)

- A. Configure an Amazon Redshift cluster.
- B. Set up an Amazon CloudFront distribution.
- C. Host the dynamic web content in Amazon S3.
- D. Create a read replica for the RDS DB instance.
- E. Configure a Multi-AZ deployment for the RDS DB instance.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* CloudFront (B) caches static content at edge locations, reducing latency. A read replica (D) offloads read queries from the primary database, improving OLTP performance. Redshift (A) is for analytics. Dynamic content in S3 (C) is not possible. Multi-AZ (E) provides HA, not read performance. (AWS Docs: CloudFront, RDS Read Replicas).
</details>

---

**Q417.** A company uses Amazon EC2 instances and AWS Lambda functions to run its application. The company has VPCs with public subnets and private subnets in its AWS account. The EC2 instances run in a private subnet in one of the VPCs. The Lambda functions need direct network access to the EC2 instances for the application to work. The application will run for at least 1 year. The company expects the number of Lambda functions that the application uses to increase during that time. The company wants to maximize its savings on all application resources and to keep network latency between the services low. Which solution will meet these requirements?

- A. Purchase an EC2 Instance Savings Plan. Optimize the Lambda functions' duration and memory usage and the number of invocations. Connect the Lambda functions to the private subnet that contains the EC2 instances.
- B. Purchase an EC2 Instance Savings Plan. Optimize the Lambda functions' duration and memory usage, the number of invocations, and the amount of data that is transferred. Connect the Lambda functions to a public subnet in the same VPC where the EC2 instances run.
- C. Purchase a Compute Savings Plan. Optimize the Lambda functions' duration and memory usage, the number of invocations, and the amount of data that is transferred. Connect the Lambda functions to the private subnet that contains the EC2 instances.
- D. Purchase a Compute Savings Plan. Optimize the Lambda functions' duration and memory usage, the number of invocations, and the amount of data that is transferred. Keep the Lambda functions in the Lambda service VPC.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* A Compute Savings Plan covers both EC2 and Lambda, maximizing savings. Lambda functions must be connected to the same VPC (private subnet) to access EC2 instances directly, keeping low latency. Option D (Lambda service VPC) would not have access to the customer VPC. (AWS Docs: Compute Savings Plan, Lambda VPC).
</details>

---

**Q418.** A solutions architect needs to allow team members to access Amazon S3 buckets in two different AWS accounts: a development account and a production account. The team currently has access to S3 buckets in the development account by using unique IAM users that are assigned to an IAM group that has appropriate permissions in the account. The solutions architect has created an IAM role in the production account. The role has a policy that grants access to an S3 bucket in the production account. Which solution will meet these requirements while complying with the principle of least privilege?

- A. Attach the Administrator Access policy to the development account users.
- B. Add the development account as a principal in the trust policy of the role in the production account.
- C. Turn off the S3 Block Public Access feature on the S3 bucket in the production account.
- D. Create a user in the production account with unique credentials for each team member.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* To allow users in the development account to assume the IAM role in the production account, you add the development account as a principal in the role's trust policy. This follows least privilege (they assume the role, not get direct access). Option A (Admin) is excessive. Option C (Block Public Access) is unrelated. Option D creates duplicate users. (AWS Docs: Cross-Account IAM Roles).
</details>

---

**Q419.** A company uses AWS Organizations with all features enabled and runs multiple Amazon EC2 workloads in the ap-southeast-2 Region. The company has a service control policy (SCP) that prevents any resources from being created in any other Region. A security policy requires the company to encrypt all data at rest. An audit discovers that employees have created Amazon Elastic Block Store (Amazon EBS) volumes for EC2 instances without encrypting the volumes. The company wants any new EC2 instances that any IAM user or root user launches in ap-southeast-2 to use encrypted EBS volumes. The company wants a solution that will have minimal effect on employees who create EBS volumes. Which combination of steps will meet these requirements? (Choose two.)

- A. In the Amazon EC2 console, select the EBS encryption account attribute and define a default encryption key.
- B. Create an IAM permission boundary. Attach the permission boundary to the root organizational unit (OU). Define the boundary to deny the ec2:CreateVolume action when the ec2:Encrypted condition equals false.
- C. Create an SCP. Attach the SCP to the root organizational unit (OU). Define the SCP to deny the ec2:CreateVolume action when the ec2:Encrypted condition equals false.
- D. Update the IAM policies for each account to deny the ec2:CreateVolume action when the ec2:Encrypted condition equals false.
- E. In the Organizations management account, specify the Default EBS volume encryption setting.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* Enable EBS encryption by default in the Region (A) using the account attribute. Additionally, create an SCP (C) to deny creation of unencrypted volumes, which is enforced even for root users. Option E (management account setting) does not exist. Option B (permission boundary) is complex. Option D (updating IAM policies) doesn't affect root user. (AWS Docs: EBS Encryption by Default, SCP for Encryption).
</details>

---

**Q420.** A company wants to use an Amazon RDS for PostgreSQL DB cluster to simplify time-consuming database administrative tasks for production database workloads. The company wants to ensure that its database is highly available and will provide automatic failover support in most scenarios in less than 40 seconds. The company wants to offload reads off of the primary instance and keep costs as low as possible. Which solution will meet these requirements?

- A. Use an Amazon RDS Multi-AZ DB instance deployment. Create one read replica and point the read workload to the read replica.
- B. Use an Amazon RDS Multi-AZ DB cluster deployment. Create two read replicas and point the read workload to the read replicas.
- C. Use an Amazon RDS Multi-AZ DB instance deployment. Point the read workload to the secondary instances in the Multi-AZ pair.
- D. Use an Amazon RDS Multi-AZ DB cluster deployment. Point the read workload to the reader endpoint.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* RDS Multi-AZ DB cluster deployment (for PostgreSQL) has a reader endpoint that distributes read traffic across two readable standby instances, offloading reads from the primary. It provides fast failover (<40 sec). Option A (Multi-AZ instance + read replica) has higher cost. Option B (two replicas) is more expensive than D. Option C (Multi-AZ instance secondary) is not readable. (AWS Docs: RDS Multi-AZ DB Cluster).
</details>

---

**Q421.** A company runs a highly available SFTP service. The SFTP service uses two Amazon EC2 Linux instances that run with elastic IP addresses to accept traffic from trusted IP sources on the internet. The SFTP service is backed by shared storage that is attached to the instances. User accounts are created and managed as Linux users in the SFTP servers. The company wants a serverless option that provides high IOPS performance and highly configurable security. The company also wants to maintain control over user permissions. Which solution will meet these requirements?

- A. Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume. Create an AWS Transfer Family SFTP service with a public endpoint that allows only trusted IP addresses. Attach the EBS volume to the SFTP service endpoint. Grant users access to the SFTP service.
- B. Create an encrypted Amazon Elastic File System (Amazon EFS) volume. Create an AWS Transfer Family SFTP service with elastic IP addresses and a VPC endpoint that has internet-facing access. Attach a security group to the endpoint that allows only trusted IP addresses. Attach the EFS volume to the SFTP service endpoint. Grant users access to the SFTP service.
- C. Create an Amazon S3 bucket with default encryption enabled. Create an AWS Transfer Family SFTP service with a public endpoint that allows only trusted IP addresses. Attach the S3 bucket to the SFTP service endpoint. Grant users access to the SFTP service.
- D. Create an Amazon S3 bucket with default encryption enabled. Create an AWS Transfer Family SFTP service with a VPC endpoint that has internal access in a private subnet. Attach a security group that allows only trusted IP addresses. Attach the S3 bucket to the SFTP service endpoint. Grant users access to the SFTP service.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Transfer Family SFTP is serverless. S3 provides high IOPS and durable storage. You can control user permissions via IAM roles or custom identity providers. EBS (A) and EFS (B) cannot be attached to Transfer Family endpoints; Transfer Family supports S3 and EFS. Option D (private endpoint) would not be accessible from the internet (public endpoint needed). (AWS Docs: Transfer Family SFTP with S3).
</details>

---

**Q422.** A company is developing a new machine learning (ML) model solution on AWS. The models are developed as independent microservices that fetch approximately 1 GB of model data from Amazon S3 at startup and load the data into memory. Users access the models through an asynchronous API. Users can send a request or a batch of requests and specify where the results should be sent. The company provides models to hundreds of users. The usage patterns for the models are irregular. Some models could be unused for days or weeks. Other models could receive batches of thousands of requests at a time. Which design should a solutions architect recommend to meet these requirements?

- A. Direct the requests from the API to a Network Load Balancer (NLB). Deploy the models as AWS Lambda functions that are invoked by the NLB.
- B. Direct the requests from the API to an Application Load Balancer (ALB). Deploy the models as Amazon Elastic Container Service (Amazon ECS) services that read from an Amazon Simple Queue Service (Amazon SQS) queue. Use AWS App Mesh to scale the instances of the ECS cluster based on the SQS queue size.
- C. Direct the requests from the API into an Amazon Simple Queue Service (Amazon SQS) queue. Deploy the models as AWS Lambda functions that are invoked by SQS events. Use AWS Auto Scaling to increase the number of vCPUs for the Lambda functions based on the SQS queue size.
- D. Direct the requests from the API into an Amazon Simple Queue Service (Amazon SQS) queue. Deploy the models as Amazon Elastic Container Service (Amazon ECS) services that read from the queue. Enable AWS Auto Scaling on Amazon ECS for both the cluster and copies of the service based on the queue size.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Models need to load 1 GB of data into memory, which Lambda (A, C) cannot efficiently do (memory limits, cold start latency). ECS services can handle large memory footprints. Using SQS decouples requests, and Auto Scaling based on queue size scales the ECS services appropriately for irregular patterns. Option B uses ALB directly (no queue) which could lead to lost requests. (AWS Docs: ECS with SQS Scaling).
</details>

---

**Q423.** A solutions architect wants to use the following JSON text as an identity-based policy to grant specific permissions: `{ "Statement": { "Action": { "ssm:ListDocuments", "ssm:GetDocument" }, "Effect": "Allow", "Resource": "*", "Sid": "*" }, "Version": "2012-10-17" }`. Which IAM principals can the solutions architect attach this policy to? (Choose two.)

- A. Role
- B. Group
- C. Organization
- D. Amazon Elastic Container Service (Amazon ECS) resource
- E. Amazon EC2 resource

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* IAM identity-based policies can be attached to IAM users, groups (B), and roles (A). They cannot be attached to Organizations (C), ECS resources (D), or EC2 resources (E) directly. (AWS Docs: IAM Policy Attachment).
</details>

---

**Q424.** A company is running a custom application on Amazon EC2 On-Demand Instances. The application has frontend nodes that need to run 24 hours a day, 7 days a week and backend nodes that need to run only for a short time based on workload. The number of backend nodes varies during the day. The company needs to scale out and scale in more instances based on workload. Which solution will meet these requirements MOST cost-effectively?

- A. Use Reserved Instances for the frontend nodes. Use AWS Fargate for the backend nodes.
- B. Use Reserved Instances for the frontend nodes. Use Spot Instances for the backend nodes.
- C. Use Spot Instances for the frontend nodes. Use Reserved Instances for the backend nodes.
- D. Use Spot Instances for the frontend nodes. Use AWS Fargate for the backend nodes.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Frontend runs 24/7 -> Reserved Instances for discount. Backend is short-lived and variable -> Spot Instances are cheapest (if workload is fault-tolerant). Fargate (A) is more expensive than Spot. Option C swaps incorrectly. (AWS Docs: Spot for Batch/Backend).
</details>

---

**Q425.** A company uses high block storage capacity to runs its workloads on premises. The company's daily peak input and output transactions per second are not more than 15,000 IOPS. The company wants to migrate the workloads to Amazon EC2 and to provision disk performance independent of storage capacity. Which Amazon Elastic Block Store (Amazon EBS) volume type will meet these requirements MOST cost-effectively?

- A. GP2 volume type
- B. io2 volume type
- C. GP3 volume type
- D. io1 volume type

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* GP3 allows you to provision IOPS (up to 20,000) independent of storage capacity, and is cheaper than io1/io2. GP2 (A) ties IOPS to capacity. io1/io2 (B, D) are more expensive for 15,000 IOPS. (AWS Docs: gp3 Volumes).
</details>

---

**Q426.** A company needs to store data from its healthcare application. The application's data frequently changes. A new regulation requires audit access at all levels of the stored data. The company hosts the application on an on-premises infrastructure that is running out of storage capacity. A solutions architect must securely migrate the existing data to AWS while satisfying the new regulation. Which solution will meet these requirements?

- A. Use AWS DataSync to move the existing data to Amazon S3. Use AWS CloudTrail to log data events.
- B. Use AWS Snowcone to move the existing data to Amazon S3. Use AWS CloudTrail to log management events.
- C. Use Amazon S3 Transfer Acceleration to move the existing data to Amazon S3. Use AWS CloudTrail to log data events.
- D. Use AWS Storage Gateway to move the existing data to Amazon S3. Use AWS CloudTrail to log management events.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* DataSync is good for migration. CloudTrail data events can log S3 object-level API activity (e.g., GetObject, PutObject) for auditing. Management events (B, D) are not sufficient for data-level audit. Transfer Acceleration (C) is for speed, not auditing. (AWS Docs: CloudTrail Data Events for S3).
</details>

---

**Q427.** A solutions architect is implementing a complex Java application with a MySQL database. The Java application must be deployed on Apache Tomcat and must be highly available. What should the solutions architect do to meet these requirements?

- A. Deploy the application in AWS Lambda. Configure an Amazon API Gateway API to connect with the Lambda functions.
- B. Deploy the application by using AWS Elastic Beanstalk. Configure a load-balanced environment and a rolling deployment policy.
- C. Migrate the database to Amazon ElastiCache. Configure the ElastiCache security group to allow access from the application.
- D. Launch an Amazon EC2 instance. Install a MySQL server on the EC2 instance. Configure the application on the server. Create an AMI. Use the AMI to create a launch template with an Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Elastic Beanstalk supports Java/Tomcat applications, provides load balancing, and can be configured for high availability (Multi-AZ). Rolling deployments ensure uptime. Lambda (A) is not suitable for Tomcat. ElastiCache (C) is a cache, not a database. Option D requires more manual work. (AWS Docs: Elastic Beanstalk for Java).
</details>

---

**Q428.** A serverless application uses Amazon API Gateway, AWS Lambda, and Amazon DynamoDB. The Lambda function needs permissions to read and write to the DynamoDB table. Which solution will give the Lambda function access to the DynamoDB table MOST securely?

- A. Create an IAM user with programmatic access to the Lambda function. Attach a policy to the user that allows read and write access to the DynamoDB table. Store the access_key_id and secret_access_key parameters as part of the Lambda environment variables. Ensure that other AWS users do not have read and write access to the Lambda function configuration.
- B. Create an IAM role that includes Lambda as a trusted service. Attach a policy to the role that allows read and write access to the DynamoDB table. Update the configuration of the Lambda function to use the new role as the execution role.
- C. Create an IAM user with programmatic access to the Lambda function. Attach a policy to the user that allows read and write access to the DynamoDB table. Store the access_key_id and secret_access_key parameters in AWS Systems Manager Parameter Store as secure string parameters. Update the Lambda function code to retrieve the secure string parameters before connecting to the DynamoDB table.
- D. Create an IAM role that includes DynamoDB as a trusted service. Attach a policy to the role that allows read and write access from the Lambda function. Update the code of the Lambda function to attach to the new role as an execution role.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* The correct pattern is to create an IAM execution role for the Lambda function with a trust policy allowing Lambda, and attach a policy granting DynamoDB access. This is the most secure (no hardcoded keys). Options A and C use IAM users with access keys (less secure). Option D incorrectly specifies DynamoDB as trusted service. (AWS Docs: Lambda Execution Role).
</details>

---

**Q429.** The following IAM policy is attached to an IAM group. This is the only policy applied to the group. `{ "Version": "2012-10-17", "Statement": [ { "Sid": "1", "Effect": "Allow", "Action": "ec2:*", "Resource": "*", "Condition": { "StringEquals": { "ec2:Region": "us-east-1" } } }, { "Sid": "2", "Effect": "Deny", "Action": [ "ec2:StopInstances", "ec2:TerminateInstances" ], "Resource": "*", "Condition": { "BoolIfExists": { "aws:MultiFactorAuthPresent": false } } } ] }`. What are the effective IAM permissions of this policy for group members?

- A. Group members are permitted any Amazon EC2 action within the us-east-1 Region. Statements after the Allow permission are not applied.
- B. Group members are denied any Amazon EC2 permissions in the us-east-1 Region unless they are logged in with multi-factor authentication (MFA).
- C. Group members are allowed the ec2:StopInstances and ec2:TerminateInstances permissions for all Regions when logged in with multi-factor authentication (MFA). Group members are permitted any other Amazon EC2 action.
- D. Group members are allowed the ec2:StopInstances and ec2:TerminateInstances permissions for the us-east-1 Region only when logged in with multi-factor authentication (MFA). Group members are permitted any other Amazon EC2 action within the us-east-1 Region.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* The Allow statement (Sid 1) allows all EC2 actions in us-east-1. The Deny statement (Sid 2) denies StopInstances and TerminateInstances if MFA is not present. Since Deny overrides Allow, Stop/Terminate are only allowed in us-east-1 when MFA is present. All other EC2 actions in us-east-1 are allowed. (AWS Docs: IAM Policy Evaluation Logic).
</details>

---

**Q430.** A manufacturing company has machine sensors that upload .csv files to an Amazon S3 bucket. These .csv files must be converted into images and must be made available as soon as possible for the automatic generation of graphical reports. The images become irrelevant after 1 month, but the .csv files must be kept to train machine learning (ML) models twice a year. The ML trainings and audits are planned weeks in advance. Which combination of steps will meet these requirements MOST cost-effectively? (Choose two.)

- A. Launch an Amazon EC2 Spot Instance that downloads the .csv files every hour, generates the image files, and uploads the images to the S3 bucket.
- B. Design an AWS Lambda function that converts the .csv files into images and stores the images in the S3 bucket. Invoke the Lambda function when a .csv file is uploaded.
- C. Create S3 Lifecycle rules for .csv files and image files in the S3 bucket. Transition the .csv files from S3 Standard to S3 Glacier 1 day after they are uploaded. Expire the image files after 30 days.
- D. Create S3 Lifecycle rules for .csv files and image files in the S3 bucket. Transition the .csv files from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) 1 day after they are uploaded. Expire the image files after 30 days.
- E. Create S3 Lifecycle rules for .csv files and image files in the S3 bucket. Transition the .csv files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 1 day after they are uploaded. Keep the image files in Reduced Redundancy Storage (RRS).

<details>
<summary>Show Answer</summary>

**Answer: B, C**

*Explanation:* Lambda (B) processes CSV to images as soon as uploaded. Lifecycle rule (C): transition CSV to S3 Glacier after 1 day (cheap storage for twice-yearly training, retrieval planned weeks in advance). Expire images after 30 days. Option A (EC2 Spot) has more overhead. Option D/E use IA classes which are more expensive than Glacier for long-term. (AWS Docs: Lambda for Conversion, S3 Lifecycle to Glacier).
</details>

---

**Q431.** A company has developed a new video game as a web application. The application is in a three-tier architecture in a VPC with Amazon RDS for MySQL in the database layer. Several players will compete concurrently online. The game's developers want to display a top-10 scoreboard in near-real time and offer the ability to stop and restore the game while preserving the current scores. What should a solutions architect do to meet these requirements?

- A. Set up an Amazon ElastiCache for Memcached cluster to cache the scores for the web application to display.
- B. Set up an Amazon ElastiCache for Redis cluster to compute and cache the scores for the web application to display.
- C. Place an Amazon CloudFront distribution in front of the web application to cache the scoreboard in a section of the application.
- D. Create a read replica on Amazon RDS for MySQL to run queries to compute the scoreboard and serve the read traffic to the web application.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Redis supports sorted sets, perfect for leaderboards, and can persist data (for stop/restore). Memcached (A) does not support persistence or sorted sets. CloudFront (C) caches static content, not dynamic scores. Read replica (D) would have higher latency and overhead. (AWS Docs: ElastiCache for Redis Leaderboards).
</details>

---

**Q432.** An ecommerce company wants to use machine learning (ML) algorithms to build and train models. The company will use the models to visualize complex scenarios and to detect trends in customer data. The architecture team wants to integrate its ML models with a reporting platform to analyze the augmented data and use the data directly in its business intelligence dashboards. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Glue to create an ML transform to build and train models. Use Amazon OpenSearch Service to visualize the data.
- B. Use Amazon SageMaker to build and train models. Use Amazon QuickSight to visualize the data.
- C. Use a pre-built ML Amazon Machine Image (AMI) from the AWS Marketplace to build and train models. Use Amazon OpenSearch Service to visualize the data.
- D. Use Amazon QuickSight to build and train models by using calculated fields. Use Amazon QuickSight to visualize the data.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon SageMaker is the managed ML service for building and training models. Amazon QuickSight is the BI service for visualization and dashboards. This combination minimizes operational overhead. Glue (A) is for ETL. OpenSearch (A, C) is for search/analytics, not BI dashboards. QuickSight (D) does not build ML models. (AWS Docs: SageMaker, QuickSight).
</details>

---

**Q433.** A company is running its production and nonproduction environment workloads in multiple AWS accounts. The accounts are in an organization in AWS Organizations. The company needs to design a solution that will prevent the modification of cost usage tags. Which solution will meet these requirements?

- A. Create a custom AWS Config rule to prevent tag modification except by authorized principals.
- B. Create a custom trail in AWS CloudTrail to prevent tag modification.
- C. Create a service control policy (SCP) to prevent tag modification except by authorized principals.
- D. Create custom Amazon CloudWatch logs to prevent tag modification.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* SCPs can be used to deny tag modification actions (e.g., `tag:TagResources`) across all accounts in the organization, with exceptions for authorized principals. Config (A) can detect but not prevent. CloudTrail (B) records, does not prevent. CloudWatch (D) is for monitoring. (AWS Docs: SCPs for Tag Protection).
</details>

---

**Q434.** A company hosts its application in the AWS Cloud. The application runs on Amazon EC2 instances behind an Elastic Load Balancer in an Auto Scaling group and with an Amazon DynamoDB table. The company wants to ensure the application can be made available in another AWS Region with minimal downtime. What should a solutions architect do to meet these requirements with the LEAST amount of downtime?

- A. Create an Auto Scaling group and a load balancer in the disaster recovery Region. Configure the DynamoDB table as a global table. Configure DNS failover to point to the new disaster recovery Region's load balancer.
- B. Create an AWS CloudFormation template to create EC2 instances, load balancers, and DynamoDB tables to be launched when needed. Configure DNS failover to point to the new disaster recovery Region's load balancer.
- C. Create an AWS CloudFormation template to create EC2 instances, and a load balancer to be launched when needed. Configure the DynamoDB table as a global table. Configure DNS failover to point to the new disaster recovery Region's load balancer.
- D. Create an Auto Scaling group and load balancer in the disaster recovery Region. Configure the DynamoDB table as a global table. Create an Amazon CloudWatch alarm to trigger an AWS Lambda function that updates Amazon Route 53 pointing to the disaster recovery load balancer.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* DynamoDB global tables provide multi-Region replication with no downtime. A CloudFormation template can quickly provision EC2/ALB infrastructure in the DR Region. DNS failover (Route 53) directs traffic. Option A uses pre-provisioned resources (more cost). Option B requires recreating DynamoDB. Option D adds Lambda complexity. (AWS Docs: DynamoDB Global Tables, CloudFormation for DR).
</details>

---

**Q435.** A company needs to migrate a MySQL database from its on-premises data center to AWS within 2 weeks. The database is 20 TB in size. The company wants to complete the migration with minimal downtime. Which solution will migrate the database MOST cost-effectively?

- A. Order an AWS Snowball Edge Storage Optimized device. Use AWS Database Migration Service (AWS DMS) with AWS Schema Conversion Tool (AWS SCT) to migrate the database with replication of ongoing changes. Send the Snowball Edge device to AWS to finish the migration and continue the ongoing replication.
- B. Order an AWS Snowmobile vehicle. Use AWS Database Migration Service (AWS DMS) with AWS Schema Conversion Tool (AWS SCT) to migrate the database with ongoing changes. Send the Snowmobile vehicle back to AWS to finish the migration and continue the ongoing replication.
- C. Order an AWS Snowball Edge Compute Optimized with GPU device. Use AWS Database Migration Service (AWS DMS) with AWS Schema Conversion Tool (AWS SCT) to migrate the database with ongoing changes. Send the Snowball device to AWS to finish the migration and continue the ongoing replication.
- D. Order a 1 GB dedicated AWS Direct Connect connection to establish a connection with the data center. Use AWS Database Migration Service (AWS DMS) with AWS Schema Conversion Tool (AWS SCT) to migrate the database with replication of ongoing changes.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* For 20 TB, Snowball Edge is cost-effective and can be used with DMS to seed the initial data. DMS can then replicate ongoing changes during the cutover. Snowmobile (B) is for petabytes. Compute Optimized (C) is not needed. Direct Connect (D) setup time and cost may exceed 2 weeks. (AWS Docs: DMS with Snowball).
</details>

---

**Q436.** A company moved its on-premises PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. The company successfully launched a new product. The workload on the database has increased. The company wants to accommodate the larger workload without adding infrastructure. Which solution will meet these requirements MOST cost-effectively?

- A. Buy reserved DB instances for the total workload. Make the Amazon RDS for PostgreSQL DB instance larger.
- B. Make the Amazon RDS for PostgreSQL DB instance a Multi-AZ DB instance.
- C. Buy reserved DB instances for the total workload. Add another Amazon RDS for PostgreSQL DB instance.
- D. Make the Amazon RDS for PostgreSQL DB instance an on-demand DB instance.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* To handle larger workload without adding infrastructure (i.e., more instances), scale up the existing instance to a larger class. Reserved Instances provide cost savings. Multi-AZ (B) adds HA, not performance. Adding another instance (C) adds infrastructure. On-demand (D) is more expensive. (AWS Docs: RDS Scaling Up).
</details>

---

**Q437.** A company operates an ecommerce website on Amazon EC2 instances behind an Application Load Balancer (ALB) in an Auto Scaling group. The site is experiencing performance issues related to a high request rate from illegitimate external systems with changing IP addresses. The security team is worried about potential DDoS attacks against the website. The company must block the illegitimate incoming requests in a way that has a minimal impact on legitimate users. What should a solutions architect recommend?

- A. Deploy Amazon Inspector and associate it with the ALB.
- B. Deploy AWS WAF, associate it with the ALB, and configure a rate-limiting rule.
- C. Deploy rules to the network ACLs associated with the ALB to block the incoming traffic.
- D. Deploy Amazon GuardDuty and enable rate-limiting protection when configuring GuardDuty.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS WAF with a rate-based rule can limit requests from IP addresses that exceed a threshold, blocking illegitimate traffic while allowing legitimate users. Inspector (A) is for vulnerability scanning. NACLs (C) are stateless and less effective. GuardDuty (D) is for detection, not mitigation. (AWS Docs: WAF Rate-Based Rules).
</details>

---

**Q438.** A company wants to share accounting data with an external auditor. The data is stored in an Amazon RDS DB instance that resides in a private subnet. The auditor has its own AWS account and requires its own copy of the database. What is the MOST secure way for the company to share the database with the auditor?

- A. Create a read replica of the database. Configure IAM standard database authentication to grant the auditor access.
- B. Export the database contents to text files. Store the files in an Amazon S3 bucket. Create a new IAM user for the auditor. Grant the user access to the S3 bucket.
- C. Copy a snapshot of the database to an Amazon S3 bucket. Create an IAM user. Share the user's keys with the auditor to grant access to the object in the S3 bucket.
- D. Create an encrypted snapshot of the database. Share the snapshot with the auditor. Allow access to the AWS Key Management Service (AWS KMS) encryption key.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Create an encrypted snapshot, share it with the auditor's account, and grant their account access to the KMS key used for encryption. This is secure and gives them their own copy. Read replica (A) would still require network access. Export to S3 (B, C) is less secure and may expose data. (AWS Docs: Sharing Encrypted RDS Snapshots).
</details>

---

**Q439.** A solutions architect configured a VPC that has a small range of IP addresses. The number of Amazon EC2 instances that are in the VPC is increasing, and there is an insufficient number of IP addresses for future workloads. Which solution resolves this issue with the LEAST operational overhead?

- A. Add an additional IPv4 CIDR block to increase the number of IP addresses and create additional subnets in the VPC. Create new resources in the new subnets by using the new CIDR.
- B. Create a second VPC with additional subnets. Use a peering connection to connect the second VPC with the first VPC. Update the routes and create new resources in the subnets of the second VPC.
- C. Use AWS Transit Gateway to add a transit gateway and connect a second VPC with the first VPC. Update the routes of the transit gateway and VPCs. Create new resources in the subnets of the second VPC.
- D. Create a second VPC. Create a Site-to-Site VPN connection between the first VPC and the second VPC by using a VPN-hosted solution on Amazon EC2 and a virtual private gateway. Update the route between VPCs to the traffic through the VPN. Create new resources in the subnets of the second VPC.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Adding a secondary IPv4 CIDR block to the existing VPC is the least operational overhead. You can then create new subnets with the new CIDR. Peering/Transit Gateway/VPN (B, C, D) add complexity and overhead. (AWS Docs: VPC Secondary CIDRs).
</details>

---

**Q440.** A company used an Amazon RDS for MySQL DB instance during application testing. Before terminating the DB instance at the end of the test cycle, a solutions architect created two backups. The solutions architect created the first backup by using the mysqldump utility to create a database dump. The solutions architect created the second backup by enabling the final DB snapshot option on RDS termination. The company is now planning for a new test cycle and wants to create a new DB instance from the most recent backup. The company has chosen a MySQL-compatible edition of Amazon Aurora to host the DB instance. Which solutions will create the new DB instance? (Choose two.)

- A. Import the RDS snapshot directly into Aurora.
- B. Upload the RDS snapshot to Amazon S3. Then import the RDS snapshot into Aurora.
- C. Upload the database dump to Amazon S3. Then import the database dump into Aurora.
- D. Use AWS Database Migration Service (AWS DMS) to import the RDS snapshot into Aurora.
- E. Upload the database dump to Amazon S3. Then use AWS Database Migration Service (AWS DMS) to import the database dump into Aurora.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* Aurora MySQL can be restored directly from an RDS for MySQL snapshot (A). Also, you can upload a mysqldump file to S3 and import it into Aurora (C). Option B (upload snapshot to S3) is not a direct process. Option D (DMS) can migrate but is more complex. Option E (DMS with dump) is not needed. (AWS Docs: Migrating RDS to Aurora, Aurora Import from S3).
</details>

---

**Q441.** A company hosts a multi-tier web application on Amazon Linux Amazon EC2 instances behind an Application Load Balancer. The instances run in an Auto Scaling group across multiple Availability Zones. The company observes that the Auto Scaling group launches more On-Demand Instances when the application's end users access high volumes of static web content. The company wants to optimize cost. What should a solutions architect do to redesign the application MOST cost-effectively?

- A. Update the Auto Scaling group to use Reserved Instances instead of On-Demand Instances.
- B. Update the Auto Scaling group to scale by launching Spot Instances instead of On-Demand Instances.
- C. Create an Amazon CloudFront distribution to host the static web contents from an Amazon S3 bucket.
- D. Create an AWS Lambda function behind an Amazon API Gateway API to host the static website contents.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Offloading static content to S3 + CloudFront reduces load on EC2 instances, allowing the Auto Scaling group to scale less, reducing costs. Reserved/Spot (A, B) address pricing but not the root cause. Lambda (D) is for dynamic content. (AWS Docs: CloudFront for Static Content).
</details>

---

**Q442.** A company stores several petabytes of data across multiple AWS accounts. The company uses AWS Lake Formation to manage its data lake. The company's data science team wants to securely share selective data from its accounts with the company's engineering team for analytical purposes. Which solution will meet these requirements with the LEAST operational overhead?

- A. Copy the required data to a common account. Create an IAM access role in that account. Grant access by specifying a permission policy that includes users from the engineering team accounts as trusted entities.
- B. Use the Lake Formation permissions Grant command in each account where the data is stored to allow the required engineering team users to access the data.
- C. Use AWS Data Exchange to privately publish the required data to the required engineering team accounts.
- D. Use Lake Formation tag-based access control to authorize and grant cross-account permissions for the required data to the engineering team accounts.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Lake Formation supports cross-account data sharing via tag-based access control or named resource permissions, with minimal overhead. Option A (copying) duplicates data. Option B (per account grants) is manual. Option C (Data Exchange) is for third-party data. (AWS Docs: Lake Formation Cross-Account Sharing).
</details>

---

**Q443.** A company wants to host a scalable web application on AWS. The application will be accessed by users from different geographic regions of the world. Application users will be able to download and upload unique data up to gigabytes in size. The development team wants a cost-effective solution to minimize upload and download latency and maximize performance. What should a solutions architect do to accomplish this?

- A. Use Amazon S3 with Transfer Acceleration to host the application.
- B. Use Amazon S3 with CacheControl headers to host the application.
- C. Use Amazon EC2 with Auto Scaling and Amazon CloudFront to host the application.
- D. Use Amazon EC2 with Auto Scaling and Amazon ElastiCache to host the application.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* CloudFront reduces download latency via edge caching. EC2 with Auto Scaling provides scalable compute for dynamic content. S3 (A, B) is for static hosting. ElastiCache (D) is for caching database queries, not file upload/download. (AWS Docs: CloudFront for Global Apps).
</details>

---

**Q444.** A company has hired a solutions architect to design a reliable architecture for its application. The application consists of one Amazon RDS DB instance and two manually provisioned Amazon EC2 instances that run web servers. The EC2 instances are located in a single Availability Zone. An employee recently deleted the DB instance, and the application was unavailable for 24 hours as a result. The company is concerned with the overall reliability of its environment. What should the solutions architect do to maximize reliability of the application's infrastructure?

- A. Delete one EC2 instance and enable termination protection on the other EC2 instance. Update the DB instance to be Multi-AZ, and enable deletion protection.
- B. Update the DB instance to be Multi-AZ, and enable deletion protection. Place the EC2 instances behind an Application Load Balancer, and run them in an EC2 Auto Scaling group across multiple Availability Zones.
- C. Create an additional DB instance along with an Amazon API Gateway and an AWS Lambda function. Configure the application to invoke the Lambda function through API Gateway. Have the Lambda function write the data to the two DB instances.
- D. Place the EC2 instances in an EC2 Auto Scaling group that has multiple subnets located in multiple Availability Zones. Use Spot Instances instead of On-Demand Instances. Set up Amazon CloudWatch alarms to monitor the health of the instances. Update the DB instance to be Multi-AZ, and enable deletion protection.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Multi-AZ RDS + deletion protection addresses DB reliability. EC2 Auto Scaling across multiple AZs with ALB provides HA for web tier. Option A reduces instances (less capacity). Option C (Lambda) is complex. Option D (Spot) is not recommended for critical web servers. (AWS Docs: High Availability Architecture).
</details>

---

**Q445.** A company is storing 700 terabytes of data on a large network-attached storage (NAS) system in its corporate data center. The company has a hybrid environment with a 10 Gbps AWS Direct Connect connection. After an audit from a regulator, the company has 90 days to move the data to the cloud. The company needs to move the data efficiently and without disruption. The company still needs to be able to access and update the data during the transfer window. Which solution will meet these requirements?

- A. Create an AWS DataSync agent in the corporate data center. Create a data transfer task. Start the transfer to an Amazon S3 bucket.
- B. Back up the data to AWS Snowball Edge Storage Optimized devices. Ship the devices to an AWS data center. Mount a target Amazon S3 bucket on the on-premises file system.
- C. Use rsync to copy the data directly from local storage to a designated Amazon S3 bucket over the Direct Connect connection.
- D. Back up the data on tapes. Ship the tapes to an AWS data center. Mount a target Amazon S3 bucket on the on-premises file system.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* DataSync can transfer 700 TB over 10 Gbps Direct Connect in about 700 TB * 8 / 10 Gbps = 560,000 seconds = ~6.5 days, well within 90 days. It supports ongoing changes during transfer. Snowball (B) would require offline transfer. rsync (C) is not managed. Tapes (D) are offline. (AWS Docs: DataSync Large Transfers).
</details>

---

**Q446.** A company stores data in PDF format in an Amazon S3 bucket. The company must follow a legal requirement to retain all new and existing data in Amazon S3 for 7 years. Which solution will meet these requirements with the LEAST operational overhead?

- A. Turn on the S3 Versioning feature for the S3 bucket. Configure S3 Lifecycle to delete the data after 7 years. Configure multi-factor authentication (MFA) delete for all S3 objects.
- B. Turn on S3 Object Lock with governance retention mode for the S3 bucket. Set the retention period to expire after 7 years. Recopy all existing objects to bring the existing data into compliance.
- C. Turn on S3 Object Lock with compliance retention mode for the S3 bucket. Set the retention period to expire after 7 years. Recopy all existing objects to bring the existing data into compliance.
- D. Turn on S3 Object Lock with compliance retention mode for the S3 bucket. Set the retention period to expire after 7 years. Use S3 Batch Operations to bring the existing data into compliance.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 Object Lock compliance mode prevents any deletion/overwrite for the retention period. S3 Batch Operations can apply the retention to existing objects efficiently. Option C (recopy) is manual. Option A (lifecycle delete) doesn't prevent early deletion. Governance mode (B) allows privileged users to bypass. (AWS Docs: S3 Object Lock Compliance Mode, Batch Operations).
</details>

---

**Q447.** A company has a stateless web application that runs on AWS Lambda functions that are invoked by Amazon API Gateway. The company wants to deploy the application across multiple AWS Regions to provide Regional failover capabilities. What should a solutions architect do to route traffic to multiple Regions?

- A. Create Amazon Route 53 health checks for each Region. Use an active-active failover configuration.
- B. Create an Amazon CloudFront distribution with an origin for each Region. Use CloudFront health checks to route traffic.
- C. Create a transit gateway. Attach the transit gateway to the API Gateway endpoint in each Region. Configure the transit gateway to route requests.
- D. Create an Application Load Balancer in the primary Region. Set the target group to point to the API Gateway endpoint hostnames in each Region.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Route 53 can route traffic to multiple Regions (active-active) using latency-based or weighted routing with health checks. CloudFront (B) is for caching, not failover. Transit gateway (C) is for VPC networking, not API Gateway endpoints. ALB (D) cannot route to cross-Region API Gateway endpoints natively. (AWS Docs: Route 53 Multi-Region Failover).
</details>

---

**Q448.** A company has two VPCs named Management and Production. The Management VPC uses VPNs through a customer gateway to connect to a single device in the data center. The Production VPC uses a virtual private gateway with two attached AWS Direct Connect connections. The Management and Production VPCs both use a single VPC peering connection to allow communication between the applications. What should a solutions architect do to mitigate any single point of failure in this architecture?

- A. Add a set of VPNs between the Management and Production VPCs.
- B. Add a second virtual private gateway and attach it to the Management VPC.
- C. Add a second set of VPNs to the Management VPC from a second customer gateway device.
- D. Add a second VPC peering connection between the Management VPC and the Production VPC.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* The single point of failure is the single customer gateway device/connection for the Management VPC. Adding a second VPN from a second customer gateway device provides redundancy. VPC peering (D) is not a single point of failure (it's a logical connection). Virtual private gateway (B) is already redundant. (AWS Docs: VPN Redundancy).
</details>

---

**Q449.** A company runs its application on an Oracle database. The company plans to quickly migrate to AWS because of limited resources for the database, backup administration, and data center maintenance. The application uses third-party database features that require privileged access. Which solution will help the company migrate the database to AWS MOST cost-effectively?

- A. Migrate the database to Amazon RDS for Oracle. Replace third-party features with cloud services.
- B. Migrate the database to Amazon RDS Custom for Oracle. Customize the database settings to support third-party features.
- C. Migrate the database to an Amazon EC2 Amazon Machine Image (AMI) for Oracle. Customize the database settings to support third-party features.
- D. Migrate the database to Amazon RDS for PostgreSQL by rewriting the application code to remove dependency on Oracle APEX.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* RDS Custom for Oracle provides privileged access (to the underlying OS/database) while still offloading many management tasks (backups, patching). It is more cost-effective than full EC2 (C) and less restrictive than standard RDS (A). Option D requires code changes. (AWS Docs: RDS Custom for Oracle).
</details>

---

**Q450.** A company has a three-tier web application that is in a single server. The company wants to migrate the application to the AWS Cloud. The company also wants the application to align with the AWS Well-Architected Framework and to be consistent with AWS recommended best practices for security, scalability, and resiliency. Which combination of solutions will meet these requirements? (Choose three.)

- A. Create a VPC across two Availability Zones with the application's existing architecture. Host the application with existing architecture on an Amazon EC2 instance in a private subnet in each Availability Zone with EC2 Auto Scaling groups. Secure the EC2 instance with security groups and network access control lists (network ACLs).
- B. Set up security groups and network access control lists (network ACLs) to control access to the database layer. Set up a single Amazon RDS database in a private subnet.
- C. Create a VPC across two Availability Zones. Refactor the application to host the web tier, application tier, and database tier. Host each tier on its own private subnet with Auto Scaling groups for the web tier and application tier.
- D. Use a single Amazon RDS database. Allow database access only from the application tier security group.
- E. Use Elastic Load Balancers in front of the web tier. Control access by using security groups containing references to each layer's security groups.
- F. Use an Amazon RDS database Multi-AZ cluster deployment in private subnets. Allow database access only from application tier security groups.

<details>
<summary>Show Answer</summary>

**Answer: C, E, F**

*Explanation:* C (multi-tier, multi-AZ, Auto Scaling) provides scalability/resiliency. E (ELB, security group references) provides HA and security. F (RDS Multi-AZ, private subnets, security group restrictions) provides HA and security. Option A (single instance per AZ) is not scalable. Option B (single RDS) is not HA. Option D (single RDS) is not HA. (AWS Docs: Well-Architected Framework Best Practices).
</details>

---

**Q451.** A company is migrating its applications and databases to the AWS Cloud. The company will use Amazon Elastic Container Service (Amazon ECS), AWS Direct Connect, and Amazon RDS. Which activities will be managed by the company's operational team? (Choose three.)

- A. Management of the Amazon RDS infrastructure layer, operating system, and platforms
- B. Creation of an Amazon RDS DB instance and configuring the scheduled maintenance window
- C. Configuration of additional software components on Amazon ECS for monitoring, patch management, log management, and host intrusion detection
- D. Installation of patches for all minor and major database versions for Amazon RDS
- E. Ensure the physical security of the Amazon RDS infrastructure in the data center
- F. Encryption of the data that moves in transit through Direct Connect

<details>
<summary>Show Answer</summary>

**Answer: B, C, F**

*Explanation:* Customer is responsible for creating RDS instances & maintenance windows (B), configuring ECS software (C), and encrypting data over Direct Connect (F). AWS manages RDS infrastructure/OS/patching (A, D) and physical security (E). (AWS Docs: Shared Responsibility Model).
</details>

---

**Q452.** A company runs a Java-based job on an Amazon EC2 instance. The job runs every hour and takes 10 seconds to run. The job runs on a scheduled interval and consumes 1 GB of memory. The CPU utilization of the instance is low except for short surges during which the job uses the maximum CPU available. The company wants to optimize the costs to run the job. Which solution will meet these requirements?

- A. Use AWS App2Container (A2C) to containerize the job. Run the job as an Amazon Elastic Container Service (Amazon ECS) task on AWS Fargate with 0.5 virtual CPU (vCPU) and 1 GB of memory.
- B. Copy the code into an AWS Lambda function that has 1 GB of memory. Create an Amazon EventBridge scheduled rule to run the code each hour.
- C. Use AWS App2Container (A2C) to containerize the job. Install the container in the existing Amazon Machine Image (AMI). Ensure that the schedule stops the container when the task finishes.
- D. Configure the existing schedule to stop the EC2 instance at the completion of the job and restart the EC2 instance when the next job starts.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Lambda can run a 10-second job with 1 GB memory, and you pay only per invocation. This is more cost-effective than keeping an EC2 instance or even Fargate (which has minimum billing). Option A (Fargate) has minimum billing of 1 minute. Option C still uses EC2. Option D (stop/start) may not save enough. (AWS Docs: Lambda for Scheduled Jobs).
</details>

---

**Q453.** A company wants to implement a backup strategy for Amazon EC2 data and multiple Amazon S3 buckets. Because of regulatory requirements, the company must retain backup files for a specific time period. The company must not alter the files for the duration of the retention period. Which solution will meet these requirements?

- A. Use AWS Backup to create a backup vault that has a vault lock in governance mode. Create the required backup plan.
- B. Use Amazon Data Lifecycle Manager to create the required automated snapshot policy.
- C. Use Amazon S3 File Gateway to create the backup. Configure the appropriate S3 Lifecycle management.
- D. Use AWS Backup to create a backup vault that has a vault lock in compliance mode. Create the required backup plan.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Backup Vault Lock in compliance mode enforces immutability (cannot be altered or deleted) for the retention period, even by root. Governance mode (A) can be bypassed by authorized users. DLM (B) is only for EBS snapshots, not S3. File Gateway (C) is not for backup immutability. (AWS Docs: AWS Backup Vault Lock).
</details>

---

**Q454.** A company has resources across multiple AWS Regions and accounts. A newly hired solutions architect discovers a previous employee did not provide details about the resources inventory. The solutions architect needs to build and map the relationship details of the various workloads across all accounts. Which solution will meet these requirements in the MOST operationally efficient way?

- A. Use AWS Systems Manager Inventory to generate a map view from the detailed view report.
- B. Use AWS Step Functions to collect workload details. Build architecture diagrams of the workloads manually.
- C. Use Workload Discovery on AWS to generate architecture diagrams of the workloads.
- D. Use AWS X-Ray to view the workload details. Build architecture diagrams with relationships.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Workload Discovery on AWS (formerly AWS Perspective) is a tool that automatically discovers and visualizes AWS resources across Regions/accounts, generating architecture diagrams. Systems Manager Inventory (A) is for instance inventory. Step Functions (B) and X-Ray (D) are not designed for this. (AWS Docs: Workload Discovery on AWS).
</details>

---

**Q455.** A company uses AWS Organizations. The company wants to operate some of its AWS accounts with different budgets. The company wants to receive alerts and automatically prevent provisioning of additional resources on AWS accounts when the allocated budget threshold is met during a specific period. Which combination of solutions will meet these requirements? (Choose three.)

- A. Use AWS Budgets to create a budget. Set the budget amount under the Cost and Usage Reports section of the required AWS accounts.
- B. Use AWS Budgets to create a budget. Set the budget amount under the Billing dashboards of the required AWS accounts.
- C. Create an IAM user for AWS Budgets to run budget actions with the required permissions.
- D. Create an IAM role for AWS Budgets to run budget actions with the required permissions.
- E. Add an alert to notify the company when each account meets its budget threshold. Add a budget action that selects the IAM identity created with the appropriate config rule to prevent provisioning of additional resources.
- F. Add an alert to notify the company when each account meets its budget threshold. Add a budget action that selects the IAM identity created with the appropriate service control policy (SCP) to prevent provisioning of additional resources.

<details>
<summary>Show Answer</summary>

**Answer: B, D, F**

*Explanation:* Create budget in Billing console (B). Create an IAM role for Budgets to take actions (D). Budget action can apply an SCP to prevent resource creation (F). Option A (CUR section) is not correct. Option C (IAM user) is less secure than role. Option E (config rule) is not for prevention. (AWS Docs: AWS Budgets Actions).
</details>

---

**Q456.** A company runs applications on Amazon EC2 instances in one AWS Region. The company wants to back up the EC2 instances to a second Region. The company also wants to provision EC2 resources in the second Region and manage the EC2 instances centrally from one AWS account. Which solution will meet these requirements MOST cost-effectively?

- A. Create a disaster recovery (DR) plan that has a similar number of EC2 instances in the second Region. Configure data replication.
- B. Create point-in-time Amazon Elastic Block Store (Amazon EBS) snapshots of the EC2 instances. Copy the snapshots to the second Region periodically.
- C. Create a backup plan by using AWS Backup. Configure cross-Region backup to the second Region for the EC2 instances.
- D. Deploy a similar number of EC2 instances in the second Region. Use AWS DataSync to transfer the data from the source Region to the second Region.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Backup provides cross-Region backup for EC2 (via AMIs or snapshots) with minimal configuration, and you can centrally manage backups. Option A (similar number of instances) would be costly (running instances). Option B (manual snapshots) is less efficient. Option D (DataSync) requires running instances. (AWS Docs: AWS Backup Cross-Region).
</details>

---

**Q457.** A company that uses AWS is building an application to transfer data to a product manufacturer. The company has its own identity provider (IdP). The company wants the IdP to authenticate application users while the users use the application to transfer data. The company must use Applicability Statement 2 (AS2) protocol. Which solution will meet these requirements?

- A. Use AWS DataSync to transfer the data. Create an AWS Lambda function for IdP authentication.
- B. Use Amazon AppFlow flows to transfer the data. Create an Amazon Elastic Container Service (Amazon ECS) task for IdP authentication.
- C. Use AWS Transfer Family to transfer the data. Create an AWS Lambda function for IdP authentication.
- D. Use AWS Storage Gateway to transfer the data. Create an Amazon Cognito identity pool for IdP authentication.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Transfer Family supports AS2 protocol for B2B messaging. It can integrate with custom identity providers (Lambda) for authentication. DataSync (A) does not support AS2. AppFlow (B) is for SaaS apps. Storage Gateway (D) is for storage. (AWS Docs: Transfer Family AS2).
</details>

---

**Q458.** A solutions architect is designing a REST API in Amazon API Gateway for a cash payback service. The application requires 1 GB of memory and 2 GB of storage for its computation resources. The application will require that the data is in a relational format. Which additional combination of AWS services will meet these requirements with the LEAST administrative effort? (Choose two.)

- A. Amazon EC2
- B. AWS Lambda
- C. Amazon RDS
- D. Amazon DynamoDB
- E. Amazon Elastic Kubernetes Services (Amazon EKS)

<details>
<summary>Show Answer</summary>

**Answer: B, C**

*Explanation:* Lambda (B) can provide compute with up to 10 GB memory. RDS (C) provides relational database. This combination minimizes administrative effort (serverless compute, managed DB). EC2 (A) and EKS (E) require more administration. DynamoDB (D) is non-relational. (AWS Docs: Lambda Memory, RDS).
</details>

---

**Q459.** A company uses AWS Organizations to run workloads within multiple AWS accounts. A tagging policy adds department tags to AWS resources when the company creates tags. An accounting team needs to determine spending on Amazon EC2 consumption. The accounting team must determine which departments are responsible for the costs regardless of AWS account. The accounting team has access to AWS Cost Explorer for all AWS accounts within the organization and needs to access all reports from Cost Explorer. Which solution meets these requirements in the MOST operationally efficient way?

- A. From the Organizations management account billing console, activate a user-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by tag name, and filter by EC2.
- B. From the Organizations management account billing console, activate an AWS-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by tag name, and filter by EC2.
- C. From the Organizations member account billing console, activate a user-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by the tag name, and filter by EC2.
- D. From the Organizations member account billing console, activate an AWS-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by tag name, and filter by EC2.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* User-defined cost allocation tags must be activated in the management account billing console. Then Cost Explorer can group by tag name across all accounts. AWS-defined tags (B, D) are not user-defined. Member account (C, D) cannot activate tags for the organization. (AWS Docs: Cost Allocation Tags).
</details>

---

**Q460.** A company wants to securely exchange data between its software as a service (SaaS) application Salesforce account and Amazon S3. The company must encrypt the data at rest by using AWS Key Management Service (AWS KMS) customer managed keys (CMKs). The company must also encrypt the data in transit. The company has enabled API access for the Salesforce account. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create AWS Lambda functions to transfer the data securely from Salesforce to Amazon S3.
- B. Create an AWS Step Functions workflow. Define the task to transfer the data securely from Salesforce to Amazon S3.
- C. Create Amazon AppFlow flows to transfer the data securely from Salesforce to Amazon S3.
- D. Create a custom connector for Salesforce to transfer the data securely from Salesforce to Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon AppFlow is a fully managed integration service that supports Salesforce to S3 transfers, with built-in encryption in transit and at rest (using KMS CMKs). This has the least operational overhead. Custom Lambda/Step Functions (A, B) or custom connector (D) require more effort. (AWS Docs: Amazon AppFlow).
</details>

---

**Q461.** A company is developing a mobile gaming app in a single AWS Region. The app runs on multiple Amazon EC2 instances in an Auto Scaling group. The company stores the app data in Amazon DynamoDB. The app communicates by using TCP traffic and UDP traffic between the users and the servers. The application will be used globally. The company wants to ensure the lowest possible latency for all users. Which solution will meet these requirements?

- A. Use AWS Global Accelerator to create an accelerator. Create an Application Load Balancer (ALB) behind an accelerator endpoint that uses Global Accelerator integration and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the ALB.
- B. Use AWS Global Accelerator to create an accelerator. Create a Network Load Balancer (NLB) behind an accelerator endpoint that uses Global Accelerator integration and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the NLB.
- C. Create an Amazon CloudFront content delivery network (CDN) endpoint. Create a Network Load Balancer (NLB) behind the endpoint and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the NLB. Update CloudFront to use the NLB as the origin.
- D. Create an Amazon CloudFront content delivery network (CDN) endpoint. Create an Application Load Balancer (ALB) behind the endpoint and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the ALB. Update CloudFront to use the ALB as the origin.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Global Accelerator supports TCP/UDP, provides low-latency routing via edge locations, and integrates with NLBs (which support TCP/UDP). ALBs (A, D) do not support UDP. CloudFront (C, D) does not support UDP origins. (AWS Docs: Global Accelerator UDP).
</details>

---

**Q462.** A company has an application that processes customer orders. The company hosts the application on an Amazon EC2 instance that saves the orders to an Amazon Aurora database. Occasionally when traffic is high the workload does not process orders fast enough. What should a solutions architect do to write the orders reliably to the database as quickly as possible?

- A. Increase the instance size of the EC2 instance when traffic is high. Write orders to Amazon Simple Notification Service (Amazon SNS). Subscribe the database endpoint to the SNS topic.
- B. Write orders to an Amazon Simple Queue Service (Amazon SQS) queue. Use EC2 instances in an Auto Scaling group behind an Application Load Balancer to read from the SQS queue and process orders into the database.
- C. Write orders to Amazon Simple Notification Service (Amazon SNS). Subscribe the database endpoint to the SNS topic. Use EC2 instances in an Auto Scaling group behind an Application Load Balancer to read from the SNS topic.
- D. Write orders to an Amazon Simple Queue Service (Amazon SQS) queue when the EC2 instance reaches CPU threshold limits. Use scheduled scaling of EC2 instances in an Auto Scaling group behind an Application Load Balancer to read from the SQS queue and process orders into the database.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Using SQS decouples order intake from processing, and Auto Scaling based on queue depth ensures processing scales with load. This provides reliability and speed. Option A (SNS to database) is not reliable (SNS doesn't buffer). Option C (SNS) doesn't buffer. Option D (conditional queue) adds complexity. (AWS Docs: SQS for Order Processing).
</details>

---

**Q463.** An IoT company is releasing a mattress that has sensors to collect data about a user's sleep. The sensors will send data to an Amazon S3 bucket. The sensors collect approximately 2 MB of data every night for each mattress. The company must process and summarize the data for each mattress. The results need to be available as soon as possible. Data processing will require 1 GB of memory and will finish within 30 seconds. Which solution will meet these requirements MOST cost-effectively?

- A. Use AWS Glue with a Scala job
- B. Use Amazon EMR with an Apache Spark script
- C. Use AWS Lambda with a Python script
- D. Use AWS Glue with a PySpark job

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Lambda can handle 1 GB memory and 30 seconds runtime. It is serverless and cost-effective (pay per invocation). Glue (A, D) and EMR (B) have minimum billing and higher overhead for small, infrequent jobs. (AWS Docs: Lambda Limits).
</details>

---

**Q464.** A company hosts an online shopping application that stores all orders in an Amazon RDS for PostgreSQL Single-AZ DB instance. Management wants to eliminate single points of failure and has asked a solutions architect to recommend an approach to minimize database downtime without requiring any changes to the application code. Which solution meets these requirements?

- A. Convert the existing database instance to a Multi-AZ deployment by modifying the database instance and specifying the Multi-AZ option.
- B. Create a new RDS Multi-AZ deployment. Take a snapshot of the current RDS instance and restore the new Multi-AZ deployment with the snapshot.
- C. Create a read-only replica of the PostgreSQL database in another Availability Zone. Use Amazon Route 53 weighted record sets to distribute requests across the databases.
- D. Place the RDS for PostgreSQL database in an Amazon EC2 Auto Scaling group with a minimum group size of two. Use Amazon Route 53 weighted record sets to distribute requests across instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* You can modify an existing Single-AZ RDS instance to Multi-AZ with zero downtime (just a reboot). No application changes required. Option B (snapshot restore) requires downtime. Option C (read replica) does not provide failover for writes. Option D (EC2) is not managed. (AWS Docs: RDS Multi-AZ Modification).
</details>

---

**Q465.** A company is developing an application to support customer demands. The company wants to deploy the application on multiple Amazon EC2 Nitro-based instances within the same Availability Zone. The company also wants to give the application the ability to write to multiple block storage volumes in multiple EC2 Nitro-based instances simultaneously to achieve higher application availability. Which solution will meet these requirements?

- A. Use General Purpose SSD (gp3) EBS volumes with Amazon Elastic Block Store (Amazon EBS) Multi-Attach
- B. Use Throughput Optimized HDD (st1) EBS volumes with Amazon Elastic Block Store (Amazon EBS) Multi-Attach
- C. Use Provisioned IOPS SSD (io2) EBS volumes with Amazon Elastic Block Store (Amazon EBS) Multi-Attach
- D. Use General Purpose SSD (gp2) EBS volumes with Amazon Elastic Block Store (Amazon EBS) Multi-Attach

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* EBS Multi-Attach is supported only on io1/io2 volumes. io2 is the current generation. gp2/gp3 (A, D) and st1 (B) do not support Multi-Attach. (AWS Docs: EBS Multi-Attach).
</details>

---

**Q466.** A company designed a stateless two-tier application that uses Amazon EC2 in a single Availability Zone and an Amazon RDS Multi-AZ DB instance. New company management wants to ensure the application is highly available. What should a solutions architect do to meet this requirement?

- A. Configure the application to use Multi-AZ EC2 Auto Scaling and create an Application Load Balancer.
- B. Configure the application to take snapshots of the EC2 instances and send them to a different AWS Region.
- C. Configure the application to use Amazon Route 53 latency-based routing to feed requests to the application.
- D. Configure Amazon Route 53 rules to handle incoming requests and create a Multi-AZ Application Load Balancer.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* To make the stateless application highly available, use EC2 Auto Scaling across multiple Availability Zones with an Application Load Balancer. RDS is already Multi-AZ. Option B (snapshots) is backup, not HA. Option C/D (Route 53) alone doesn't provide HA for EC2. (AWS Docs: Auto Scaling Multi-AZ).
</details>

---

**Q467.** A company uses AWS Organizations. A member account has purchased a Compute Savings Plan. Because of changes in the workloads inside the member account, the account no longer receives the full benefit of the Compute Savings Plan commitment. The company uses less than 50% of its purchased compute power. What should the solutions architect recommend?

- A. Turn on discount sharing from the Billing Preferences section of the account console in the member account that purchased the Compute Savings Plan.
- B. Turn on discount sharing from the Billing Preferences section of the account console in the company's Organizations management account.
- C. Migrate additional compute workloads from another AWS account to the account that has the Compute Savings Plan.
- D. Sell the excess Savings Plan commitment in the Reserved Instance Marketplace.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Discount sharing for Savings Plans can be enabled in the management account's Billing Preferences, allowing the discount to be shared across all accounts in the organization. Option C (migrate workloads) is possible but not the primary recommendation. Option D (sell) is not available for Savings Plans (only RIs). (AWS Docs: Savings Plan Discount Sharing).
</details>

---

**Q468.** A company is developing a microservices application that will provide a search catalog for customers. The company must use REST APIs to present the frontend of the application to users. The REST APIs must access the backend services that the company hosts in containers in private VPC subnets. Which solution will meet these requirements?

- A. Design a WebSocket API by using Amazon API Gateway. Host the application in Amazon Elastic Container Service (Amazon ECS) in a private subnet. Create a private VPC link for API Gateway to access Amazon ECS.
- B. Design a REST API by using Amazon API Gateway. Host the application in Amazon Elastic Container Service (Amazon ECS) in a private subnet. Create a private VPC link for API Gateway to access Amazon ECS.
- C. Design a WebSocket API by using Amazon API Gateway. Host the application in Amazon Elastic Container Service (Amazon ECS) in a private subnet. Create a security group for API Gateway to access Amazon ECS.
- D. Design a REST API by using Amazon API Gateway. Host the application in Amazon Elastic Container Service (Amazon ECS) in a private subnet. Create a security group for API Gateway to access Amazon ECS.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* REST API is required. To allow API Gateway to access backend services in a private VPC, you create a VPC link (using Network Load Balancer). Security groups (C, D) alone are insufficient. (AWS Docs: API Gateway VPC Link).
</details>

---

**Q469.** A company stores raw collected data in an Amazon S3 bucket. The data is used for several types of analytics on behalf of the company's customers. The type of analytics requested determines the access pattern on the S3 objects. The company cannot predict or control the access pattern. The company wants to reduce its S3 costs. Which solution will meet these requirements?

- A. Use S3 replication to transition infrequently accessed objects to S3 Standard-Infrequent Access (S3 Standard-IA)
- B. Use S3 Lifecycle rules to transition objects from S3 Standard to Standard-Infrequent Access (S3 Standard-IA)
- C. Use S3 Lifecycle rules to transition objects from S3 Standard to S3 Intelligent-Tiering
- D. Use S3 Inventory to identify and transition objects that have not been accessed from S3 Standard to S3 Intelligent-Tiering

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* S3 Intelligent-Tiering automatically moves objects between frequent and infrequent access tiers based on changing access patterns, with no retrieval fees. It is ideal for unknown/unpredictable access patterns. Lifecycle to IA (B) would require knowing which objects are infrequent. (AWS Docs: S3 Intelligent-Tiering).
</details>

---

**Q470.** A company has applications hosted on Amazon EC2 instances with IPv6 addresses. The applications must initiate communications with other external applications using the internet. However the company's security policy states that any external service cannot initiate a connection to the EC2 instances. What should a solutions architect recommend to resolve this issue?

- A. Create a NAT gateway and make it the destination of the subnet's route table.
- B. Create an internet gateway and make it the destination of the subnet's route table.
- C. Create a virtual private gateway and make it the destination of the subnet's route table.
- D. Create an egress-only internet gateway and make it the destination of the subnet's route table.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* An egress-only internet gateway provides outbound-only internet access for IPv6, allowing instances to initiate connections to the internet but preventing inbound connections from the internet. NAT gateway (A) is for IPv4. Internet gateway (B) allows inbound. VPN gateway (C) is for VPN. (AWS Docs: Egress-Only Internet Gateway).
</details>

---

**Q471.** A company is creating an application that runs on containers in a VPC. The application stores and accesses data in an Amazon S3 bucket. During the development phase, the application will store and access 1 TB of data in Amazon S3 each day. The company wants to minimize costs and wants to prevent traffic from traversing the internet whenever possible. Which solution will meet these requirements?

- A. Enable S3 Intelligent-Tiering for the S3 bucket.
- B. Enable S3 Transfer Acceleration for the S3 bucket.
- C. Create a gateway VPC endpoint for Amazon S3. Associate this endpoint with all route tables in the VPC.
- D. Create an interface endpoint for Amazon S3 in the VPC. Associate this endpoint with all route tables in the VPC.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* A gateway VPC endpoint for S3 keeps traffic within AWS network (no internet), reducing data transfer costs. S3 does not support interface endpoints (D). Transfer Acceleration (B) speeds up uploads but uses internet. Intelligent-Tiering (A) optimizes storage cost, not data transfer. (AWS Docs: S3 Gateway Endpoint).
</details>

---

**Q472.** A company has a mobile chat application with a data store based in Amazon DynamoDB. Users would like new messages to be read with as little latency as possible. A solutions architect needs to design an optimal solution that requires minimal application changes. Which method should the solutions architect select?

- A. Configure Amazon DynamoDB Accelerator (DAX) for the new messages table. Update the code to use the DAX endpoint.
- B. Add DynamoDB read replicas to handle the increased read load. Update the application to point to the read endpoint for the read replicas.
- C. Double the number of read capacity units for the new messages table in DynamoDB. Continue to use the existing DynamoDB endpoint.
- D. Add an Amazon ElastiCache for Redis cache to the application stack. Update the application to point to the Redis cache endpoint instead of DynamoDB.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* DAX is an in-memory cache for DynamoDB that reduces read latency from milliseconds to microseconds, with minimal application changes (just change endpoint). DynamoDB does not have read replicas (B). Doubling RCUs (C) improves throughput but not latency significantly. Redis (D) requires more code changes. (AWS Docs: DAX).
</details>

---

**Q473.** A company hosts a website on Amazon EC2 instances behind an Application Load Balancer (ALB). The website serves static content. Website traffic is increasing, and the company is concerned about a potential increase in cost. What should the solutions architect do to reduce costs?

- A. Create an Amazon CloudFront distribution to cache static files at edge locations.
- B. Create an Amazon ElastiCache cluster. Connect the ALB to the ElastiCache cluster to serve cached files.
- C. Create an AWS WAF web ACL and associate it with the ALB. Add a rule to the web ACL to cache static files.
- D. Create a second ALB in an alternative AWS Region. Route user traffic to the closest Region to minimize data transfer costs.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudFront caches static content at edge locations, reducing load on EC2 instances and data transfer costs. ElastiCache (B) is for database caching. WAF (C) is for security, not caching. Second ALB (D) would increase costs. (AWS Docs: CloudFront Cost Reduction).
</details>

---

**Q474.** A company has multiple VPCs across AWS Regions to support and run workloads that are isolated from workloads in other Regions. Because of a recent application launch requirement, the company's VPCs must communicate with all other VPCs across all Regions. Which solution will meet these requirements with the LEAST amount of administrative effort?

- A. Use VPC peering to manage VPC communication in a single Region. Use VPC peering across Regions to manage VPC communications.
- B. Use AWS Direct Connect gateways across all Regions to connect VPCs across regions and manage VPC communications.
- C. Use AWS Transit Gateway to manage VPC communication in a single Region and Transit Gateway peering across Regions to manage VPC communications.
- D. Use AWS PrivateLink across all Regions to connect VPCs across Regions and manage VPC communications.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Transit Gateway simplifies VPC connectivity within a Region, and Transit Gateway peering connects across Regions, providing a scalable, central management point with less administrative effort than many VPC peering connections. Direct Connect gateways (B) are for on-premises. PrivateLink (D) is for service endpoints. (AWS Docs: Transit Gateway Peering).
</details>

---

**Q475.** A company is designing a containerized application that will use Amazon Elastic Container Service (Amazon ECS). The application needs to access a shared file system that is highly durable and can recover data to another AWS Region with a recovery point objective (RPO) of 8 hours. The file system needs to provide a mount target in each Availability Zone within a Region. A solutions architect wants to use AWS Backup to manage the replication to another Region. Which solution will meet these requirements?

- A. Amazon FSx for Windows File Server with a Multi-AZ deployment
- B. Amazon FSx for NetApp ONTAP with a Multi-AZ deployment
- C. Amazon Elastic File System (Amazon EFS) with the Standard storage class
- D. Amazon FSx for OpenZFS

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon EFS Standard provides mount targets in each AZ, high durability, and supports AWS Backup for cross-Region replication with RPO as low as 8 hours. FSx for Windows (A) and ONTAP (B) are more complex for this use case. OpenZFS (D) does not have native cross-Region backup. (AWS Docs: EFS Backup Cross-Region).
</details>

---

**Q476.** A company is expecting rapid growth in the near future. A solutions architect needs to configure existing users and grant permissions to new users on AWS. The solutions architect has decided to create IAM groups. The solutions architect will add the new users to IAM groups based on department. Which additional action is the MOST secure way to grant permissions to the new users?

- A. Apply service control policies (SCPs) to manage access permissions.
- B. Create IAM roles that have least privilege permission. Attach the roles to the IAM groups.
- C. Create an IAM policy that grants least privilege permission. Attach the policy to the IAM groups.
- D. Create IAM roles. Associate the roles with a permissions boundary that defines the maximum permissions.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* The standard way to grant permissions to users in groups is to create IAM policies with least privilege and attach them to the IAM groups. Roles (B) are assumed by users, not attached to groups directly. SCPs (A) are for organization-wide restrictions. Permissions boundaries (D) are for delegation, not direct permission granting. (AWS Docs: IAM Groups).
</details>

---

**Q477.** A group requires permissions to list an Amazon S3 bucket and delete objects from that bucket. An administrator has created the following IAM policy to provide access to the bucket and applied that policy to the group. The group is not able to delete objects in the bucket. The company follows least-privilege access rules. `{ "Version": "2012-10-17", "Statement": [ { "Action": [ "s3:ListBucket", "s3:DeleteObject" ], "Resource": [ "arn:aws:s3:::bucket-name" ], "Effect": "Allow" } ] }`. Which statement should a solutions architect add to the policy to correct bucket access?

- A. `{ "Action": [ "s3:Object" ], "Resource": [ "arn:aws:s3:::bucket-name/*" ], "Effect": "Allow" }`
- B. `{ "Action": [ "s3:DeleteObject" ], "Resource": [ "arn:aws:s3:::bucket-name/*" ], "Effect": "Allow" }`
- C. `{ "Action": [ "s3:DeleteObject" ], "Resource": [ "arn:aws:s3:::bucket-name/*" ], "Effect": "Allow" }` (duplicate)
- D. `{ "Action": [ "s3:DeleteObject" ], "Resource": [ "arn:aws:s3:::bucket-name/*" ], "Effect": "Allow" }` (duplicate)

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* The resource for `s3:DeleteObject` must be the object ARN (`bucket-name/*`), not just the bucket ARN. The original policy incorrectly used the bucket ARN for DeleteObject. Option B adds the correct resource. (AWS Docs: S3 IAM Resource Syntax).
</details>

---

**Q478.** A law firm needs to share information with the public. The information includes hundreds of files that must be publicly readable. Modifications or deletions of the files by anyone before a designated future date are prohibited. Which solution will meet these requirements in the MOST secure way?

- A. Upload all files to an Amazon S3 bucket that is configured for static website hosting. Grant read-only IAM permissions to any AWS principals that access the S3 bucket until the designated date.
- B. Create a new Amazon S3 bucket with S3 Versioning enabled. Use S3 Object Lock with a retention period in accordance with the designated date. Configure the S3 bucket for static website hosting. Set an S3 bucket policy to allow read-only access to the objects.
- C. Create a new Amazon S3 bucket with S3 Versioning enabled. Configure an event trigger to run an AWS Lambda function in case of object modification or deletion. Configure the Lambda function to replace the objects with the original versions from a private S3 bucket.
- D. Upload all files to an Amazon S3 bucket that is configured for static website hosting. Select the folder that contains the files. Use S3 Object Lock with a retention period in accordance with the designated date. Grant read-only IAM permissions to any AWS principals that access the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Object Lock with a retention period prevents deletion/modification. Public read access via bucket policy (or static website hosting) makes files publicly readable. Versioning (B) is required for Object Lock. Option A (read-only IAM) would not allow public access without AWS credentials. Option C (Lambda) is reactive. Option D (IAM permissions) not public. (AWS Docs: S3 Object Lock for Public Content).
</details>

---

**Q479.** A company is making a prototype of the infrastructure for its new website by manually provisioning the necessary infrastructure. This infrastructure includes an Auto Scaling group, an Application Load Balancer and an Amazon RDS database. After the configuration has been thoroughly validated, the company wants the capability to immediately deploy the infrastructure for development and production use in two Availability Zones in an automated fashion. What should a solutions architect recommend to meet these requirements?

- A. Use AWS Systems Manager to replicate and provision the prototype infrastructure in two Availability Zones.
- B. Define the infrastructure as a template by using the prototype infrastructure as a guide. Deploy the infrastructure with AWS CloudFormation.
- C. Use AWS Config to record the inventory of resources that are used in the prototype infrastructure. Use AWS Config to deploy the prototype infrastructure into two Availability Zones.
- D. Use AWS Elastic Beanstalk and configure it to use an automated reference to the prototype infrastructure to automatically deploy new environments in two Availability Zones.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS CloudFormation allows you to define infrastructure as code (template) and deploy it consistently across environments. This is the standard way to automate deployment. Systems Manager (A) is for management. Config (C) is for compliance, not deployment. Elastic Beanstalk (D) could work but CloudFormation is more flexible. (AWS Docs: CloudFormation).
</details>

---

**Q480.** A business application is hosted on Amazon EC2 and uses Amazon S3 for encrypted object storage. The chief information security officer has directed that no application traffic between the two services should traverse the public internet. Which capability should the solutions architect use to meet the compliance requirements?

- A. AWS Key Management Service (AWS KMS)
- B. VPC endpoint
- C. Private subnet
- D. Virtual private gateway

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* A VPC endpoint (gateway endpoint) for S3 ensures traffic between EC2 and S3 stays within the AWS network, not the public internet. KMS (A) is for encryption. Private subnet (C) alone doesn't guarantee private path to S3. Virtual private gateway (D) is for on-premises. (AWS Docs: S3 VPC Endpoint).
</details>

---

**Q481.** A company hosts a three-tier web application in the AWS Cloud. A Multi-AZ Amazon RDS for MySQL server forms the database layer. Amazon ElastiCache forms the cache layer. The company wants a caching strategy that adds or updates data in the cache when a customer adds an item to the database. The data in the cache must always match the data in the database. Which solution will meet these requirements?

- A. Implement the lazy loading caching strategy
- B. Implement the write-through caching strategy
- C. Implement the adding TTL caching strategy
- D. Implement the AWS AppConfig caching strategy

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Write-through caching writes data to both the cache and the database simultaneously, ensuring consistency. Lazy loading (A) loads data on cache miss, which can lead to stale data. TTL (C) eventually expires but doesn't guarantee immediate consistency. AppConfig (D) is for configuration management. (AWS Docs: Write-Through Caching).
</details>

---

**Q482.** A company wants to migrate 100 GB of historical data from an on-premises location to an Amazon S3 bucket. The company has a 100 megabits per second (Mbps) internet connection on premises. The company needs to encrypt the data in transit to the S3 bucket. The company will store new data directly in Amazon S3. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use the s3 sync command in the AWS CLI to move the data directly to an S3 bucket.
- B. Use AWS DataSync to migrate the data from the on-premises location to an S3 bucket.
- C. Use AWS Snowball to move the data to an S3 bucket.
- D. Set up an IPsec VPN from the on-premises location to AWS. Use the s3 cp command in the AWS CLI to move the data directly to an S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* For 100 GB over 100 Mbps, transfer time is ~100 GB * 8 / 100 Mbps = 8,000 seconds = ~2.2 hours, which is fine. `s3 sync` over HTTPS encrypts in transit and requires minimal setup. DataSync (B) is for larger/more complex transfers. Snowball (C) is overkill. VPN + CLI (D) adds unnecessary complexity. (AWS Docs: AWS CLI S3).
</details>

---

**Q483.** A company containerized a Windows job that runs on .NET 6 Framework under a Windows container. The company wants to run this job in the AWS Cloud. The job runs every 10 minutes. The job's runtime varies between 1 minute and 3 minutes. Which solution will meet these requirements MOST cost-effectively?

- A. Create an AWS Lambda function based on the container image of the job. Configure Amazon EventBridge to invoke the function every 10 minutes.
- B. Use AWS Batch to create a job that uses AWS Fargate resources. Configure the job scheduling to run every 10 minutes.
- C. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate to run the job. Create a scheduled task based on the container image of the job to run every 10 minutes.
- D. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate to run the job. Create a standalone task based on the container image of the job. Use Windows task scheduler to run the job every 10 minutes.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* ECS on Fargate supports Windows containers and scheduled tasks (EventBridge). Lambda (A) does not support Windows containers. AWS Batch (B) is more complex. Standalone task with Windows scheduler (D) is not managed. (AWS Docs: ECS Scheduled Tasks with Fargate).
</details>

---

**Q484.** A company wants to move from many standalone AWS accounts to a consolidated, multi-account architecture. The company plans to create many new AWS accounts for different business units. The company needs to authenticate access to these AWS accounts by using a centralized corporate directory service. Which combination of actions should a solutions architect recommend to meet these requirements? (Choose two.)

- A. Create a new organization in AWS Organizations with all features turned on. Create the new AWS accounts in the organization.
- B. Set up an Amazon Cognito identity pool. Configure AWS IAM Identity Center (AWS Single Sign-On) to accept Amazon Cognito authentication.
- C. Configure a service control policy (SCP) to manage the AWS accounts. Add AWS IAM Identity Center (AWS Single Sign-On) to AWS Directory Service.
- D. Create a new organization in AWS Organizations. Configure the organization's authentication mechanism to use AWS Directory Service directly.
- E. Set up AWS IAM Identity Center (AWS Single Sign-On) in the organization. Configure IAM Identity Center, and integrate it with the company's corporate directory service.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* Create organization in AWS Organizations (A). Set up IAM Identity Center (SSO) and integrate with corporate directory (E). IAM Identity Center supports integration with Active Directory, SAML, etc. Cognito (B) is for customer identity. SCPs (C) are for permissions, not authentication. (AWS Docs: IAM Identity Center with Organizations).
</details>

---

**Q485.** A company is looking for a solution that can store video archives in AWS from old news footage. The company needs to minimize costs and will rarely need to restore these files. When the files are needed, they must be available in a maximum of five minutes. What is the MOST cost-effective solution?

- A. Store the video archives in Amazon S3 Glacier and use Expedited retrievals.
- B. Store the video archives in Amazon S3 Glacier and use Standard retrievals.
- C. Store the video archives in Amazon S3 Standard-Infrequent Access (S3 Standard-IA).
- D. Store the video archives in Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA).

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Glacier with Expedited retrievals (1-5 minutes) is cost-effective for rarely accessed data that needs retrieval within minutes. Standard retrievals (B) take 3-5 hours. Standard-IA (C) and One Zone-IA (D) are more expensive for long-term archival. (AWS Docs: Glacier Retrieval Options).
</details>

---

**Q486.** A company is building a three-tier application on AWS. The presentation tier will serve a static website. The logic tier is a containerized application. This application will store data in a relational database. The company wants to simplify deployment and to reduce operational costs. Which solution will meet these requirements?

- A. Use Amazon S3 to host static content. Use Amazon Elastic Container Service (Amazon ECS) with AWS Fargate for compute power. Use a managed Amazon RDS cluster for the database.
- B. Use Amazon CloudFront to host static content. Use Amazon Elastic Container Service (Amazon ECS) with Amazon EC2 for compute power. Use a managed Amazon RDS cluster for the database.
- C. Use Amazon S3 to host static content. Use Amazon Elastic Kubernetes Service (Amazon EKS) with AWS Fargate for compute power. Use a managed Amazon RDS cluster for the database.
- D. Use Amazon EC2 Reserved Instances to host static content. Use Amazon Elastic Kubernetes Service (Amazon EKS) with Amazon EC2 for compute power. Use a managed Amazon RDS cluster for the database.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 for static website (cost-effective). ECS with Fargate (serverless containers) for logic tier. RDS for relational database. This minimizes operational overhead. EKS (C, D) is more complex. EC2 for static (D) is overkill. (AWS Docs: Serverless Three-Tier).
</details>

---

**Q487.** A company seeks a storage solution for its application. The solution must be highly available and scalable. The solution also must function as a file system, be mountable by multiple Linux instances in AWS and on premises through native protocols, and have no minimum size requirements. The company has set up a Site-to-Site VPN for access from its on-premises network to its VPC. Which storage solution meets these requirements?

- A. Amazon FSx Multi-AZ deployments
- B. Amazon Elastic Block Store (Amazon EBS) Multi-Attach volumes
- C. Amazon Elastic File System (Amazon EFS) with multiple mount targets
- D. Amazon Elastic File System (Amazon EFS) with a single mount target and multiple access points

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* EFS is a scalable, highly available file system (NFS) that can be mounted by multiple Linux instances both in AWS (via mount targets) and on-premises (via VPN). No minimum size. FSx (A) is Windows-focused. EBS Multi-Attach (B) has limitations and is block storage. (AWS Docs: EFS On-Premises Access).
</details>

---

**Q488.** A 4-year-old media company is using the AWS Organizations all features feature set to organize its AWS accounts. According to the company's finance team, the billing information on the member accounts must not be accessible to anyone, including the root user of the member accounts. Which solution will meet these requirements?

- A. Add all finance team users to an IAM group. Attach an AWS managed policy named Billing to the group.
- B. Attach an identity-based policy to deny access to the billing information to all users, including the root user.
- C. Create a service control policy (SCP) to deny access to the billing information. Attach the SCP to the root organizational unit (OU).
- D. Convert from the Organizations all features feature set to the Organizations consolidated billing feature set.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* SCPs can deny access to billing actions for all accounts in the organization, including root users of member accounts. Identity-based policies (B) cannot restrict the root user of the member account. Option A (Billing policy) grants access, not denies. Option D (consolidated billing only) still allows member root access. (AWS Docs: SCP for Billing).
</details>

---

**Q489.** An ecommerce company runs an application in the AWS Cloud that is integrated with an on-premises warehouse solution. The company uses Amazon Simple Notification Service (Amazon SNS) to send order messages to an on-premises HTTPS endpoint so the warehouse application can process the orders. The local data center team has detected that some of the order messages were not received. A solutions architect needs to retain messages that are not delivered and analyze the messages for up to 14 days. Which solution will meet these requirements with the LEAST development effort?

- A. Configure an Amazon SNS dead letter queue that has an Amazon Kinesis Data Stream target with a retention period of 14 days.
- B. Add an Amazon Simple Queue Service (Amazon SQS) queue with a retention period of 14 days between the application and Amazon SNS.
- C. Configure an Amazon SNS dead letter queue that has an Amazon Simple Queue Service (Amazon SQS) target with a retention period of 14 days.
- D. Configure an Amazon SNS dead letter queue that has an Amazon DynamoDB target with a TTL attribute set for a retention period of 14 days.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* SNS supports dead-letter queues (DLQ) using SQS. Messages that fail delivery go to the SQS queue, which can retain them for up to 14 days for analysis. This requires minimal development. Option A (Kinesis) is more complex. Option B adds queue before SNS (changes application). Option D (DynamoDB) requires custom Lambda. (AWS Docs: SNS DLQ).
</details>

---

**Q490.** A gaming company uses Amazon DynamoDB to store user information such as geographic location, player data, and leaderboards. The company needs to configure continuous backups to an Amazon S3 bucket with a minimal amount of coding. The backups must not affect availability of the application and must not affect the read capacity units (RCUs) that are defined for the table. Which solution meets these requirements?

- A. Use an Amazon EMR cluster. Create an Apache Hive job to back up the data to Amazon S3.
- B. Export the data directly from DynamoDB to Amazon S3 with continuous backups. Turn on point-in-time recovery for the table.
- C. Configure Amazon DynamoDB Streams. Create an AWS Lambda function to consume the stream and export the data to an Amazon S3 bucket.
- D. Create an AWS Lambda function to export the data from the database tables to Amazon S3 on a regular basis. Turn on point-in-time recovery for the table.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* DynamoDB native export to S3 (using PITR or on-demand) does not consume RCUs and is fully managed, requiring minimal coding. Option A (EMR) requires coding. Option C (Streams + Lambda) consumes read capacity and requires coding. Option D (Lambda) requires coding. (AWS Docs: DynamoDB Export to S3).
</details>

---

**Q491.** A solutions architect is designing an asynchronous application to process credit card data validation requests for a bank. The application must be secure and be able to process each request at least once. Which solution will meet these requirements MOST cost-effectively?

- A. Use AWS Lambda event source mapping. Set Amazon Simple Queue Service (Amazon SQS) standard queues as the event source. Use AWS Key Management Service (SSE-KMS) for encryption. Add the kms:Decrypt permission for the Lambda execution role.
- B. Use AWS Lambda event source mapping. Use Amazon Simple Queue Service (Amazon SQS) FIFO queues as the event source. Use SQS managed encryption keys (SSE-SQS) for encryption. Add the encryption key invocation permission for the Lambda function.
- C. Use the AWS Lambda event source mapping. Set Amazon Simple Queue Service (Amazon SQS) FIFO queues as the event source. Use AWS KMS keys (SSE-KMS). Add the kms:Decrypt permission for the Lambda execution role.
- D. Use the AWS Lambda event source mapping. Set Amazon Simple Queue Service (Amazon SQS) standard queues as the event source. Use AWS KMS keys (SSE-KMS) for encryption. Add the encryption key invocation permission for the Lambda function.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Standard SQS queues provide at-least-once delivery and are cheaper than FIFO. SSE-KMS with customer managed key is secure. Standard queues are fine for credit card validation (ordering not required). FIFO (B, C) is more expensive and unnecessary. (AWS Docs: SQS Standard vs FIFO Cost).
</details>

---

**Q492.** A company has multiple AWS accounts for development work. Some staff consistently use oversized Amazon EC2 instances, which causes the company to exceed the yearly budget for the development accounts. The company wants to centrally restrict the creation of AWS resources in these accounts. Which solution will meet these requirements with the LEAST development effort?

- A. Develop AWS Systems Manager templates that use an approved EC2 creation process. Use the approved Systems Manager templates to provision EC2 instances.
- B. Use AWS Organizations to organize the accounts into organizational units (OUs). Define and attach a service control policy (SCP) to control the usage of EC2 instance types.
- C. Configure an Amazon EventBridge rule that invokes an AWS Lambda function when an EC2 instance is created. Stop disallowed EC2 instance types.
- D. Set up AWS Service Catalog products for the staff to create the allowed EC2 instance types. Ensure that staff can deploy EC2 instances only by using the Service Catalog products.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* SCPs can deny creation of EC2 instances with specific instance types (oversized) across all accounts in an OU, with minimal effort (no custom code). Option A (Systems Manager) requires template development. Option C (Lambda) requires coding. Option D (Service Catalog) requires product creation. (AWS Docs: SCPs for Instance Type Restriction).
</details>

---

**Q493.** A company wants to use artificial intelligence (AI) to determine the quality of its customer service calls. The company currently manages calls in four different languages, including English. The company will offer new languages in the future. The company does not have the resources to regularly maintain machine learning (ML) models. The company needs to create written sentiment analysis reports from the customer service call recordings. The customer service call recording text must be translated into English. Which combination of steps will meet these requirements? (Choose three.)

- A. Use Amazon Comprehend to translate the audio recordings into English.
- B. Use Amazon Lex to create the written sentiment analysis reports.
- C. Use Amazon Polly to convert the audio recordings into text.
- D. Use Amazon Transcribe to convert the audio recordings in any language into text.
- E. Use Amazon Translate to translate text in any language to English.
- F. Use Amazon Comprehend to create the sentiment analysis reports.

<details>
<summary>Show Answer</summary>

**Answer: D, E, F**

*Explanation:* Transcribe (D) converts audio to text. Translate (E) translates text to English. Comprehend (F) performs sentiment analysis on text. Comprehend (A) does not translate audio. Lex (B) is for chatbots. Polly (C) is text-to-speech. (AWS Docs: Transcribe, Translate, Comprehend).
</details>

---

**Q494.** A company uses Amazon EC2 instances to host its internal systems. As part of a deployment operation, an administrator tries to use the AWS CLI to terminate an EC2 instance. However, the administrator receives a 403 (Access Denied) error message. The administrator is using an IAM role that has the following IAM policy attached: `{ "Version":"2012-10-17", "Statement":[ { "Effect":"Allow", "Action":["ec2:TerminateInstances"], "Resource":["*"] }, { "Effect":"Deny", "Action":["ec2:TerminateInstances"], "Condition":{ "NotIpAddress":{ "aws:SourceIp":["192.0.2.0/24", "203.0.113.0/24"] } }, "Resource":["*"] } ] }`. What is the cause of the unsuccessful request?

- A. The EC2 instance has a resource-based policy with a Deny statement.
- B. The principal has not been specified in the policy statement.
- C. The "Action" field does not grant the actions that are required to terminate the EC2 instance.
- D. The request to terminate the EC2 instance does not originate from the CIDR blocks 192.0.2.0/24 or 203.0.113.0/24.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* The policy allows termination, but denies termination if the source IP is NOT in the specified CIDR blocks. Since the administrator received an Access Denied, their IP is not in the allowed list. Option D is correct. (AWS Docs: IAM Condition Keys).
</details>

---

**Q495.** A company is conducting an internal audit. The company wants to ensure that the data in an Amazon S3 bucket that is associated with the company's AWS Lake Formation data lake does not contain sensitive customer or employee data. The company wants to discover personally identifiable information (PII) or financial information, including passport numbers and credit card numbers. Which solution will meet these requirements?

- A. Configure AWS Audit Manager on the account. Select the Payment Card Industry Data Security Standards (PCI DSS) for auditing.
- B. Configure Amazon S3 Inventory on the S3 bucket. Configure Amazon Athena to query the inventory.
- C. Configure Amazon Macie to run a data discovery job that uses managed identifiers for the required data types.
- D. Use Amazon S3 Select to run a report across the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Amazon Macie is designed to discover and classify sensitive data (PII, financial) in S3 using managed identifiers. Audit Manager (A) is for compliance frameworks. S3 Inventory (B) is for inventory, not data content. S3 Select (D) is for querying, not discovery. (AWS Docs: Amazon Macie).
</details>

---

**Q496.** A company uses on-premises servers to host its applications. The company is running out of storage capacity. The applications use both block storage and NFS storage. The company needs a high-performing solution that supports local caching without re-architecting its existing applications. Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

- A. Mount Amazon S3 as a file system to the on-premises servers.
- B. Deploy an AWS Storage Gateway file gateway to replace NFS storage.
- C. Deploy AWS Snowball Edge to provision NFS mounts to on-premises servers.
- D. Deploy an AWS Storage Gateway volume gateway to replace the block storage.
- E. Deploy Amazon Elastic File System (Amazon EFS) volumes and mount them to on-premises servers.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* File Gateway (B) provides NFS/SMB with local caching for file storage. Volume Gateway (D) provides iSCSI block storage with local caching. S3 (A) is not a file system. Snowball (C) is for data transfer. EFS (E) is not designed for on-premises with local caching (requires Direct Connect and no local cache). (AWS Docs: Storage Gateway for On-Premises).
</details>

---

**Q497.** A company has a service that reads and writes large amounts of data from an Amazon S3 bucket in the same AWS Region. The service is deployed on Amazon EC2 instances within the private subnet of a VPC. The service communicates with Amazon S3 over a NAT gateway in the public subnet. However, the company wants a solution that will reduce the data output costs. Which solution will meet these requirements MOST cost-effectively?

- A. Provision a dedicated EC2 NAT instance in the public subnet. Configure the route table for the private subnet to use the elastic network interface of this instance as the destination for all S3 traffic.
- B. Provision a dedicated EC2 NAT instance in the private subnet. Configure the route table for the public subnet to use the elastic network interface of this instance as the destination for all S3 traffic.
- C. Provision a VPC gateway endpoint. Configure the route table for the private subnet to use the gateway endpoint as the route for all S3 traffic.
- D. Provision a second NAT gateway. Configure the route table for the private subnet to use this NAT gateway as the destination for all S3 traffic.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* A VPC gateway endpoint for S3 keeps traffic within AWS network, eliminating NAT gateway data processing costs. This is the most cost-effective. Additional NAT instances/gateways (A, B, D) do not reduce costs. (AWS Docs: S3 VPC Endpoint Cost Savings).
</details>

---

**Q498.** A company uses Amazon S3 to store high-resolution pictures in an S3 bucket. To minimize application changes, the company stores the pictures as the latest version of an S3 object. The company needs to retain only the two most recent versions of the pictures. The company wants to reduce costs. The company has identified the S3 bucket as a large expense. Which solution will reduce the S3 costs with the LEAST operational overhead?

- A. Use S3 Lifecycle to delete expired object versions and retain the two most recent versions.
- B. Use an AWS Lambda function to check for older versions and delete all but the two most recent versions.
- C. Use S3 Batch Operations to delete noncurrent object versions and retain only the two most recent versions.
- D. Deactivate versioning on the S3 bucket and retain the two most recent versions.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Lifecycle policies can be configured to expire noncurrent versions after a certain number of versions (e.g., keep 2 most recent). This is the least operational overhead. Lambda (B) and Batch (C) require more effort. Deactivating versioning (D) would lose versioning capability. (AWS Docs: S3 Lifecycle Noncurrent Version Expiration).
</details>

---

**Q499.** A company needs to minimize the cost of its 1 Gbps AWS Direct Connect connection. The company's average connection utilization is less than 10%. A solutions architect must recommend a solution that will reduce the cost without compromising security. Which solution will meet these requirements?

- A. Set up a new 1 Gbps Direct Connect connection. Share the connection with another AWS account.
- B. Set up a new 200 Mbps Direct Connect connection in the AWS Management Console.
- C. Contact an AWS Direct Connect Partner to order a 1 Gbps connection. Share the connection with another AWS account.
- D. Contact an AWS Direct Connect Partner to order a 200 Mbps hosted connection for an existing AWS account.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Reducing bandwidth from 1 Gbps to 200 Mbps (adequate for 10% utilization) reduces cost. A hosted connection from a Direct Connect Partner allows you to provision lower bandwidth (e.g., 200 Mbps). Option B (setting up in console) is not possible; you need a partner for hosted connections. (AWS Docs: Direct Connect Hosted Connections).
</details>

---

**Q500.** A company has multiple Windows file servers on premises. The company wants to migrate and consolidate its files into an Amazon FSx for Windows File Server file system. File permissions must be preserved to ensure that access rights do not change. Which solutions will meet these requirements? (Choose two.)

- A. Deploy AWS DataSync agents on premises. Schedule DataSync tasks to transfer the data to the FSx for Windows File Server file system.
- B. Copy the shares on each file server into Amazon S3 buckets by using the AWS CLI. Schedule AWS DataSync tasks to transfer the data to the FSx for Windows File Server file system.
- C. Remove the drives from each file server. Ship the drives to AWS for import into Amazon S3. Schedule AWS DataSync tasks to transfer the data to the FSx for Windows File Server file system.
- D. Order an AWS Snowcone device. Connect the device to the on-premises network. Launch AWS DataSync agents on the device. Schedule DataSync tasks to transfer the data to the FSx for Windows File Server file system.
- E. Order an AWS Snowball Edge Storage Optimized device. Connect the device to the on-premises network. Copy data to the device by using the AWS CLI. Ship the device back to AWS for import into Amazon S3. Schedule AWS DataSync tasks to transfer the data to the FSx for Windows File Server file system.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* DataSync agents on-premises (A) can transfer directly to FSx, preserving permissions. For large data, Snowball Edge (E) can be used for initial seed, then DataSync for ongoing changes. Option B (CLI to S3) may not preserve permissions. Option C (ship drives) is not recommended. Option D (Snowcone) has limited capacity. (AWS Docs: DataSync to FSx, Snowball with DataSync).
</details>

---

**Q501.** A company wants to ingest customer payment data into the company's data lake in Amazon S3. The company receives payment data every minute on average. The company wants to analyze the payment data in real time. Then the company wants to ingest the data into the data lake. Which solution will meet these requirements with the MOST operational efficiency?

- A. Use Amazon Kinesis Data Streams to ingest data. Use AWS Lambda to analyze the data in real time.
- B. Use AWS Glue to ingest data. Use Amazon Kinesis Data Analytics to analyze the data in real time.
- C. Use Amazon Kinesis Data Firehose to ingest data. Use Amazon Kinesis Data Analytics to analyze the data in real time.
- D. Use Amazon API Gateway to ingest data. Use AWS Lambda to analyze the data in real time.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Kinesis Data Streams is ideal for real-time ingestion. Lambda can analyze data in real time (stream processing). Firehose (C) is for delivery, not real-time analysis. Glue (B) is for ETL, not real-time streaming. API Gateway (D) is synchronous, not streaming. (AWS Docs: Kinesis Streams + Lambda).
</details>

---

**Q502.** A company runs a website that uses a content management system (CMS) on Amazon EC2. The CMS runs on a single EC2 instance and uses an Amazon Aurora MySQL Multi-AZ DB instance for the data tier. Website images are stored on an Amazon Elastic Block Store (Amazon EBS) volume that is mounted inside the EC2 instance. Which combination of actions should a solutions architect take to improve the performance and resilience of the website? (Choose two.)

- A. Move the website images into an Amazon S3 bucket that is mounted on every EC2 instance.
- B. Share the website images by using an NFS share from the primary EC2 instance. Mount this share on the other EC2 instances.
- C. Move the website images onto an Amazon Elastic File System (Amazon EFS) file system that is mounted on every EC2 instance.
- D. Create an Amazon Machine Image (AMI) from the existing EC2 instance. Use the AMI to provision new instances behind an Application Load Balancer as part of an Auto Scaling group. Configure the Auto Scaling group to maintain a minimum of two instances. Configure an accelerator in AWS Global Accelerator for the website.
- E. Create an Amazon Machine Image (AMI) from the existing EC2 instance. Use the AMI to provision new instances behind an Application Load Balancer as part of an Auto Scaling group. Configure the Auto Scaling group to maintain a minimum of two instances. Configure an Amazon CloudFront distribution for the website.

<details>
<summary>Show Answer</summary>

**Answer: C, E**

*Explanation:* Move images to EFS (C) for shared, scalable storage. Use AMI, ALB, Auto Scaling (multi-AZ) for HA (E). CloudFront (E) improves performance for static content. Option A (S3 mounted) is not standard (S3 is not a file system). Option B (NFS from primary) creates a single point of failure. Global Accelerator (D) is not needed for a website (CloudFront is better for caching). (AWS Docs: EFS for Shared Images, CloudFront for Static).
</details>

---

**Q503.** A company runs an infrastructure monitoring service. The company is building a new feature that will enable the service to monitor data in customer AWS accounts. The new feature will call AWS APIs in customer accounts to describe Amazon EC2 instances and read Amazon CloudWatch metrics. What should the company do to obtain access to customer accounts in the MOST secure way?

- A. Ensure that the customers create an IAM role in their account with read-only EC2 and CloudWatch permissions and a trust policy to the company's account.
- B. Create a serverless API that implements a token vending machine to provide temporary AWS credentials for a role with read-only EC2 and CloudWatch permissions.
- C. Ensure that the customers create an IAM user in their account with read-only EC2 and CloudWatch permissions. Encrypt and store customer access and secret keys in a secrets management system.
- D. Ensure that the customers create an Amazon Cognito user in their account to use an IAM role with read-only EC2 and CloudWatch permissions. Encrypt and store the Amazon Cognito user and password in a secrets management system.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Cross-account IAM roles are the most secure way to grant access. Customers create a role with the required permissions and a trust policy allowing the company's account to assume it. No long-term credentials are stored. Option B (token vending machine) is more complex. Options C and D use IAM users (long-term credentials) which are less secure. (AWS Docs: Cross-Account Roles).
</details>

---

**Q504.** A company needs to connect several VPCs in the us-east-1 Region that span hundreds of AWS accounts. The company's networking team has its own AWS account to manage the cloud network. What is the MOST operationally efficient solution to connect the VPCs?

- A. Set up VPC peering connections between each VPC. Update each associated subnet's route table.
- B. Configure a NAT gateway and an internet gateway in each VPC to connect each VPC through the internet.
- C. Create an AWS Transit Gateway in the networking team's AWS account. Configure static routes from each VPC.
- D. Deploy VPN gateways in each VPC. Create a transit VPC in the networking team's AWS account to connect to each VPC.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Transit Gateway centralizes connectivity and scales to hundreds of VPCs across accounts (via AWS RAM). This is more operationally efficient than many VPC peering connections (A). Option B (internet) is insecure. Option D (transit VPC) is an older, more complex pattern. (AWS Docs: Transit Gateway).
</details>

---

**Q505.** A company has Amazon EC2 instances that run nightly batch jobs to process data. The EC2 instances run in an Auto Scaling group that uses On-Demand billing. If a job fails on one instance, another instance will reprocess the job. The batch jobs run between 12:00 AM and 06:00 AM local time every day. Which solution will provide EC2 instances to meet these requirements MOST cost-effectively?

- A. Purchase a 1-year Savings Plan for Amazon EC2 that covers the instance family of the Auto Scaling group that the batch job uses.
- B. Purchase a 1-year Reserved Instance for the specific instance type and operating system of the instances in the Auto Scaling group that the batch job uses.
- C. Create a new launch template for the Auto Scaling group. Set the instances to Spot Instances. Set a policy to scale out based on CPU usage.
- D. Create a new launch template for the Auto Scaling group. Increase the instance size. Set a policy to scale out based on CPU usage.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Batch jobs are fault-tolerant (can reprocess) and run only 6 hours a day. Spot Instances offer significant cost savings (up to 90%) and are ideal for this workload. Savings Plan/Reserved (A, B) are for 24/7 workloads. Increasing instance size (D) doesn't reduce cost. (AWS Docs: Spot for Batch).
</details>

---

**Q506.** A social media company is building a feature for its website. The feature will give users the ability to upload photos. The company expects significant increases in demand during large events and must ensure that the website can handle the upload traffic from users. Which solution meets these requirements with the MOST scalability?

- A. Upload files from the user's browser to the application servers. Transfer the files to an Amazon S3 bucket.
- B. Provision an AWS Storage Gateway file gateway. Upload files directly from the user's browser to the file gateway.
- C. Generate Amazon S3 presigned URLs in the application. Upload files directly from the user's browser into an S3 bucket.
- D. Provision an Amazon Elastic File System (Amazon EFS) file system. Upload files directly from the user's browser to the file system.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Presigned URLs allow direct client-to-S3 uploads, offloading the application servers and providing massive scalability (S3 handles the load). Option A uses app servers as bottleneck. Option B (File Gateway) is not for massive web uploads. Option D (EFS) is not designed for direct browser uploads. (AWS Docs: S3 Presigned URLs for Uploads).
</details>

---

**Q507.** A company has a web application for travel ticketing. The application is based on a database that runs in a single data center in North America. The company wants to expand the application to serve a global user base. The company needs to deploy the application to multiple AWS Regions. Average latency must be less than 1 second on updates to the reservation database. The company wants to have separate deployments of its web platform across multiple Regions. However, the company must maintain a single primary reservation database that is globally consistent. Which solution should a solutions architect recommend to meet these requirements?

- A. Convert the application to use Amazon DynamoDB. Use a global table for the center reservation table. Use the correct Regional endpoint in each Regional deployment.
- B. Migrate the database to an Amazon Aurora MySQL database. Deploy Aurora Read Replicas in each Region. Use the correct Regional endpoint in each Regional deployment for access to the database.
- C. Migrate the database to an Amazon RDS for MySQL database. Deploy MySQL read replicas in each Region. Use the correct Regional endpoint in each Regional deployment for access to the database.
- D. Migrate the application to an Amazon Aurora Serverless database. Deploy instances of the database to each Region. Use the correct Regional endpoint in each Regional deployment to access the database. Use AWS Lambda functions to process event streams in each Region to synchronize the databases.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* DynamoDB global tables provide multi-Region, fully replicated, globally consistent tables with low latency. Aurora Read Replicas (B, C) are asynchronous and may have higher latency. Aurora Serverless with custom sync (D) is complex. (AWS Docs: DynamoDB Global Tables).
</details>

---

**Q508.** A company has migrated multiple Microsoft Windows Server workloads to Amazon EC2 instances that run in the us-west-1 Region. The company manually backs up the workloads to create an image as needed. In the event of a natural disaster in the us-west-1 Region, the company wants to recover workloads quickly in the us-west-2 Region. The company wants no more than 24 hours of data loss on the EC2 instances. The company also wants to automate any backups of the EC2 instances. Which solutions will meet these requirements with the LEAST administrative effort? (Choose two.)

- A. Create an Amazon EC2-backed Amazon Machine Image (AMI) lifecycle policy to create a backup based on tags. Schedule the backup to run twice daily. Copy the image on demand.
- B. Create an Amazon EC2-backed Amazon Machine Image (AMI) lifecycle policy to create a backup based on tags. Schedule the backup to run twice daily. Configure the copy to the us-west-2 Region.
- C. Create backup vaults in us-west-1 and in us-west-2 by using AWS Backup. Create a backup plan for the EC2 instances based on tag values. Create an AWS Lambda function to run as a scheduled job to copy the backup data to us-west-2.
- D. Create a backup vault by using AWS Backup. Use AWS Backup to create a backup plan for the EC2 instances based on tag values. Define the destination for the copy as us-west-2. Specify the backup schedule to run twice daily.
- E. Create a backup vault by using AWS Backup. Use AWS Backup to create a backup plan for the EC2 instances based on tag values. Specify the backup schedule to run twice daily. Copy on demand to us-west-2.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* AWS Backup (D) or AMI lifecycle policies (B) can automate backups and cross-Region copying with minimal effort. Option B (AMI lifecycle policy) and D (AWS Backup) both support scheduled cross-Region copy. Option A (copy on demand) requires manual intervention. Option C (Lambda) adds overhead. Option E (copy on demand) not automated. (AWS Docs: AMI Lifecycle Policy, AWS Backup Cross-Region).
</details>

---

**Q509.** A company operates a two-tier application for image processing. The application uses two Availability Zones, each with one public subnet and one private subnet. An Application Load Balancer (ALB) for the web tier uses the public subnets, Amazon EC2 instances for the application tier use the private subnets. Users report that the application is running more slowly than expected. A security audit of the web server log files shows that the application is receiving millions of illegitimate requests from a small number of IP addresses. A solutions architect needs to resolve the immediate performance problem while the company investigates a more permanent solution. What should the solutions architect recommend to meet this requirement?

- A. Modify the inbound security group for the web tier. Add a deny rule for the IP addresses that are consuming resources.
- B. Modify the network ACL for the web tier subnets. Add an inbound deny rule for the IP addresses that are consuming resources.
- C. Modify the inbound security group for the application tier. Add a deny rule for the IP addresses that are consuming resources.
- D. Modify the network ACL for the application tier subnets. Add an inbound deny rule for the IP addresses that are consuming resources.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Security groups (A, C) are stateful and do not support deny rules (they are allow-only). Network ACLs (B, D) are stateless and support deny rules. The traffic is hitting the web tier, so the NACL on the web tier subnets should block the malicious IPs at the subnet boundary. (AWS Docs: NACL vs Security Group).
</details>

---

**Q510.** A global marketing company has applications that run in the ap-southeast-2 Region and the eu-west-1 Region. Applications that run in a VPC in eu-west-1 need to communicate securely with databases that run in a VPC in ap-southeast-2. Which network design will meet these requirements?

- A. Create a VPC peering connection between the eu-west-1 VPC and the ap-southeast-2 VPC. Create an inbound rule in the eu-west-1 application security group that allows traffic from the database server IP addresses in the ap-southeast-2 security group.
- B. Configure a VPC peering connection between the ap-southeast-2 VPC and the eu-west-1 VPC. Update the subnet route tables. Create an inbound rule in the ap-southeast-2 database security group that references the security group ID of the application servers in eu-west-1.
- C. Configure a VPC peering connection between the ap-southeast-2 VPC and the eu-west-1 VPC. Update the subnet route tables. Create an inbound rule in the ap-southeast-2 database security group that allows traffic from the eu-west-1 application server IP addresses.
- D. Create a transit gateway with a peering attachment between the eu-west-1 VPC and the ap-southeast-2 VPC. After the transit gateways are properly peered and routing is configured, create an inbound rule in the database security group that references the security group ID of the application servers in eu-west-1.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Cross-Region VPC peering does not support referencing security group IDs (only CIDR blocks). Transit Gateway peering is the correct way to connect across Regions, and it does allow security group referencing (via Transit Gateway). Option B/C use VPC peering which doesn't support SG referencing across Regions. (AWS Docs: Transit Gateway Peering with Security Groups).
</details>

---

**Q511.** A company is developing software that uses a PostgreSQL database schema. The company needs to configure multiple development environments and databases for the company's developers. On average, each development environment is used for half of the 8-hour workday. Which solution will meet these requirements MOST cost-effectively?

- A. Configure each development environment with its own Amazon Aurora PostgreSQL database.
- B. Configure each development environment with its own Amazon RDS for PostgreSQL Single-AZ DB instances.
- C. Configure each development environment with its own Amazon Aurora On-Demand PostgreSQL-Compatible database.
- D. Configure each development environment with its own Amazon S3 bucket by using Amazon S3 Object Select.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* RDS Single-AZ is cost-effective for dev environments that can be stopped when not in use. Aurora (A, C) is more expensive. S3 (D) is not a PostgreSQL database. (AWS Docs: RDS for Dev/Test).
</details>

---

**Q512.** A company uses AWS Organizations with resources tagged by account. The company also uses AWS Backup to back up its AWS infrastructure resources. The company needs to back up all AWS resources. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Config to identify all untagged resources. Tag the identified resources programmatically. Use tags in the backup plan.
- B. Use AWS Config to identify all resources that are not running. Add those resources to the backup vault.
- C. Require all AWS account owners to review their resources to identify the resources that need to be backed up.
- D. Use Amazon Inspector to identify all noncompliant resources.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Backup can use tags to select resources. Use AWS Config to find untagged resources and tag them programmatically, then use tag-based backup plans. Option B (not running) is irrelevant. Option C (manual) is overhead. Option D (Inspector) is for vulnerability scanning. (AWS Docs: AWS Backup Tag-Based Policies).
</details>

---

**Q513.** A social media company wants to allow its users to upload images in an application that is hosted in the AWS Cloud. The company needs a solution that automatically resizes the images so that the images can be displayed on multiple device types. The application experiences unpredictable traffic patterns throughout the day. The company is seeking a highly available solution that maximizes scalability. What should a solutions architect do to meet these requirements?

- A. Create a static website hosted in Amazon S3 that invokes AWS Lambda functions to resize the images and store the images in an Amazon S3 bucket.
- B. Create a static website hosted in Amazon CloudFront that invokes AWS Step Functions to resize the images and store the images in an Amazon RDS database.
- C. Create a dynamic website hosted on a web server that runs on an Amazon EC2 instance. Configure a process that runs on the EC2 instance to resize the images and store the images in an Amazon S3 bucket.
- D. Create a dynamic website hosted on an automatically scaling Amazon Elastic Container Service (Amazon ECS) cluster that creates a resize job in Amazon Simple Queue Service (Amazon SQS). Set up an image-resizing program that runs on an Amazon EC2 instance to process the resize jobs.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Serverless architecture: S3 static hosting, Lambda for resizing, S3 for storage. This scales automatically with unpredictable traffic and has high availability. Step Functions (B) is overkill. EC2 (C) or ECS (D) have more overhead. (AWS Docs: Serverless Image Resizing).
</details>

---

**Q514.** A company is running a microservices application on Amazon EC2 instances. The company wants to migrate the application to an Amazon Elastic Kubernetes Service (Amazon EKS) cluster for scalability. The company must configure the Amazon EKS control plane with endpoint private access set to true and endpoint public access set to false to maintain security compliance. The company must also put the data plane in private subnets. However, the company has received error notifications because the node cannot join the cluster. Which solution will allow the node to join the cluster?

- A. Grant the required permission in AWS Identity and Access Management (IAM) to the AmazonEKSNodeRole IAM role.
- B. Create interface VPC endpoints to allow nodes to access the control plane.
- C. Recreate nodes in the public subnet. Restrict security groups for EC2 nodes.
- D. Allow outbound traffic in the security group of the nodes.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* With private control plane endpoint only (public access false), nodes must communicate with the control plane via private network. You need to create interface VPC endpoints for EKS (and other services) to allow nodes in private subnets to reach the control plane. Option A (IAM) is not the issue. Option C (public subnet) violates security. Option D (outbound traffic) is already allowed. (AWS Docs: EKS Private Endpoint).
</details>

---

**Q515.** A company is migrating an on-premises application to AWS. The company wants to use Amazon Redshift as a solution. Which use cases are suitable for Amazon Redshift in this scenario? (Choose three.)

- A. Supporting data APIs to access data with traditional, containerized, and event-driven applications
- B. Supporting client-side and server-side encryption
- C. Building analytics workloads during specified hours and when the application is not active
- D. Caching data to reduce the pressure on the backend database
- E. Scaling globally to support petabytes of data and tens of millions of requests per minute
- F. Creating a secondary replica of the cluster by using the AWS Management Console

<details>
<summary>Show Answer</summary>

**Answer: B, C, F**

*Explanation:* Redshift supports encryption (B), can be paused/resumed for analytics workloads (C), and supports creating replicas via console (F). Data APIs (A) are not a primary use case. Caching (D) is for ElastiCache. Global scaling (E) is for DynamoDB or Aurora. (AWS Docs: Redshift Features).
</details>

---

**Q516.** A company provides an API interface to customers so the customers can retrieve their financial information. The company expects a larger number of requests during peak usage times of the year. The company requires the API to respond consistently with low latency to ensure customer satisfaction. The company needs to provide a compute host for the API. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use an Application Load Balancer and Amazon Elastic Container Service (Amazon ECS).
- B. Use Amazon API Gateway and AWS Lambda functions with provisioned concurrency.
- C. Use an Application Load Balancer and an Amazon Elastic Kubernetes Service (Amazon EKS) cluster.
- D. Use Amazon API Gateway and AWS Lambda functions with reserved concurrency.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* API Gateway + Lambda with provisioned concurrency keeps functions initialized, reducing cold start latency and providing consistent low latency during peak times. This is serverless with low overhead. ECS (A) or EKS (C) require infrastructure management. Reserved concurrency (D) limits concurrency but does not prevent cold starts. (AWS Docs: Lambda Provisioned Concurrency).
</details>

---

**Q517.** A company wants to send all AWS Systems Manager Session Manager logs to an Amazon S3 bucket for archival purposes. Which solution will meet this requirement with the MOST operational efficiency?

- A. Enable S3 logging in the Systems Manager console. Choose an S3 bucket to send the session data to.
- B. Install the Amazon CloudWatch agent. Push all logs to a CloudWatch log group. Export the logs to an S3 bucket from the group for archival purposes.
- C. Create a Systems Manager document to upload all server logs to a central S3 bucket. Use Amazon EventBridge to run the Systems Manager document against all servers that are in the account daily.
- D. Install an Amazon CloudWatch agent. Push all logs to a CloudWatch log group. Create a CloudWatch logs subscription that pushes any incoming log events to an Amazon Kinesis Data Firehose delivery stream. Set Amazon S3 as the destination.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Systems Manager Session Manager has native S3 logging capability. You can enable it in the console to directly send session logs to S3, which is the most operationally efficient. Options B, C, D require additional agents and complexity. (AWS Docs: Session Manager S3 Logging).
</details>

---

**Q518.** An application uses an Amazon RDS MySQL DB instance. The RDS database is becoming low on disk space. A solutions architect wants to increase the disk space without downtime. Which solution meets these requirements with the LEAST amount of effort?

- A. Enable storage autoscaling in RDS.
- B. Increase the RDS database instance size.
- C. Change the RDS database instance storage type to Provisioned IOPS.
- D. Back up the RDS database, increase the storage capacity, restore the database, and stop the previous instance.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* RDS storage autoscaling automatically increases storage when free space is low, with minimal downtime. This is the least effort. Increasing instance size (B) doesn't increase storage. Changing storage type (C) may cause downtime. Backup/restore (D) has significant downtime. (AWS Docs: RDS Storage Autoscaling).
</details>

---

**Q519.** A consulting company provides professional services to customers worldwide. The company provides solutions and tools for customers to expedite gathering and analyzing data on AWS. The company needs to centrally manage and deploy a common set of solutions and tools for customers to use for self-service purposes. Which solution will meet these requirements?

- A. Create AWS CloudFormation templates for the customers.
- B. Create AWS Service Catalog products for the customers.
- C. Create AWS Systems Manager templates for the customers.
- D. Create AWS Config items for the customers.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Service Catalog allows you to create and manage a catalog of IT services (CloudFormation templates) that are approved for use. Customers can self-deploy these products. This is the best fit for centrally managed, self-service deployment. CloudFormation templates (A) alone don't provide self-service catalog. Systems Manager (C) is for management. Config (D) is for compliance. (AWS Docs: AWS Service Catalog).
</details>

---

**Q520.** A company is designing a new web application that will run on Amazon EC2 instances. The application will use Amazon DynamoDB for backend data storage. The application traffic will be unpredictable. The company expects that the application read and write throughput to the database will be moderate to high. The company needs to scale in response to application traffic. Which DynamoDB table configuration will meet these requirements MOST cost-effectively?

- A. Configure DynamoDB with provisioned read and write by using the DynamoDB Standard table class. Set DynamoDB auto scaling to a maximum defined capacity.
- B. Configure DynamoDB in on-demand mode by using the DynamoDB Standard table class.
- C. Configure DynamoDB with provisioned read and write by using the DynamoDB Standard Infrequent Access (DynamoDB Standard-IA) table class. Set DynamoDB auto scaling to a maximum defined capacity.
- D. Configure DynamoDB in on-demand mode by using the DynamoDB Standard Infrequent Access (DynamoDB Standard-IA) table class.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* For unpredictable traffic (moderate to high), on-demand mode is recommended. Standard table class is appropriate (Standard-IA is for infrequent access). On-demand scales automatically without capacity planning. Provisioned with auto scaling (A) may lag behind sudden spikes. (AWS Docs: DynamoDB On-Demand).
</details>

---

**Q521.** A retail company has several businesses. The IT team for each business manages its own AWS account. Each team account is part of an organization in AWS Organizations. Each team monitors its product inventory levels in an Amazon DynamoDB table in the team's own AWS account. The company is deploying a central inventory reporting application into a shared AWS account. The application must be able to read items from all the teams' DynamoDB tables. Which authentication option will meet these requirements MOST securely?

- A. Integrate DynamoDB with AWS Secrets Manager in the inventory application account. Configure the application to use the correct secret from Secrets Manager to authenticate and read the DynamoDB table. Schedule secret rotation for every 30 days.
- B. In every business account, create an IAM user that has programmatic access. Configure the application to use the correct IAM user access key ID and secret access key to authenticate and read the DynamoDB table. Manually rotate IAM access keys every 30 days.
- C. In every business account, create an IAM role named BU_ROLE with a policy that gives the role access to the DynamoDB table and a trust policy to trust a specific role in the inventory application account. In the inventory account, create a role named APP_ROLE that allows access to the STS AssumeRole API operation. Configure the application to use APP_ROLE and assume the cross-account role BU_ROLE to read the DynamoDB table.
- D. Integrate DynamoDB with AWS Certificate Manager (ACM). Generate identity certificates to authenticate DynamoDB. Configure the application to use the correct certificate to authenticate and read the DynamoDB table.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Cross-account IAM roles are the most secure way. Each business account creates a role with access to its DynamoDB table and trusts the inventory account's role. The inventory account assumes these roles to read data. No long-term credentials are stored. Options A (Secrets Manager) and B (IAM users) use long-term credentials. Option D (ACM) is not for DynamoDB authentication. (AWS Docs: Cross-Account DynamoDB Access).
</details>

---

**Q522.** A company runs container applications by using Amazon Elastic Kubernetes Service (Amazon EKS). The company's workload is not consistent throughout the day. The company wants Amazon EKS to scale in and out according to the workload. Which combination of steps will meet these requirements with the LEAST operational overhead? (Choose two.)

- A. Use an AWS Lambda function to resize the EKS cluster.
- B. Use the Kubernetes Metrics Server to activate horizontal pod autoscaling.
- C. Use the Kubernetes Cluster Autoscaler to manage the number of nodes in the cluster.
- D. Use Amazon API Gateway and connect it to Amazon EKS.
- E. Use AWS App Mesh to observe network activity.

<details>
<summary>Show Answer</summary>

**Answer: B, C**

*Explanation:* Horizontal Pod Autoscaler (B) scales pods based on metrics. Cluster Autoscaler (C) scales the number of nodes based on pod scheduling requirements. This is the standard way to scale EKS. Lambda (A) is not needed. API Gateway (D) is for ingress. App Mesh (E) is for service mesh. (AWS Docs: EKS Autoscaling).
</details>

---

**Q523.** A company runs a microservice-based serverless web application. The application must be able to retrieve data from multiple Amazon DynamoDB tables. A solutions architect needs to give the application the ability to retrieve the data with no impact on the baseline performance of the application. Which solution will meet these requirements in the MOST operationally efficient way?

- A. AWS AppSync pipeline resolvers
- B. Amazon CloudFront with Lambda@Edge functions
- C. Edge-optimized Amazon API Gateway with AWS Lambda functions
- D. Amazon Athena Federated Query with a DynamoDB connector

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS AppSync can use pipeline resolvers to fetch data from multiple DynamoDB tables in a single request, reducing overhead and improving performance. It is serverless and operationally efficient. CloudFront (B) is for caching. API Gateway + Lambda (C) would require multiple calls or custom batching. Athena (D) is for analytics, not low-latency app queries. (AWS Docs: AppSync Pipeline Resolvers).
</details>

---

**Q524.** A company wants to analyze and troubleshoot Access Denied errors and Unauthorized errors that are related to IAM permissions. The company has AWS CloudTrail turned on. Which solution will meet these requirements with the LEAST effort?

- A. Use AWS Glue and write custom scripts to query CloudTrail logs for the errors.
- B. Use AWS Batch and write custom scripts to query CloudTrail logs for the errors.
- C. Search CloudTrail logs with Amazon Athena queries to identify the errors.
- D. Search CloudTrail logs with Amazon QuickSight. Create a dashboard to identify the errors.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Athena can query CloudTrail logs stored in S3 using standard SQL, with no custom scripts (just create a table). This is the least effort. Glue (A) and Batch (B) require scripts. QuickSight (D) is for visualization, but Athena is easier for ad-hoc queries. (AWS Docs: Athena for CloudTrail).
</details>

---

**Q525.** A company wants to add its existing AWS usage cost to its operation cost dashboard. A solutions architect needs to recommend a solution that will give the company access to its usage cost programmatically. The company must be able to access cost data for the current year and forecast costs for the next 12 months. Which solution will meet these requirements with the LEAST operational overhead?

- A. Access usage cost-related data by using the AWS Cost Explorer API with pagination.
- B. Access usage cost-related data by using downloadable AWS Cost Explorer report .csv files.
- C. Configure AWS Budgets actions to send usage cost data to the company through FTP.
- D. Create AWS Budgets reports for usage cost data. Send the data to the company through SMTP.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* The AWS Cost Explorer API provides programmatic access to cost data and forecasts. This is the standard way for integration into a cost dashboard. Option B (CSV) is not programmatic. Options C/D (Budgets) are for alerts, not data feeds. (AWS Docs: Cost Explorer API).
</details>

---

**Q526.** A solutions architect is reviewing the resilience of an application. The solutions architect notices that a database administrator recently failed over the application's Amazon Aurora PostgreSQL database writer instance as part of a scaling exercise. The failover resulted in 3 minutes of downtime for the application. Which solution will reduce the downtime for scaling exercises with the LEAST operational overhead?

- A. Create more Aurora PostgreSQL read replicas in the cluster to handle the load during failover.
- B. Set up a secondary Aurora PostgreSQL cluster in the same AWS Region. During failover, update the application to use the secondary cluster's writer endpoint.
- C. Create an Amazon ElastiCache for Memcached cluster to handle the load during failover.
- D. Set up an Amazon RDS proxy for the database. Update the application to use the proxy endpoint.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* RDS Proxy reduces failover time by maintaining connections and directing traffic to the new writer faster (typically under 1 second). This has low operational overhead. Read replicas (A) don't reduce failover time for the writer. Secondary cluster (B) requires application changes. ElastiCache (C) is for caching. (AWS Docs: RDS Proxy Failover).
</details>

---

**Q527.** A company has a regional subscription-based streaming service that runs in a single AWS Region. The architecture consists of web servers and application servers on Amazon EC2 instances. The EC2 instances are in Auto Scaling groups behind Elastic Load Balancers. The architecture includes an Amazon Aurora global database cluster that extends across multiple Availability Zones. The company wants to expand globally and to ensure that its application has minimal downtime. Which solution will provide the MOST fault tolerance?

- A. Extend the Auto Scaling groups for the web tier and the application tier to deploy instances in Availability Zones in a second Region. Use an Aurora global database to deploy the database in the primary Region and the second Region. Use Amazon Route 53 health checks with a failover routing policy to the second Region.
- B. Deploy the web tier and the application tier to a second Region. Add an Aurora PostgreSQL cross-Region Aurora Replica in the second Region. Use Amazon Route 53 health checks with a failover routing policy to the second Region. Promote the secondary to primary as needed.
- C. Deploy the web tier and the application tier to a second Region. Create an Aurora PostgreSQL database in the second Region. Use AWS Database Migration Service (AWS DMS) to replicate the primary database to the second Region. Use Amazon Route 53 health checks with a failover routing policy to the second Region.
- D. Deploy the web tier and the application tier to a second Region. Use an Amazon Aurora global database to deploy the database in the primary Region and the second Region. Use Amazon Route 53 health checks with a failover routing policy to the second Region. Promote the secondary to primary as needed.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Aurora Global Database provides managed cross-Region replication with fast failover (typically <1 minute). Deploying both tiers in both Regions and using Route 53 failover provides maximum fault tolerance. Option A (extend ASG to second Region) is not the same as deploying there. Option B (cross-Region replica) is less managed. Option C (DMS) has higher latency. (AWS Docs: Aurora Global Database Failover).
</details>

---

**Q528.** A data analytics company wants to migrate its batch processing system to AWS. The company receives thousands of small data files periodically during the day through FTP. An on-premises batch job processes the data files overnight. However, the batch job takes hours to finish running. The company wants the AWS solution to process incoming data files as soon as possible with minimal changes to the FTP clients that send the files. The solution must delete the incoming data files after the files have been processed successfully. Processing for each file needs to take 3-8 minutes. Which solution will meet these requirements in the MOST operationally efficient way?

- A. Use an Amazon EC2 instance that runs an FTP server to store incoming files as objects in Amazon S3 Glacier Flexible Retrieval. Configure a job queue in AWS Batch. Use Amazon EventBridge rules to invoke the job to process the objects nightly from S3 Glacier Flexible Retrieval. Delete the objects after the job has processed the objects.
- B. Use an Amazon EC2 instance that runs an FTP server to store incoming files on an Amazon Elastic Block Store (Amazon EBS) volume. Configure a job queue in AWS Batch. Use Amazon EventBridge rules to invoke the job to process the files nightly from the EBS volume. Delete the files after the job has processed the files.
- C. Use AWS Transfer Family to create an FTP server to store incoming files on an Amazon Elastic Block Store (Amazon EBS) volume. Configure a job queue in AWS Batch. Use an Amazon S3 event notification when each file arrives to invoke the job in AWS Batch. Delete the files after the job has processed the files.
- D. Use AWS Transfer Family to create an FTP server to store incoming files in Amazon S3 Standard. Create an AWS Lambda function to process the files and to delete the files after they are processed. Use an S3 event notification to invoke the Lambda function when the files arrive.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Transfer Family provides managed FTP. S3 event notification triggers Lambda immediately. Lambda can process each file (3-8 minutes, within Lambda's 15 min limit) and delete it. This is serverless, processes files as soon as they arrive, and has minimal changes (just point FTP to Transfer Family). Option A/B/C use EC2 or Batch, which have more overhead and are not "as soon as possible" (nightly). (AWS Docs: Transfer Family, S3 Event to Lambda).
</details>

---

**Q529.** A company is migrating its workloads to AWS. The company has transactional and sensitive data in its databases. The company wants to use AWS Cloud solutions to increase security and reduce operational overhead for the databases. Which solution will meet these requirements?

- A. Migrate the databases to Amazon EC2. Use an AWS Key Management Service (AWS KMS) AWS managed key for encryption.
- B. Migrate the databases to Amazon RDS. Configure encryption at rest.
- C. Migrate the data to Amazon S3. Use Amazon Macie for data security and protection.
- D. Migrate the database to Amazon RDS. Use Amazon CloudWatch Logs for data security and protection.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* RDS is a managed database service that reduces operational overhead. Configuring encryption at rest (using KMS) provides security. EC2 (A) has higher overhead. S3 (C) is not a database. CloudWatch Logs (D) is for logging, not database security. (AWS Docs: RDS Encryption).
</details>

---

**Q530.** A company has an online gaming application that has TCP and UDP multiplayer gaming capabilities. The company uses Amazon Route 53 to point the application traffic to multiple Network Load Balancers (NLBs) in different AWS Regions. The company needs to improve application performance and decrease latency for the online game in preparation for user growth. Which solution will meet these requirements?

- A. Add an Amazon CloudFront distribution in front of the NLBs. Increase the Cache-Control max-age parameter.
- B. Replace the NLBs with Application Load Balancers (ALBs). Configure Route 53 to use latency-based routing.
- C. Add AWS Global Accelerator in front of the NLBs. Configure a Global Accelerator endpoint to use the correct listener ports.
- D. Add an Amazon API Gateway endpoint behind the NLBs. Enable API caching. Override method caching for the different stages.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Global Accelerator supports TCP/UDP, provides static IPs, and routes traffic to optimal endpoints based on latency, improving performance. CloudFront (A) does not support UDP. ALBs (B) do not support UDP. API Gateway (D) is for HTTP. (AWS Docs: Global Accelerator for Gaming).
</details>

---

**Q531.** A company needs to integrate with a third-party data feed. The data feed sends a webhook to notify an external service when new data is ready for consumption. A developer wrote an AWS Lambda function to retrieve data when the company receives a webhook callback. The developer must make the Lambda function available for the third party to call. Which solution will meet these requirements with the MOST operational efficiency?

- A. Create a function URL for the Lambda function. Provide the Lambda function URL to the third party for the webhook.
- B. Deploy an Application Load Balancer (ALB) in front of the Lambda function. Provide the ALB URL to the third party for the webhook.
- C. Create an Amazon Simple Notification Service (Amazon SNS) topic. Attach the topic to the Lambda function. Provide the public hostname of the SNS topic to the third party for the webhook.
- D. Create an Amazon Simple Queue Service (Amazon SQS) queue. Attach the queue to the Lambda function. Provide the public hostname of the SQS queue to the third party for the webhook.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Lambda function URLs provide a built-in HTTPS endpoint for invoking a function, with the least operational overhead. ALB (B) requires additional setup. SNS (C) and SQS (D) are not designed for webhooks (they are for messaging). (AWS Docs: Lambda Function URLs).
</details>

---

**Q532.** A company has a workload in an AWS Region. Customers connect to and access the workload by using an Amazon API Gateway REST API. The company uses Amazon Route 53 as its DNS provider. The company wants to provide individual and secure URLs for all customers. Which combination of steps will meet these requirements with the MOST operational efficiency? (Choose three.)

- A. Register the required domain in a registrar. Create a wildcard custom domain name in a Route 53 hosted zone and record in the zone that points to the API Gateway endpoint.
- B. Request a wildcard certificate that matches the domains in AWS Certificate Manager (ACM) in a different Region.
- C. Create hosted zones for each customer as required in Route 53. Create zone records that point to the API Gateway endpoint.
- D. Request a wildcard certificate that matches the custom domain name in AWS Certificate Manager (ACM) in the same Region.
- E. Create multiple API endpoints for each customer in API Gateway.
- F. Create a custom domain name in API Gateway for the REST API. Import the certificate from AWS Certificate Manager (ACM).

<details>
<summary>Show Answer</summary>

**Answer: A, D, F**

*Explanation:* Register domain (A). Request wildcard certificate in the same Region (D). Create custom domain name in API Gateway and import certificate (F). Create Route 53 wildcard record pointing to API Gateway endpoint (A). This provides individual URLs (e.g., customer1.example.com) efficiently. Option B (different Region) is not needed. Option C (hosted zones per customer) is not efficient. Option E (multiple API endpoints) is not efficient. (AWS Docs: API Gateway Custom Domain with Wildcard).
</details>

---

**Q533.** A company stores data in Amazon S3. According to regulations, the data must not contain personally identifiable information (PII). The company recently discovered that S3 buckets have some objects that contain PII. The company needs to automatically detect PII in S3 buckets and to notify the company's security team. Which solution will meet these requirements?

- A. Use Amazon Macie. Create an Amazon EventBridge rule to filter the SensitiveData event type from Macie findings and to send an Amazon Simple Notification Service (Amazon SNS) notification to the security team.
- B. Use Amazon GuardDuty. Create an Amazon EventBridge rule to filter the CRITICAL event type from GuardDuty findings and to send an Amazon Simple Notification Service (Amazon SNS) notification to the security team.
- C. Use Amazon Macie. Create an Amazon EventBridge rule to filter the SensitiveData:S3Object/Personal event type from Macie findings and to send an Amazon Simple Queue Service (Amazon SQS) notification to the security team.
- D. Use Amazon GuardDuty. Create an Amazon EventBridge rule to filter the CRITICAL event type from GuardDuty findings and to send an Amazon Simple Queue Service (Amazon SQS) notification to the security team.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Amazon Macie detects PII in S3. EventBridge can filter findings and trigger SNS for email/SMS notifications to the security team. GuardDuty (B, D) is for threat detection. SQS (C, D) is for queues, not direct notifications. (AWS Docs: Macie with EventBridge and SNS).
</details>

---

**Q534.** A company wants to build a logging solution for its multiple AWS accounts. The company currently stores the logs from all accounts in a centralized account. The company has created an Amazon S3 bucket in the centralized account to store the VPC flow logs and AWS CloudTrail logs. All logs must be highly available for 30 days for frequent analysis, retained for an additional 60 days for backup purposes, and deleted 90 days after creation. Which solution will meet these requirements MOST cost-effectively?

- A. Transition objects to the S3 Standard storage class 30 days after creation. Write an expiration action that directs Amazon S3 to delete objects after 90 days.
- B. Transition objects to the S3 Standard-Infrequent Access (S3 Standard-IA) storage class 30 days after creation. Move all objects to the S3 Glacier Flexible Retrieval storage class after 90 days. Write an expiration action that directs Amazon S3 to delete objects after 90 days.
- C. Transition objects to the S3 Glacier Flexible Retrieval storage class 30 days after creation. Write an expiration action that directs Amazon S3 to delete objects after 90 days.
- D. Transition objects to the S3 One Zone-Infrequent Access (S3 One Zone-IA) storage class 30 days after creation. Move all objects to the S3 Glacier Flexible Retrieval storage class after 90 days. Write an expiration action that directs Amazon S3 to delete objects after 90 days.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* First 30 days: S3 Standard (frequent access). Days 30-90: S3 Standard-IA (infrequent access). After 90 days: delete. Option A (Standard for 90 days) is more expensive. Option C (Glacier from day 30) has retrieval delays for the first 30 days. Option D (One Zone-IA) is not highly available. (AWS Docs: S3 Lifecycle for Logs).
</details>

---

**Q535.** A company is building an Amazon Elastic Kubernetes Service (Amazon EKS) cluster for its workloads. All secrets that are stored in Amazon EKS must be encrypted in the Kubernetes etcd key-value store. Which solution will meet these requirements?

- A. Create a new AWS Key Management Service (AWS KMS) key. Use AWS Secrets Manager to manage, rotate, and store all secrets in Amazon EKS.
- B. Create a new AWS Key Management Service (AWS KMS) key. Enable Amazon EKS KMS secrets encryption on the Amazon EKS cluster.
- C. Create the Amazon EKS cluster with default options. Use the Amazon Elastic Block Store (Amazon EBS) Container Storage Interface (CSI) driver as an add-on.
- D. Create a new AWS Key Management Service (AWS KMS) key with the alias/aws/ebs alias. Enable default Amazon Elastic Block Store (Amazon EBS) volume encryption for the account.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* EKS supports envelope encryption of Kubernetes secrets using AWS KMS. You create a KMS key and enable secrets encryption on the EKS cluster. This encrypts secrets in etcd. Option A (Secrets Manager) is for external secrets. Option C (EBS CSI) is for volume encryption. Option D (EBS encryption) is for volumes. (AWS Docs: EKS Secrets Encryption).
</details>

---

**Q536.** A company wants to provide data scientists with near real-time read-only access to the company's production Amazon RDS for PostgreSQL database. The database is currently configured as a Single-AZ database. The data scientists use complex queries that will not affect the production database. The company needs a solution that is highly available. Which solution will meet these requirements MOST cost-effectively?

- A. Scale the existing production database in a maintenance window to provide enough power for the data scientists.
- B. Change the setup from a Single-AZ to a Multi-AZ instance deployment with a larger secondary standby instance. Provide the data scientists access to the secondary instance.
- C. Change the setup from a Single-AZ to a Multi-AZ instance deployment. Provide two additional read replicas for the data scientists.
- D. Change the setup from a Single-AZ to a Multi-AZ cluster deployment with two readable standby instances. Provide read endpoints to the data scientists.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* RDS Multi-AZ DB cluster deployment (for PostgreSQL) provides two readable standby instances, allowing read traffic offloading with high availability. This is more cost-effective than separate read replicas (C) and provides near-real-time (synchronous replication). Option B (Multi-AZ instance) secondary is not readable. Option A (scaling up) doesn't provide HA. (AWS Docs: RDS Multi-AZ DB Cluster).
</details>

---

**Q537.** A company runs a three-tier web application in the AWS Cloud that operates across three Availability Zones. The application architecture has an Application Load Balancer, an Amazon EC2 web server that hosts user session states, and a MySQL database that runs on an EC2 instance. The company expects sudden increases in application traffic. The company wants to be able to scale to meet future application capacity demands and to ensure high availability across all three Availability Zones. Which solution will meet these requirements?

- A. Migrate the MySQL database to Amazon RDS for MySQL with a Multi-AZ DB cluster deployment. Use Amazon ElastiCache for Redis with high availability to store session data and to cache reads. Migrate the web server to an Auto Scaling group that is in three Availability Zones.
- B. Migrate the MySQL database to Amazon RDS for MySQL with a Multi-AZ DB cluster deployment. Use Amazon ElastiCache for Memcached with high availability to store session data and to cache reads. Migrate the web server to an Auto Scaling group that is in three Availability Zones.
- C. Migrate the MySQL database to Amazon DynamoDB. Use DynamoDB Accelerator (DAX) to cache reads. Store the session data in DynamoDB. Migrate the web server to an Auto Scaling group that is in three Availability Zones.
- D. Migrate the MySQL database to Amazon RDS for MySQL in a single Availability Zone. Use Amazon ElastiCache for Redis with high availability to store session data and to cache reads. Migrate the web server to an Auto Scaling group that is in three Availability Zones.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* RDS Multi-AZ DB cluster provides HA for database. ElastiCache for Redis (supports HA, persistence) for session storage. Web tier in Auto Scaling group across 3 AZs. Memcached (B) does not support persistence or HA. DynamoDB (C) is non-relational. Single-AZ (D) is not HA. (AWS Docs: ElastiCache for Redis Session Storage, RDS Multi-AZ Cluster).
</details>

---

**Q538.** A global video streaming company uses Amazon CloudFront as a content distribution network (CDN). The company wants to roll out content in a phased manner across multiple countries. The company needs to ensure that viewers who are outside the countries to which the company rolls out content are not able to view the content. Which solution will meet these requirements?

- A. Add geographic restrictions to the content in CloudFront by using an allow list. Set up a custom error message.
- B. Set up a new URL for restricted content. Authorize access by using a signed URL and cookies. Set up a custom error message.
- C. Encrypt the data for the content that the company distributes. Set up a custom error message.
- D. Create a new URL for restricted content. Set up a time-restricted access policy for signed URLs.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudFront geographic restrictions (geo blocking) allow you to create an allow list of countries. Viewers outside those countries are blocked. Signed URLs/cookies (B, D) control access based on authentication, not geography. Encryption (C) doesn't block access. (AWS Docs: CloudFront Geo Restriction).
</details>

---

**Q539.** A company wants to use the AWS Cloud to improve its on-premises disaster recovery (DR) configuration. The company's core production business application uses Microsoft SQL Server Standard, which runs on a virtual machine (VM). The application has a recovery point objective (RPO) of 30 seconds or fewer and a recovery time objective (RTO) of 60 minutes. The DR solution needs to minimize costs wherever possible. Which solution will meet these requirements?

- A. Configure a multi-site active/active setup between the on-premises server and AWS by using Microsoft SQL Server Enterprise with Always On availability groups.
- B. Configure a warm standby Amazon RDS for SQL Server database on AWS. Configure AWS Database Migration Service (AWS DMS) to use change data capture (CDC).
- C. Use AWS Elastic Disaster Recovery configured to replicate disk changes to AWS as a pilot light.
- D. Use third-party backup software to capture backups every night. Store a secondary set of backups in Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Elastic Disaster Recovery (DRS) can replicate on-premises VMs to AWS with low RPO (seconds) and RTO (minutes). It supports pilot light mode to minimize costs. Option A (Active/Active) requires SQL Server Enterprise (expensive). Option B (DMS) has higher latency. Option D (nightly backups) fails RPO of 30 seconds. (AWS Docs: AWS DRS).
</details>

---

**Q540.** A company has an on-premises server that uses an Oracle database to process and store customer information. The company wants to use an AWS database service to achieve higher availability and to improve application performance. The company also wants to offload reporting from its primary database system. Which solution will meet these requirements in the MOST operationally efficient way?

- A. Use AWS Database Migration Service (AWS DMS) to create an Amazon RDS DB instance in multiple AWS Regions. Point the reporting functions toward a separate DB instance from the primary DB instance.
- B. Use Amazon RDS in a Single-AZ deployment to create an Oracle database. Create a read replica in the same zone as the primary DB instance. Direct the reporting functions to the read replica.
- C. Use Amazon RDS deployed in a Multi-AZ cluster deployment to create an Oracle database. Direct the reporting functions to use the reader instance in the cluster deployment.
- D. Use Amazon RDS deployed in a Multi-AZ instance deployment to create an Amazon Aurora database. Direct the reporting functions to the reader instances.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* RDS Multi-AZ cluster deployment (Oracle) provides reader instances for offloading reporting. This is operationally efficient. Option A (cross-Region) adds latency. Option B (Single-AZ) not HA. Option D (Aurora) is not Oracle-compatible. (AWS Docs: RDS Multi-AZ Cluster for Oracle).
</details>

---

**Q541.** A company wants to build a web application on AWS. Client access requests to the website are not predictable and can be idle for a long time. Only customers who have paid a subscription fee can have the ability to sign in and use the web application. Which combination of steps will meet these requirements MOST cost-effectively? (Choose three.)

- A. Create an AWS Lambda function to retrieve user information from Amazon DynamoDB. Create an Amazon API Gateway endpoint to accept RESTful APIs. Send the API calls to the Lambda function.
- B. Create an Amazon Elastic Container Service (Amazon ECS) service behind an Application Load Balancer to retrieve user information from Amazon RDS. Create an Amazon API Gateway endpoint to accept RESTful APIs. Send the API calls to the Lambda function.
- C. Create an Amazon Cognito user pool to authenticate users.
- D. Create an Amazon Cognito identity pool to authenticate users.
- E. Use AWS Amplify to serve the frontend web content with HTML, CSS, and JS. Use an integrated Amazon CloudFront configuration.
- F. Use Amazon S3 static web hosting with PHP, CSS, and JS. Use Amazon CloudFront to serve the frontend web content.

<details>
<summary>Show Answer</summary>

**Answer: A, C, E**

*Explanation:* Serverless architecture: Amplify (E) for frontend hosting, Cognito user pool (C) for authentication, API Gateway + Lambda + DynamoDB (A) for backend. This is cost-effective for unpredictable traffic. Option B (ECS) has overhead. Option D (identity pool) is for AWS credentials. Option F (S3 with PHP) doesn't work (S3 static hosting doesn't support PHP). (AWS Docs: Amplify, Cognito, API Gateway + Lambda).
</details>

---

**Q542.** A media company uses an Amazon CloudFront distribution to deliver content over the internet. The company wants only premium customers to have access to the media streams and file content. The company stores all content in an Amazon S3 bucket. The company also delivers content on demand to customers for a specific purpose, such as movie rentals or music downloads. Which solution will meet these requirements?

- A. Generate and provide S3 signed cookies to premium customers.
- B. Generate and provide CloudFront signed URLs to premium customers.
- C. Use origin access control (OAC) to limit the access of non-premium customers.
- D. Generate and activate field-level encryption to block non-premium customers.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* CloudFront signed URLs are ideal for on-demand, time-limited access to content (e.g., movie rentals). Signed cookies (A) are for multiple files but require cookie support. OAC (C) is for origin access, not customer authorization. Field-level encryption (D) is for protecting specific data fields. (AWS Docs: CloudFront Signed URLs).
</details>

---

**Q543.** A company runs Amazon EC2 instances in multiple AWS accounts that are individually billed. The company recently purchased a Savings Plan. Because of changes in the company's business requirements, the company has decommissioned a large number of EC2 instances. The company wants to use its Savings Plan discounts on its other AWS accounts. Which combination of steps will meet these requirements? (Choose two.)

- A. From the AWS Account Management Console of the management account, turn on discount sharing from the billing preferences section.
- B. From the AWS Account Management Console of the account that purchased the existing Savings Plan, turn on discount sharing from the billing preferences section. Include all accounts.
- C. From the AWS Organizations management account, use AWS Resource Access Manager (AWS RAM) to share the Savings Plan with other accounts.
- D. Create an organization in AWS Organizations in a new payer account. Invite the other AWS accounts to join the organization from the management account.
- E. Create an organization in AWS Organizations in the existing AWS account with the existing EC2 instances and Savings Plan. Invite the other AWS accounts to join the organization from the management account.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* First, create an organization in the account that owns the Savings Plan (E). Then, from that account's billing preferences, turn on discount sharing (B). Discounts can then be shared across all accounts in the organization. Option A (management account) is not the payer if the Savings Plan is in another account. Option C (RAM) is not for Savings Plans. Option D (new payer) is not needed. (AWS Docs: Savings Plan Discount Sharing).
</details>

---

**Q544.** A retail company uses a regional Amazon API Gateway API for its public REST APIs. The API Gateway endpoint is a custom domain name that points to an Amazon Route 53 alias record. A solutions architect needs to create a solution that has minimal effects on customers and minimal data loss to release the new version of APIs. Which solution will meet these requirements?

- A. Create a canary release deployment stage for API Gateway. Deploy the latest API version. Point an appropriate percentage of traffic to the canary stage. After API verification, promote the canary stage to the production stage.
- B. Create a new API Gateway endpoint with a new version of the API in OpenAPI YAML file format. Use the import-to-update operation in merge mode into the API in API Gateway. Deploy the new version of the API to the production stage.
- C. Create a new API Gateway endpoint with a new version of the API in OpenAPI JSON file format. Use the import-to-update operation in overwrite mode into the API in API Gateway. Deploy the new version of the API to the production stage.
- D. Create a new API Gateway endpoint with new versions of the API definitions. Create a custom domain name for the new API Gateway API. Point the Route 53 alias record to the new API Gateway API custom domain name.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Canary release deployment allows you to send a percentage of traffic to the new version, verify, then promote. This minimizes customer impact and data loss. Options B/C (import-to-update) overwrite the existing API (risky). Option D (new endpoint) requires DNS change (potential downtime). (AWS Docs: API Gateway Canary Releases).
</details>

---

**Q545.** A company wants to direct its users to a backup static error page if the company's primary website is unavailable. The primary website's DNS records are hosted in Amazon Route 53. The domain is pointing to an Application Load Balancer (ALB). The company needs a solution that minimizes changes and infrastructure overhead. Which solution will meet these requirements?

- A. Update the Route 53 records to use a latency routing policy. Add a static error page that is hosted in an Amazon S3 bucket to the records so that the traffic is sent to the most responsive endpoints.
- B. Set up a Route 53 active-passive failover configuration. Direct traffic to a static error page that is hosted in an Amazon S3 bucket when Route 53 health checks determine that the ALB endpoint is unhealthy.
- C. Set up a Route 53 active-active configuration with the ALB and an Amazon EC2 instance that hosts a static error page as endpoints. Configure Route 53 to send requests to the instance only if the health checks fail for the ALB.
- D. Update the Route 53 records to use a multivalue answer routing policy. Create a health check. Direct traffic to the website if the health check passes. Direct traffic to a static error page that is hosted in Amazon S3 if the health check does not pass.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Route 53 active-passive failover with health checks can route traffic to a static S3 website error page when the primary ALB is unhealthy. This minimizes changes (just add failover record). Option A (latency) not for failover. Option C (active-active) is more complex. Option D (multivalue) is not for failover. (AWS Docs: Route 53 Failover with S3).
</details>

---

**Q546.** A recent analysis of a company's IT expenses highlights the need to reduce backup costs. The company's chief information officer wants to simplify the on-premises backup infrastructure and reduce costs by eliminating the use of physical backup tapes. The company must preserve the existing investment in the on-premises backup applications and workflows. What should a solutions architect recommend?

- A. Set up AWS Storage Gateway to connect with the backup applications using the NFS interface.
- B. Set up an Amazon EFS file system that connects with the backup applications using the NFS interface.
- C. Set up an Amazon EFS file system that connects with the backup applications using the iSCSI interface.
- D. Set up AWS Storage Gateway to connect with the backup applications using the iSCSI-virtual tape library (VTL) interface.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Storage Gateway Tape Gateway (VTL) presents a virtual tape library using iSCSI, allowing existing backup applications to write to virtual tapes instead of physical tapes, preserving workflows. NFS (A) is not for backup apps. EFS (B, C) is not for backup applications. (AWS Docs: Tape Gateway).
</details>

---

**Q547.** A company has data collection sensors at different locations. The data collection sensors stream a high volume of data to the company. The company wants to design a platform on AWS to ingest and process high-volume streaming data. The solution must be scalable and support data collection in near real time. The company must store the data in Amazon S3 for future reporting. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon Kinesis Data Firehose to deliver streaming data to Amazon S3.
- B. Use AWS Glue to deliver streaming data to Amazon S3.
- C. Use AWS Lambda to deliver streaming data and store the data to Amazon S3.
- D. Use AWS Database Migration Service (AWS DMS) to deliver streaming data to Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Kinesis Data Firehose is a fully managed service for streaming data ingestion and delivery to S3, with minimal operational overhead. Glue (B) is for ETL, not real-time ingestion. Lambda (C) has scaling limits and requires more management. DMS (D) is for database migration. (AWS Docs: Kinesis Firehose).
</details>

---

**Q548.** A company has separate AWS accounts for its finance, data analytics, and development departments. Because of costs and security concerns, the company wants to control which services each AWS account can use. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Systems Manager templates to control which AWS services each department can use.
- B. Create organization units (OUs) for each department in AWS Organizations. Attach service control policies (SCPs) to the OUs.
- C. Use AWS CloudFormation to automatically provision only the AWS services that each department can use.
- D. Set up a list of products in AWS Service Catalog in the AWS accounts to manage and control the usage of specific AWS services.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* SCPs attached to OUs in AWS Organizations allow you to centrally control which services can be used in member accounts, with least overhead. Service Catalog (D) is for provisioning approved products, not denying services. Systems Manager (A) is for management. CloudFormation (C) is for provisioning. (AWS Docs: SCPs for Service Restriction).
</details>

---

**Q549.** A company has created a multi-tier application for its ecommerce website. The website uses an Application Load Balancer that resides in the public subnets, a web tier in the public subnets, and a MySQL cluster hosted on Amazon EC2 instances in the private subnets. The MySQL database needs to retrieve product catalog and pricing information that is hosted on the internet by a third-party provider. A solutions architect must devise a strategy that maximizes security without increasing operational overhead. What should the solutions architect do to meet these requirements?

- A. Deploy a NAT instance in the VPC. Route all the internet-based traffic through the NAT instance.
- B. Deploy a NAT gateway in the public subnets. Modify the private subnet route table to direct all internet-bound traffic to the NAT gateway.
- C. Configure an internet gateway and attach it to the VPC. Modify the private subnet route table to direct internet-bound traffic to the internet gateway.
- D. Configure a virtual private gateway and attach it to the VPC. Modify the private subnet route table to direct internet-bound traffic to the virtual private gateway.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* NAT gateway in public subnets allows private subnet EC2 instances (MySQL) to access the internet (to retrieve catalog info) while preventing inbound connections. This is managed (low operational overhead) and secure. NAT instance (A) has more overhead. IGW (C) would make instances public. VPN gateway (D) is for on-premises. (AWS Docs: NAT Gateway).
</details>

---

**Q550.** A company is using AWS Key Management Service (AWS KMS) keys to encrypt AWS Lambda environment variables. A solutions architect needs to ensure that the required permissions are in place to decrypt and use the environment variables. Which steps must the solutions architect take to implement the correct permissions? (Choose two.)

- A. Add AWS KMS permissions in the Lambda resource policy.
- B. Add AWS KMS permissions in the Lambda execution role.
- C. Add AWS KMS permissions in the Lambda function policy.
- D. Allow the Lambda execution role in the AWS KMS key policy.
- E. Allow the Lambda resource policy in the AWS KMS key policy.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* The Lambda execution role (B) needs `kms:Decrypt` permission. The KMS key policy must allow the execution role to use the key (D). Resource policy (A, C) is for who can invoke the function. (AWS Docs: Lambda Environment Variable Encryption).
</details>

---

**Q551.** A company has a financial application that produces reports. The reports average 50 KB in size and are stored in Amazon S3. The reports are frequently accessed during the first week after production and must be stored for several years. The reports must be retrievable within 6 hours. Which solution meets these requirements MOST cost-effectively?

- A. Use S3 Standard. Use an S3 Lifecycle rule to transition the reports to S3 Glacier after 7 days.
- B. Use S3 Standard. Use an S3 Lifecycle rule to transition the reports to S3 Standard-Infrequent Access (S3 Standard-IA) after 7 days.
- C. Use S3 Intelligent-Tiering. Configure S3 Intelligent-Tiering to transition the reports to S3 Standard-Infrequent Access (S3 Standard-IA) and S3 Glacier.
- D. Use S3 Standard. Use an S3 Lifecycle rule to transition the reports to S3 Glacier Deep Archive after 7 days.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Glacier (Standard retrieval) provides 3-5 hour retrieval, meeting "within 6 hours". Glacier Deep Archive (D) has retrieval times of 12-48 hours. Option B (Standard-IA) is more expensive for long-term. Option C (Intelligent-Tiering) has monitoring costs. (AWS Docs: Glacier Retrieval Options).
</details>

---

**Q552.** A company needs to optimize the cost of its Amazon EC2 instances. The company also needs to change the type and family of its EC2 instances every 2-3 months. What should the company do to meet these requirements?

- A. Purchase Partial Upfront Reserved Instances for a 3-year term.
- B. Purchase a No Upfront Compute Savings Plan for a 1-year term.
- C. Purchase All Upfront Reserved Instances for a 1-year term.
- D. Purchase an All Upfront EC2 Instance Savings Plan for a 1-year term.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Compute Savings Plan offers flexibility to change instance family/type, and No Upfront minimizes initial cost. 1-year term matches the change frequency. Reserved Instances (A, C) and EC2 Instance Savings Plan (D) are locked to a specific instance family. (AWS Docs: Compute Savings Plan Flexibility).
</details>

---

**Q553.** A solutions architect needs to review a company's Amazon S3 buckets to discover personally identifiable information (PII). The company stores the PII data in the us-east-1 Region and us-west-2 Region. Which solution will meet these requirements with the LEAST operational overhead?

- A. Configure Amazon Macie in each Region. Create a job to analyze the data that is in Amazon S3.
- B. Configure AWS Security Hub for all Regions. Create an AWS Config rule to analyze the data that is in Amazon S3.
- C. Configure Amazon Inspector to analyze the data that is in Amazon S3.
- D. Configure Amazon GuardDuty to analyze the data that is in Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Amazon Macie is the service for discovering PII in S3. You need to configure it in each Region where data resides. Security Hub (B) is for security posture. Inspector (C) is for EC2 vulnerabilities. GuardDuty (D) is for threat detection. (AWS Docs: Macie Multi-Region).
</details>

---

**Q554.** A company's SAP application has a backend SQL Server database in an on-premises environment. The company wants to migrate its on-premises application and database server to AWS. The company needs an instance type that meets the high demands of its SAP database. On-premises performance data shows that both the SAP application and the database have high memory utilization. Which solution will meet these requirements?

- A. Use the compute optimized instance family for the application. Use the memory optimized instance family for the database.
- B. Use the storage optimized instance family for both the application and the database.
- C. Use the memory optimized instance family for both the application and the database.
- D. Use the high performance computing (HPC) optimized instance family for the application. Use the memory optimized instance family for the database.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Both application and database have high memory utilization, so memory optimized instances (R-family) are appropriate for both. Compute optimized (A) is for compute-heavy. Storage optimized (B) is for I/O-heavy. HPC (D) is for specialized workloads. (AWS Docs: EC2 Instance Families).
</details>

---

**Q555.** A company runs an application in a VPC with public and private subnets. The VPC extends across multiple Availability Zones. The application runs on Amazon EC2 instances in private subnets. The application uses an Amazon Simple Queue Service (Amazon SQS) queue. A solutions architect needs to design a secure solution to establish a connection between the EC2 instances and the SQS queue. Which solution will meet these requirements?

- A. Implement an interface VPC endpoint for Amazon SQS. Configure the endpoint to use the private subnets. Add to the endpoint a security group that has an inbound access rule that allows traffic from the EC2 instances that are in the private subnets.
- B. Implement an interface VPC endpoint for Amazon SQS. Configure the endpoint to use the public subnets. Attach to the interface endpoint a VPC endpoint policy that allows access from the EC2 instances that are in the private subnets.
- C. Implement an interface VPC endpoint for Amazon SQS. Configure the endpoint to use the public subnets. Attach an Amazon SQS access policy to the interface VPC endpoint that allows requests from only a specified VPC endpoint.
- D. Implement a gateway endpoint for Amazon SQS. Add a NAT gateway to the private subnets. Attach an IAM role to the EC2 instances that allows access to the SQS queue.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* SQS supports interface VPC endpoints. The endpoint should be in the private subnets (where EC2 instances are). Security group on the endpoint should allow inbound from EC2 instances' security group. Option B (public subnets) is less secure. Option C (public subnets). Option D (gateway endpoint) does not exist for SQS (SQS only interface endpoints). (AWS Docs: SQS VPC Endpoint).
</details>

---

**Q556.** A solutions architect is using an AWS CloudFormation template to deploy a three-tier web application. The web application consists of a web tier and an application tier that stores and retrieves user data in Amazon DynamoDB tables. The web and application tiers are hosted on Amazon EC2 instances, and the database tier is not publicly accessible. The application EC2 instances need to access the DynamoDB tables without exposing API credentials in the template. What should the solutions architect do to meet these requirements?

- A. Create an IAM role to read the DynamoDB tables. Associate the role with the application instances by referencing an instance profile.
- B. Create an IAM role that has the required permissions to read and write from the DynamoDB tables. Add the role to the EC2 instance profile, and associate the instance profile with the application instances.
- C. Use the parameter section in the AWS CloudFormation template to have the user input access and secret keys from an already-created IAM user that has the required permissions to read and write from the DynamoDB tables.
- D. Create an IAM user in the AWS CloudFormation template that has the required permissions to read and write from the DynamoDB tables. Use the GetAtt function to retrieve the access and secret keys, and pass them to the application instances through the user data.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Create an IAM role with DynamoDB permissions, add it to an instance profile, and associate the instance profile with the EC2 instances in the CloudFormation template. This avoids hardcoding credentials. Option A (role to read) is not specific. Option C (user input) and D (create IAM user) expose credentials. (AWS Docs: EC2 Instance Profile with CloudFormation).
</details>

---

**Q557.** A solutions architect manages an analytics application. The application stores large amounts of semistructured data in an Amazon S3 bucket. The solutions architect wants to use parallel data processing to process the data more quickly. The solutions architect also wants to use information that is stored in an Amazon Redshift database to enrich the data. Which solution will meet these requirements?

- A. Use Amazon Athena to process the S3 data. Use AWS Glue with the Amazon Redshift data to enrich the S3 data.
- B. Use Amazon EMR to process the S3 data. Use Amazon EMR with the Amazon Redshift data to enrich the S3 data.
- C. Use Amazon EMR to process the S3 data. Use Amazon Kinesis Data Streams to move the S3 data into Amazon Redshift so that the data can be enriched.
- D. Use AWS Glue to process the S3 data. Use AWS Lake Formation with the Amazon Redshift data to enrich the S3 data.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon EMR can process data in S3 and also connect to Redshift (via JDBC) to enrich the data in parallel. Athena (A) is for querying, not complex processing. Kinesis (C) is for streaming. Lake Formation (D) is for data lake management. (AWS Docs: EMR with Redshift).
</details>

---

**Q558.** A company has two VPCs that are located in the us-west-2 Region within the same AWS account. The company needs to allow network traffic between these VPCs. Approximately 500 GB of data transfer will occur between the VPCs each month. What is the MOST cost-effective solution to connect these VPCs?

- A. Implement AWS Transit Gateway to connect the VPCs. Update the route tables of each VPC to use the transit gateway for inter-VPC communication.
- B. Implement an AWS Site-to-Site VPN tunnel between the VPCs. Update the route tables of each VPC to use the VPN tunnel for inter-VPC communication.
- C. Set up a VPC peering connection between the VPCs. Update the route tables of each VPC to use the VPC peering connection for inter-VPC communication.
- D. Set up a 1 GB AWS Direct Connect connection between the VPCs. Update the route tables of each VPC to use the Direct Connect connection for inter-VPC communication.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* VPC peering is free (no data transfer cost) for intra-Region traffic, making it the most cost-effective for connecting two VPCs in the same Region. Transit Gateway (A) has hourly charges. VPN (B) and Direct Connect (D) have costs and complexity. (AWS Docs: VPC Peering Pricing).
</details>

---

**Q559.** A company hosts multiple applications on AWS for different product lines. The applications use different compute resources, including Amazon EC2 instances and Application Load Balancers. The applications run in different AWS accounts under the same organization in AWS Organizations across multiple AWS Regions. Teams for each product line have tagged each compute resource in the individual accounts. The company wants more details about the cost for each product line from the consolidated billing feature in Organizations. Which combination of steps will meet these requirements? (Choose two.)

- A. Select a specific AWS generated tag in the AWS Billing console.
- B. Select a specific user-defined tag in the AWS Billing console.
- C. Select a specific user-defined tag in the AWS Resource Groups console.
- D. Activate the selected tag from each AWS account.
- E. Activate the selected tag from the Organizations management account.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* User-defined cost allocation tags must be activated in the management account's Billing console (E). Then you select that tag in the Billing console (B) to see costs grouped by product line. AWS generated tags (A) are not user-defined. Resource Groups (C) is for resource organization, not cost reporting. (AWS Docs: Cost Allocation Tags).
</details>

---

**Q560.** A company's solutions architect is designing an AWS multi-account solution that uses AWS Organizations. The solutions architect has organized the company's accounts into organizational units (OUs). The solutions architect needs a solution that will identify any changes to the OU hierarchy. The solution also needs to notify the company's operations team of any changes. Which solution will meet these requirements with the LEAST operational overhead?

- A. Provision the AWS accounts by using AWS Control Tower. Use account drift notifications to identify the changes to the OU hierarchy.
- B. Provision the AWS accounts by using AWS Control Tower. Use AWS Config aggregated rules to identify the changes to the OU hierarchy.
- C. Use AWS Service Catalog to create accounts in Organizations. Use an AWS CloudTrail organization trail to identify the changes to the OU hierarchy.
- D. Use AWS CloudFormation templates to create accounts in Organizations. Use the drift detection operation on a stack to identify the changes to the OU hierarchy.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS CloudTrail organization trail records API calls including changes to OU hierarchy (e.g., `MoveAccount`). This can trigger EventBridge to notify operations. Control Tower (A, B) is for governance, not specifically for OU change detection. CloudFormation drift (D) is for stack resources, not OU hierarchy. (AWS Docs: CloudTrail for Organizations).
</details>

---

**Q561.** A company's website handles millions of requests each day, and the number of requests continues to increase. A solutions architect needs to improve the response time of the web application. The solutions architect determines that the application needs to decrease latency when retrieving product details from the Amazon DynamoDB table. Which solution will meet these requirements with the LEAST amount of operational overhead?

- A. Set up a DynamoDB Accelerator (DAX) cluster. Route all read requests through DAX.
- B. Set up Amazon ElastiCache for Redis between the DynamoDB table and the web application. Route all read requests through Redis.
- C. Set up Amazon ElastiCache for Memcached between the DynamoDB table and the web application. Route all read requests through Memcached.
- D. Set up Amazon DynamoDB Streams on the table, and have AWS Lambda read from the table and populate Amazon ElastiCache. Route all read requests through ElastiCache.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* DAX is a fully managed, in-memory cache for DynamoDB that reduces read latency from milliseconds to microseconds with minimal application changes (just change endpoint). ElastiCache (B, C, D) requires application changes to implement caching logic. (AWS Docs: DAX).
</details>

---

**Q562.** A solutions architect needs to ensure that API calls to Amazon DynamoDB from Amazon EC2 instances in a VPC do not travel across the internet. Which combination of steps should the solutions architect take to meet this requirement? (Choose two.)

- A. Create a route table entry for the endpoint.
- B. Create a gateway endpoint for DynamoDB.
- C. Create an interface endpoint for Amazon EC2.
- D. Create an elastic network interface for the endpoint in each of the subnets of the VPC.
- E. Create a security group entry in the endpoint's security group to provide access.

<details>
<summary>Show Answer</summary>

**Answer: A, B**

*Explanation:* DynamoDB uses a gateway endpoint (B). You need to create a route table entry (A) to direct traffic to the endpoint. Interface endpoints (C) are not for DynamoDB. ENI (D) is for interface endpoints. Security group (E) is for interface endpoints, not gateway endpoints. (AWS Docs: DynamoDB Gateway Endpoint).
</details>

---

**Q563.** A company runs its applications on both Amazon Elastic Kubernetes Service (Amazon EKS) clusters and on-premises Kubernetes clusters. The company wants to view all clusters and workloads from a central location. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon CloudWatch Container Insights to collect and group the cluster information.
- B. Use Amazon EKS Connector to register and connect all Kubernetes clusters.
- C. Use AWS Systems Manager to collect and view the cluster information.
- D. Use Amazon EKS Anywhere as the primary cluster to view the other clusters with native Kubernetes commands.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Amazon EKS Connector allows you to register and connect any Kubernetes cluster (EKS, on-premises, other cloud) to the AWS Management Console for centralized visibility. Container Insights (A) is for monitoring. Systems Manager (C) is for EC2. EKS Anywhere (D) is for on-premises EKS clusters. (AWS Docs: EKS Connector).
</details>

---

**Q564.** A company is building an ecommerce application and needs to store sensitive customer information. The company needs to give customers the ability to complete purchase transactions on the website. The company also needs to ensure that sensitive customer data is protected, even from database administrators. Which solution meets these requirements?

- A. Store sensitive data in an Amazon Elastic Block Store (Amazon EBS) volume. Use EBS encryption to encrypt the data. Use an IAM instance role to restrict access.
- B. Store sensitive data in Amazon RDS for MySQL. Use AWS Key Management Service (AWS KMS) client-side encryption to encrypt the data.
- C. Store sensitive data in Amazon S3. Use AWS Key Management Service (AWS KMS) server-side encryption to encrypt the data. Use S3 bucket policies to restrict access.
- D. Store sensitive data in Amazon FSx for Windows Server. Mount the file share on application servers. Use Windows file permissions to restrict access.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Client-side encryption ensures that the data is encrypted before it reaches the database, so even database administrators cannot see plaintext data (they only see encrypted data). Server-side encryption (A, C, D) protects data at rest but the database service (or administrators) may have access to decrypted data. (AWS Docs: Client-Side Encryption).
</details>

---

**Q565.** A company has an on-premises MySQL database that handles transactional data. The company is migrating the database to the AWS Cloud. The migrated database must maintain compatibility with the company's applications that use the database. The migrated database also must scale automatically during periods of increased demand. Which migration solution will meet these requirements?

- A. Use native MySQL tools to migrate the database to Amazon RDS for MySQL. Configure elastic storage scaling.
- B. Migrate the database to Amazon Redshift by using the mysqldump utility. Turn on Auto Scaling for the Amazon Redshift cluster.
- C. Use AWS Database Migration Service (AWS DMS) to migrate the database to Amazon Aurora. Turn on Aurora Auto Scaling.
- D. Use AWS Database Migration Service (AWS DMS) to migrate the database to Amazon DynamoDB. Configure an Auto Scaling policy.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Aurora is MySQL-compatible and supports Aurora Auto Scaling for read replicas (and serverless for compute scaling). Option A (RDS) does not auto-scale compute. Option B (Redshift) is not MySQL-compatible. Option D (DynamoDB) is not MySQL-compatible. (AWS Docs: Aurora Auto Scaling).
</details>

---

**Q566.** A company runs multiple Amazon EC2 Linux instances in a VPC across two Availability Zones. The instances host applications that use a hierarchical directory structure. The applications need to read and write rapidly and concurrently to shared storage. What should a solutions architect do to meet these requirements?

- A. Create an Amazon S3 bucket. Allow access from all the EC2 instances in the VPC.
- B. Create an Amazon Elastic File System (Amazon EFS) file system. Mount the EFS file system from each EC2 instance.
- C. Create a file system on a Provisioned IOPS SSD (io2) Amazon Elastic Block Store (Amazon EBS) volume. Attach the EBS volume to all the EC2 instances.
- D. Create file systems on Amazon Elastic Block Store (Amazon EBS) volumes that are attached to each EC2 instance. Synchronize the EBS volumes across the different EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* EFS provides a shared, POSIX-compliant file system that can be mounted by multiple Linux instances concurrently, with high throughput. S3 (A) is not a file system. EBS (C) cannot be attached to multiple instances (Multi-Attach has limitations and is not for general-purpose file systems). Option D (synchronization) is complex. (AWS Docs: EFS for Concurrent Access).
</details>

---

**Q567.** A solutions architect is designing a workload that will store hourly energy consumption by business tenants in a building. The sensors will feed a database through HTTP requests that will add up usage for each tenant. The solutions architect must use managed services when possible. The workload will receive more features in the future as the solutions architect adds independent components. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon API Gateway with AWS Lambda functions to receive the data from the sensors, process the data, and store the data in an Amazon DynamoDB table.
- B. Use an Elastic Load Balancer that is supported by an Auto Scaling group of Amazon EC2 instances to receive and process the data from the sensors. Use an Amazon S3 bucket to store the processed data.
- C. Use Amazon API Gateway with AWS Lambda functions to receive the data from the sensors, process the data, and store the data in a Microsoft SQL Server Express database on an Amazon EC2 instance.
- D. Use an Elastic Load Balancer that is supported by an Auto Scaling group of Amazon EC2 instances to receive and process the data from the sensors. Use an Amazon Elastic File System (Amazon EFS) shared file system to store the processed data.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Serverless (API Gateway + Lambda + DynamoDB) uses managed services, minimizes operational overhead, and allows adding independent components easily. Options B and D use EC2 (more overhead). Option C uses EC2 and SQL Express (not managed). (AWS Docs: Serverless Data Ingestion).
</details>

---

**Q568.** A solutions architect is designing the storage architecture for a new web application used for storing and viewing engineering drawings. All application components will be deployed on the AWS infrastructure. The application design must support caching to minimize the amount of time that users wait for the engineering drawings to load. The application must be able to store petabytes of data. Which combination of storage and caching should the solutions architect use?

- A. Amazon S3 with Amazon CloudFront
- B. Amazon S3 Glacier with Amazon ElastiCache
- C. Amazon Elastic Block Store (Amazon EBS) volumes with Amazon CloudFront
- D. AWS Storage Gateway with Amazon ElastiCache

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 can store petabytes of data. CloudFront caches drawings at edge locations for fast loading. Glacier (B) has retrieval delays. EBS (C) is limited in scale. Storage Gateway (D) is for hybrid. (AWS Docs: S3 + CloudFront for Large Objects).
</details>

---

**Q569.** An Amazon EventBridge rule targets a third-party API. The third-party API has not received any incoming traffic. A solutions architect needs to determine whether the rule conditions are being met and if the rule's target is being invoked. Which solution will meet these requirements?

- A. Check for metrics in Amazon CloudWatch in the namespace for AWS/Events.
- B. Review events in the Amazon Simple Queue Service (Amazon SQS) dead-letter queue.
- C. Check for the events in Amazon CloudWatch Logs.
- D. Check the trails in AWS CloudTrail for the EventBridge events.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* EventBridge publishes metrics to CloudWatch (namespace AWS/Events) including `Invocations`, `FailedInvocations`, etc. This is the first place to check. Dead-letter queue (B) would show failed invocations but not whether conditions were met. CloudWatch Logs (C) requires logging configured. CloudTrail (D) is for API calls. (AWS Docs: EventBridge Metrics).
</details>

---

**Q570.** A company has a large workload that runs every Friday evening. The workload runs on Amazon EC2 instances that are in two Availability Zones in the us-east-1 Region. Normally, the company must run no more than two instances at all times. However, the company wants to scale up to six instances each Friday to handle a regularly repeating increased workload. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create a reminder in Amazon EventBridge to scale the instances.
- B. Create an Auto Scaling group that has a scheduled action.
- C. Create an Auto Scaling group that uses manual scaling.
- D. Create an Auto Scaling group that uses automatic scaling.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Scheduled scaling in an Auto Scaling group can increase desired capacity to 6 on Friday evenings and reduce back to 2 afterwards. This is the least overhead. Reminder (A) is not actionable. Manual scaling (C) requires manual intervention. Automatic scaling (D) based on metrics may not react in time for predictable weekly load. (AWS Docs: Scheduled Scaling).
</details>

---

**Q571.** A company is creating a REST API. The company has strict requirements for the use of TLS. The company requires TLSv1.3 on the API endpoints. The company also requires a specific public third-party certificate authority (CA) to sign the TLS certificate. Which solution will meet these requirements?

- A. Use a local machine to create a certificate that is signed by the third-party CA. Import the certificate into AWS Certificate Manager (ACM). Create an HTTP API in Amazon API Gateway with a custom domain. Configure the custom domain to use the certificate.
- B. Create a certificate in AWS Certificate Manager (ACM) that is signed by the third-party CA. Create an HTTP API in Amazon API Gateway with a custom domain. Configure the custom domain to use the certificate.
- C. Use AWS Certificate Manager (ACM) to create a certificate that is signed by the third-party CA. Import the certificate into AWS Certificate Manager (ACM). Create an AWS Lambda function with a Lambda function URL. Configure the Lambda function URL to use the certificate.
- D. Create a certificate in AWS Certificate Manager (ACM) that is signed by the third-party CA. Create an AWS Lambda function with a Lambda function URL. Configure the Lambda function URL to use the certificate.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* ACM cannot issue certificates signed by an external CA; you must import them. Import the externally signed certificate into ACM. Then create an API Gateway custom domain with that certificate. API Gateway supports TLSv1.3. Lambda function URLs (C, D) have limited TLS configuration. (AWS Docs: Importing Certificates into ACM, API Gateway Custom Domain).
</details>

---

**Q572.** A company runs an application on AWS. The application receives inconsistent amounts of usage. The application uses AWS Direct Connect to connect to an on-premises MySQL-compatible database. The on-premises database consistently uses a minimum of 2 GiB of memory. The company wants to migrate the on-premises database to a managed AWS service. The company wants to use auto scaling capabilities to manage unexpected workload increases. Which solution will meet these requirements with the LEAST administrative overhead?

- A. Provision an Amazon DynamoDB database with default read and write capacity settings.
- B. Provision an Amazon Aurora database with a minimum capacity of 1 Aurora capacity unit (ACU).
- C. Provision an Amazon Aurora Serverless v2 database with a minimum capacity of 1 Aurora capacity unit (ACU).
- D. Provision an Amazon RDS for MySQL database with 2 GiB of memory.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Aurora Serverless v2 automatically scales compute capacity based on workload, with a minimum of 1 ACU (approx 2 GiB memory). This meets the requirement with minimal overhead. DynamoDB (A) is not MySQL-compatible. Aurora provisioned (B) requires capacity planning. RDS (D) does not auto-scale compute. (AWS Docs: Aurora Serverless v2).
</details>

---

**Q573.** A company wants to use an event-driven programming model with AWS Lambda. The company wants to reduce startup latency for Lambda functions that run on Java 11. The company does not have strict latency requirements for the applications. The company wants to reduce cold starts and outlier latencies when a function scales up. Which solution will meet these requirements MOST cost-effectively?

- A. Configure Lambda provisioned concurrency.
- B. Increase the timeout of the Lambda functions.
- C. Increase the memory of the Lambda functions.
- D. Configure Lambda SnapStart.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Lambda SnapStart for Java functions significantly reduces cold start latency (often <200ms) by taking a snapshot of the initialized function. It is more cost-effective than provisioned concurrency (A) for reducing cold starts without paying for always-on capacity. Increasing timeout (B) or memory (C) does not directly reduce cold starts. (AWS Docs: Lambda SnapStart).
</details>

---

**Q574.** A financial services company launched a new application that uses an Amazon RDS for MySQL database. The company uses the application to track stock market trends. The company needs to operate the application for only 2 hours at the end of each week. The company needs to optimize the cost of running the database. Which solution will meet these requirements MOST cost-effectively?

- A. Migrate the existing RDS for MySQL database to an Aurora Serverless v2 MySQL database cluster.
- B. Migrate the existing RDS for MySQL database to an Aurora MySQL database cluster.
- C. Migrate the existing RDS for MySQL database to an Amazon EC2 instance that runs MySQL. Purchase an instance reservation for the EC2 instance.
- D. Migrate the existing RDS for MySQL database to an Amazon Elastic Container Service (Amazon ECS) cluster that uses MySQL container images to run tasks.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Aurora Serverless v2 can scale to zero (or minimal capacity) when not in use, and automatically scale during the 2-hour weekly run. This is most cost-effective for intermittent use. Provisioned Aurora (B) and RDS (A original) incur continuous costs. EC2 (C) and ECS (D) have fixed costs. (AWS Docs: Aurora Serverless for Intermittent Workloads).
</details>

---

**Q575.** A company deploys its applications on Amazon Elastic Kubernetes Service (Amazon EKS) behind an Application Load Balancer in an AWS Region. The application needs to store data in a PostgreSQL database engine. The company wants the data in the database to be highly available. The company also needs increased capacity for read workloads. Which solution will meet these requirements with the MOST operational efficiency?

- A. Create an Amazon DynamoDB database table configured with global tables.
- B. Create an Amazon RDS database with Multi-AZ deployments.
- C. Create an Amazon RDS database with Multi-AZ DB cluster deployment.
- D. Create an Amazon RDS database configured with cross-Region read replicas.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* RDS Multi-AZ DB cluster deployment (for PostgreSQL) provides high availability and two readable standby instances for read workload scaling, with operational efficiency. Multi-AZ instance (B) provides HA but no readable standby. Cross-Region replicas (D) add latency. DynamoDB (A) is not PostgreSQL. (AWS Docs: RDS Multi-AZ DB Cluster).
</details>

---

**Q576.** A company is building a RESTful serverless web application on AWS by using Amazon API Gateway and AWS Lambda. The users of this web application will be geographically distributed, and the company wants to reduce the latency of API requests to these users. Which type of endpoint should a solutions architect use to meet these requirements?

- A. Private endpoint
- B. Regional endpoint
- C. Interface VPC endpoint
- D. Edge-optimized endpoint

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Edge-optimized API Gateway endpoints use CloudFront edge locations to reduce latency for geographically distributed users. Regional endpoints (B) are for users in the same Region. Private (A) and interface VPC (C) endpoints are for internal use. (AWS Docs: API Gateway Edge-Optimized).
</details>

---

**Q577.** A company uses an Amazon CloudFront distribution to serve content pages for its website. The company needs to ensure that clients use a TLS certificate when accessing the company's website. The company wants to automate the creation and renewal of the TLS certificates. Which solution will meet these requirements with the MOST operational efficiency?

- A. Use a CloudFront security policy to create a certificate.
- B. Use a CloudFront origin access control (OAC) to create a certificate.
- C. Use AWS Certificate Manager (ACM) to create a certificate. Use DNS validation for the domain.
- D. Use AWS Certificate Manager (ACM) to create a certificate. Use email validation for the domain.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* ACM with DNS validation allows automatic certificate renewal, requiring no manual intervention. Email validation (D) requires responding to emails annually. CloudFront security policy (A) and OAC (B) do not create certificates. (AWS Docs: ACM DNS Validation).
</details>

---

**Q578.** A company deployed a serverless application that uses Amazon DynamoDB as a database layer. The application has experienced a large increase in users. The company wants to improve database response time from milliseconds to microseconds and to cache requests to the database. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use DynamoDB Accelerator (DAX).
- B. Migrate the database to Amazon Redshift.
- C. Migrate the database to Amazon RDS.
- D. Use Amazon ElastiCache for Redis.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* DAX is a fully managed, in-memory cache for DynamoDB that reduces response time to microseconds with minimal operational overhead. ElastiCache (D) would require application changes. Redshift (B) and RDS (C) are not suitable for sub-millisecond latency. (AWS Docs: DAX).
</details>

---

**Q579.** A company runs an application that uses Amazon RDS for PostgreSQL. The application receives traffic only on weekdays during business hours. The company wants to optimize costs and reduce operational overhead based on this usage. Which solution will meet these requirements?

- A. Use the Instance Scheduler on AWS to configure start and stop schedules.
- B. Turn off automatic backups. Create weekly manual snapshots of the database.
- C. Create a custom AWS Lambda function to start and stop the database based on minimum CPU utilization.
- D. Purchase All Upfront reserved DB instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS Instance Scheduler (or custom Lambda with EventBridge) can start/stop RDS instances on a schedule to match business hours, reducing costs. Option B (disable backups) is risky. Option C (based on CPU) may not align with schedule. Option D (All Upfront) is for 24/7 workloads. (AWS Docs: Start/Stop RDS on Schedule).
</details>

---

**Q580.** A company uses locally attached storage to run a latency-sensitive application on premises. The company is using a lift and shift method to move the application to the AWS Cloud. The company does not want to change the application architecture. Which solution will meet these requirements MOST cost-effectively?

- A. Configure an Auto Scaling group with an Amazon EC2 instance. Use an Amazon FSx for Lustre file system to run the application.
- B. Host the application on an Amazon EC2 instance. Use an Amazon Elastic Block Store (Amazon EBS) GP2 volume to run the application.
- C. Configure an Auto Scaling group with an Amazon EC2 instance. Use an Amazon FSx for OpenZFS file system to run the application.
- D. Host the application on an Amazon EC2 instance. Use an Amazon Elastic Block Store (Amazon EBS) GP3 volume to run the application.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* GP3 is the latest generation SSD volume, cost-effective and offers baseline performance suitable for many applications. It can be attached to a single EC2 instance (lift and shift). FSx (A, C) is overkill for a single instance. GP2 (B) is older and less cost-effective than GP3. (AWS Docs: gp3 Volumes).
</details>

---

**Q581.** A company runs a stateful production application on Amazon EC2 instances. The application requires at least two EC2 instances to always be running. A solutions architect needs to design a highly available and fault-tolerant architecture for the application. The solutions architect creates an Auto Scaling group of EC2 instances. Which set of additional steps should the solutions architect take to meet these requirements?

- A. Set the Auto Scaling group's minimum capacity to two. Deploy one On-Demand Instance in one Availability Zone and one On-Demand Instance in a second Availability Zone.
- B. Set the Auto Scaling group's minimum capacity to four. Deploy two On-Demand Instances in one Availability Zone and two On-Demand Instances in a second Availability Zone.
- C. Set the Auto Scaling group's minimum capacity to two. Deploy four Spot Instances in one Availability Zone.
- D. Set the Auto Scaling group's minimum capacity to four. Deploy two On-Demand Instances in one Availability Zone and two Spot Instances in a second Availability Zone.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* For HA, distribute instances across at least 2 AZs. Minimum capacity 2 ensures at least 2 instances running. On-Demand ensures they are not interrupted (stateful app). Option B (4 instances) is unnecessary. Option C (single AZ, Spot) not HA/fault-tolerant. Option D (Spot) risky. (AWS Docs: Auto Scaling Multi-AZ HA).
</details>

---

**Q582.** An ecommerce company uses Amazon Route 53 as its DNS provider. The company hosts its website on premises and in the AWS Cloud. The company's on-premises data center is near the us-west-1 Region. The company uses the eu-central-1 Region to host the website. The company wants to minimize load time for the website as much as possible. Which solution will meet these requirements?

- A. Set up a geolocation routing policy. Send the traffic that is near us-west-1 to the on-premises data center. Send the traffic that is near eu-central-1 to eu-central-1.
- B. Set up a simple routing policy that routes all traffic that is near eu-central-1 to eu-central-1 and routes all traffic that is near the on-premises datacenter to the on-premises data center.
- C. Set up a latency routing policy. Associate the policy with us-west-1.
- D. Set up a weighted routing policy. Split the traffic evenly between eu-central-1 and the on-premises data center.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Geolocation routing policy routes users based on their geographic location to the endpoint that is physically closest, minimizing latency. Latency routing (C) measures actual latency, but geolocation is simpler and effective when endpoints are in distinct locations. Simple (B) doesn't consider location. Weighted (D) ignores latency. (AWS Docs: Route 53 Geolocation).
</details>

---

**Q583.** A company has 5 PB of archived data on physical tapes. The company needs to preserve the data on the tapes for another 10 years for compliance purposes. The company wants to migrate to AWS in the next 6 months. The data center that stores the tapes has a 1 Gbps uplink internet connectivity. Which solution will meet these requirements MOST cost-effectively?

- A. Read the data from the tapes on premises. Stage the data in a local NFS storage. Use AWS DataSync to migrate the data to Amazon S3 Glacier Flexible Retrieval.
- B. Use an on-premises backup application to read the data from the tapes and to write directly to Amazon S3 Glacier Deep Archive.
- C. Order multiple AWS Snowball devices that have Tape Gateway. Copy the physical tapes to virtual tapes in Snowball. Ship the Snowball devices to AWS. Create a lifecycle policy to move the tapes to Amazon S3 Glacier Deep Archive.
- D. Configure an on-premises Tape Gateway. Create virtual tapes in the AWS Cloud. Use backup software to copy the physical tape to the virtual tape.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Tape Gateway allows you to replace physical tapes with virtual tapes stored in S3/Glacier. You can migrate physical tapes to virtual tapes using your existing backup software, then store them in Glacier Deep Archive for low-cost 10-year retention. DataSync (A) would require reading all 5 PB over 1 Gbps (would take years). Snowball (C) would require many devices. (AWS Docs: Tape Gateway for Tape Migration).
</details>

---

**Q584.** A company is deploying an application that processes large quantities of data in parallel. The company plans to use Amazon EC2 instances for the workload. The network architecture must be configurable to prevent groups of nodes from sharing the same underlying hardware. Which networking solution meets these requirements?

- A. Run the EC2 instances in a spread placement group.
- B. Group the EC2 instances in separate accounts.
- C. Configure the EC2 instances with dedicated tenancy.
- D. Configure the EC2 instances with shared tenancy.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Spread placement groups place each instance on distinct underlying hardware, preventing sharing. Dedicated tenancy (C) ensures instances run on dedicated hardware but multiple instances can still share a host. Spread is the correct solution for this requirement. (AWS Docs: Spread Placement Groups).
</details>

---

**Q585.** A solutions architect is designing a disaster recovery (DR) strategy to provide Amazon EC2 capacity in a failover AWS Region. Business requirements state that the DR strategy must meet capacity in the failover Region. Which solution will meet these requirements?

- A. Purchase On-Demand Instances in the failover Region.
- B. Purchase an EC2 Savings Plan in the failover Region.
- C. Purchase regional Reserved Instances in the failover Region.
- D. Purchase a Capacity Reservation in the failover Region.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* On-Demand Capacity Reservations guarantee capacity in a specific AZ/Region, even if you don't have instances running. This meets the requirement to have capacity available during failover. Reserved Instances (C) provide a discount but do not guarantee capacity unless you also create a Capacity Reservation. Savings Plan (B) is a discount mechanism. (AWS Docs: Capacity Reservations).
</details>

---

**Q586.** A company has five organizational units (OUs) as part of its organization in AWS Organizations. Each OU correlates to the five businesses that the company owns. The company's research and development (R&D) business is separating from the company and will need its own organization. A solutions architect creates a separate new management account for this purpose. What should the solutions architect do next in the new management account?

- A. Have the R&D AWS account be part of both organizations during the transition.
- B. Invite the R&D AWS account to be part of the new organization after the R&D AWS account has left the prior organization.
- C. Create a new R&D AWS account in the new organization. Migrate resources from the prior R&D AWS account to the new R&D AWS account.
- D. Have the R&D AWS account join the new organization. Make the new management account a member of the prior organization.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* An AWS account can only be a member of one organization at a time. You must first remove the R&D account from the original organization, then invite it to the new organization. Option C (create new account) is unnecessary if you can migrate. Option A (part of both) is not possible. Option D (new management account as member) is incorrect. (AWS Docs: Removing Accounts from Organizations).
</details>

---

**Q587.** A company is designing a solution to capture customer activity in different web applications to process analytics and make predictions. Customer activity in the web applications is unpredictable and can increase suddenly. The company requires a solution that integrates with other web applications. The solution must include an authorization step for security purposes. Which solution will meet these requirements?

- A. Configure a Gateway Load Balancer (GWLB) in front of an Amazon Elastic Container Service (Amazon ECS) container instance that stores the information that the company receives in an Amazon Elastic File System (Amazon EFS) file system. Authorization is resolved at the GWLB.
- B. Configure an Amazon API Gateway endpoint in front of an Amazon Kinesis data stream that stores the information that the company receives in an Amazon S3 bucket. Use an AWS Lambda function to resolve authorization.
- C. Configure an Amazon API Gateway endpoint in front of an Amazon Kinesis Data Firehose that stores the information that the company receives in an Amazon S3 bucket. Use an API Gateway Lambda authorizer to resolve authorization.
- D. Configure a Gateway Load Balancer (GWLB) in front of an Amazon Elastic Container Service (Amazon ECS) container instance that stores the information that the company receives on an Amazon Elastic File System (Amazon EFS) file system. Use an AWS Lambda function to resolve authorization.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* API Gateway provides built-in Lambda authorizers for authorization. Kinesis Data Firehose can ingest streaming data and deliver to S3. This is serverless and scalable. GWLB (A, D) is for traffic inspection, not API endpoints. Kinesis Data Stream (B) adds complexity. (AWS Docs: API Gateway Lambda Authorizer, Firehose).
</details>

---

**Q588.** An ecommerce company wants a disaster recovery solution for its Amazon RDS DB instances that run Microsoft SQL Server Enterprise Edition. The company's current recovery point objective (RPO) and recovery time objective (RTO) are 24 hours. Which solution will meet these requirements MOST cost-effectively?

- A. Create a cross-Region read replica and promote the read replica to the primary instance.
- B. Use AWS Database Migration Service (AWS DMS) to create RDS cross-Region replication.
- C. Use cross-Region replication every 24 hours to copy native backups to an Amazon S3 bucket.
- D. Copy automatic snapshots to another Region every 24 hours.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Copying automated snapshots to another Region every 24 hours meets RPO=24h and RTO within 24h (restore from snapshot). This is cost-effective (only storage and cross-Region copy charges). Read replica (A) is more expensive. DMS (B) adds cost. Native backups to S3 (C) requires additional configuration. (AWS Docs: RDS Snapshot Copy for DR).
</details>

---

**Q589.** A company runs a web application on Amazon EC2 instances in an Auto Scaling group behind an Application Load Balancer that has sticky sessions enabled. The web server currently hosts the user session state. The company wants to ensure high availability and avoid user session state loss in the event of a web server outage. Which solution will meet these requirements?

- A. Use an Amazon ElastiCache for Memcached instance to store the session data. Update the application to use ElastiCache for Memcached to store the session state.
- B. Use Amazon ElastiCache for Redis to store the session state. Update the application to use ElastiCache for Redis to store the session state.
- C. Use an AWS Storage Gateway cached volume to store session data. Update the application to use AWS Storage Gateway cached volume to store the session state.
- D. Use Amazon RDS to store the session state. Update the application to use Amazon RDS to store the session state.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* ElastiCache for Redis supports persistence (AOF) and high availability (Multi-AZ), making it suitable for session state storage. Memcached (A) does not support persistence. Storage Gateway (C) is for hybrid. RDS (D) adds latency. (AWS Docs: ElastiCache for Redis Session Storage).
</details>

---

**Q590.** A company migrated a MySQL database from the company's on-premises data center to an Amazon RDS for MySQL DB instance. The company sized the RDS DB instance to meet the company's average daily workload. Once a month, the database performs slowly when the company runs queries for a report. The company wants to have the ability to run reports and maintain the performance of the daily workloads. Which solution will meet these requirements?

- A. Create a read replica of the database. Direct the queries to the read replica.
- B. Create a backup of the database. Restore the backup to another DB instance. Direct the queries to the new database.
- C. Export the data to Amazon S3. Use Amazon Athena to query the S3 bucket.
- D. Resize the DB instance to accommodate the additional workload.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* A read replica offloads the monthly reporting queries from the primary instance, maintaining performance for daily workloads. Option B (backup/restore) is not real-time. Option C (Athena) requires data export. Option D (resize) is more expensive and doesn't isolate workloads. (AWS Docs: RDS Read Replicas for Reporting).
</details>

---

**Q591.** A company runs a container application by using Amazon Elastic Kubernetes Service (Amazon EKS). The application includes microservices that manage customers and place orders. The company needs to route incoming requests to the appropriate microservices. Which solution will meet this requirement MOST cost-effectively?

- A. Use the AWS Load Balancer Controller to provision a Network Load Balancer.
- B. Use the AWS Load Balancer Controller to provision an Application Load Balancer.
- C. Use an AWS Lambda function to connect the requests to Amazon EKS.
- D. Use Amazon API Gateway to connect the requests to Amazon EKS.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* The AWS Load Balancer Controller can provision an ALB for EKS services, providing path-based routing to microservices. This is cost-effective and standard. NLB (A) is for TCP/UDP. Lambda (C) and API Gateway (D) add complexity and cost. (AWS Docs: ALB Ingress Controller for EKS).
</details>

---

**Q592.** A company uses AWS and sells access to copyrighted images. The company's global customer base needs to be able to access these images quickly. The company must deny access to users from specific countries. The company wants to minimize costs as much as possible. Which solution will meet these requirements?

- A. Use Amazon S3 to store the images. Turn on multi-factor authentication (MFA) and public bucket access. Provide customers with a link to the S3 bucket.
- B. Use Amazon S3 to store the images. Create an IAM user for each customer. Add the users to a group that has permission to access the S3 bucket.
- C. Use Amazon EC2 instances that are behind Application Load Balancers (ALBs) to store the images. Deploy the instances only in the countries the company services. Provide customers with links to the ALBs for their specific country's instances.
- D. Use Amazon S3 to store the images. Use Amazon CloudFront to distribute the images with geographic restrictions. Provide a signed URL for each customer to access the data in CloudFront.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* CloudFront geographic restrictions block users from specific countries. Signed URLs provide access control. S3 for storage is cost-effective. This minimizes costs while meeting requirements. Option A (public S3) is insecure. Option B (IAM users per customer) doesn't scale. Option C (EC2) is more expensive. (AWS Docs: CloudFront Geo Restriction + Signed URLs).
</details>

---

**Q593.** A solutions architect is designing a highly available Amazon ElastiCache for Redis based solution. The solutions architect needs to ensure that failures do not result in performance degradation or loss of data locally and within an AWS Region. The solution needs to provide high availability at the node level and at the Region level. Which solution will meet these requirements?

- A. Use Multi-AZ Redis replication groups with shards that contain multiple nodes.
- B. Use Redis shards that contain multiple nodes with Redis append only files (AOF) turned on.
- C. Use a Multi-AZ Redis cluster with more than one read replica in the replication group.
- D. Use Redis shards that contain multiple nodes with Auto Scaling turned on.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Multi-AZ Redis replication groups with shards (cluster mode enabled) provide node-level HA (replica nodes) and Region-level HA (Multi-AZ). AOF (B) provides persistence but not HA. Multi-AZ with one replica (C) provides HA but not sharding for performance. Auto Scaling (D) is for scaling, not HA. (AWS Docs: ElastiCache for Redis Cluster Mode).
</details>

---

**Q594.** A company plans to migrate to AWS and use Amazon EC2 On-Demand Instances for its application. During the migration testing phase, a technical team observes that the application takes a long time to launch and load memory to become fully productive. Which solution will reduce the launch time of the application during the next testing phase?

- A. Launch two or more EC2 On-Demand Instances. Turn on auto scaling features and make the EC2 On-Demand Instances available during the next testing phase.
- B. Launch EC2 Spot Instances to support the application and to scale the application so it is available during the next testing phase.
- C. Launch the EC2 On-Demand Instances with hibernation turned on. Configure EC2 Auto Scaling warm pools during the next testing phase.
- D. Launch EC2 On-Demand Instances with Capacity Reservations. Start additional EC2 instances during the next testing phase.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Hibernation saves the memory state to EBS, allowing instances to resume quickly. Warm pools in Auto Scaling keep pre-initialized instances ready to serve traffic, reducing launch time. Option A (more instances) doesn't reduce launch time. Option B (Spot) may be interrupted. Option D (Capacity Reservations) doesn't reduce launch time. (AWS Docs: EC2 Hibernation, Auto Scaling Warm Pools).
</details>

---

**Q595.** A company's applications run on Amazon EC2 instances in Auto Scaling groups. The company notices that its applications experience sudden traffic increases on random days of the week. The company wants to maintain application performance during sudden traffic increases. Which solution will meet these requirements MOST cost-effectively?

- A. Use manual scaling to change the size of the Auto Scaling group.
- B. Use predictive scaling to change the size of the Auto Scaling group.
- C. Use dynamic scaling to change the size of the Auto Scaling group.
- D. Use schedule scaling to change the size of the Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Dynamic scaling (e.g., target tracking or step scaling) automatically adjusts capacity based on real-time metrics (CPU, network), handling sudden, unpredictable traffic spikes. Predictive scaling (B) is for predictable patterns. Scheduled scaling (D) is for known schedules. Manual (A) is not automatic. (AWS Docs: Dynamic Scaling).
</details>

---

**Q596.** An ecommerce application uses a PostgreSQL database that runs on an Amazon EC2 instance. During a monthly sales event, database usage increases and causes database connection issues for the application. The traffic is unpredictable for subsequent monthly sales events, which impacts the sales forecast. The company needs to maintain performance when there is an unpredictable increase in traffic. Which solution resolves this issue in the MOST cost-effective way?

- A. Migrate the PostgreSQL database to Amazon Aurora Serverless v2.
- B. Enable auto scaling for the PostgreSQL database on the EC2 instance to accommodate increased usage.
- C. Migrate the PostgreSQL database to Amazon RDS for PostgreSQL with a larger instance type.
- D. Migrate the PostgreSQL database to Amazon Redshift to accommodate increased usage.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Aurora Serverless v2 automatically scales compute capacity based on workload, ideal for unpredictable traffic. EC2 (B) cannot auto-scale database compute easily. RDS (C) requires provisioning for peak. Redshift (D) is for analytics, not OLTP. (AWS Docs: Aurora Serverless for Unpredictable Traffic).
</details>

---

**Q597.** A company hosts an internal serverless application on AWS by using Amazon API Gateway and AWS Lambda. The company's employees report issues with high latency when they begin using the application each day. The company wants to reduce latency. Which solution will meet these requirements?

- A. Increase the API Gateway throttling limit.
- B. Set up a scheduled scaling to increase Lambda provisioned concurrency before employees begin to use the application each day.
- C. Create an Amazon CloudWatch alarm to initiate a Lambda function as a target for the alarm at the beginning of each day.
- D. Increase the Lambda function memory.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Provisioned concurrency keeps Lambda functions initialized, eliminating cold start latency. Scheduled scaling can increase provisioned concurrency before employees start work. Increasing memory (D) may help but doesn't eliminate cold starts. API Gateway throttling (A) doesn't reduce latency. (AWS Docs: Lambda Provisioned Concurrency).
</details>

---

**Q598.** A research company uses on-premises devices to generate data for analysis. The company wants to use the AWS Cloud to analyze the data. The devices generate .csv files and support writing the data to an SMB file share. Company analysts must be able to use SQL commands to query the data. The analysts will run queries periodically throughout the day. Which combination of steps will meet these requirements MOST cost-effectively? (Choose three.)

- A. Deploy an AWS Storage Gateway on premises in Amazon S3 File Gateway mode.
- B. Deploy an AWS Storage Gateway on premises in Amazon FSx File Gateway mode.
- C. Set up an AWS Glue crawler to create a table based on the data that is in Amazon S3.
- D. Set up an Amazon EMR cluster with EMR File System (EMRFS) to query the data that is in Amazon S3. Provide access to analysts.
- E. Set up an Amazon Redshift cluster to query the data that is in Amazon S3. Provide access to analysts.
- F. Setup Amazon Athena to query the data that is in Amazon S3. Provide access to analysts.

<details>
<summary>Show Answer</summary>

**Answer: A, C, F**

*Explanation:* S3 File Gateway (A) allows on-premises devices to write CSV files to S3. Glue crawler (C) catalogs the data. Athena (F) provides serverless SQL querying on S3, cost-effective for periodic queries. FSx File Gateway (B) is for FSx. EMR (D) and Redshift (E) have higher overhead. (AWS Docs: S3 File Gateway, Athena).
</details>

---

**Q599.** A company wants to use Amazon Elastic Container Service (Amazon ECS) clusters and Amazon RDS DB instances to build and run a payment processing application. The company will run the application in its on-premises data center for compliance purposes. A solutions architect wants to use AWS Outposts as part of the solution. The solutions architect is working with the company's operational team to build the application. Which activities are the responsibility of the company's operational team? (Choose three.)

- A. Providing resilient power and network connectivity to the Outposts racks
- B. Managing the virtualization hypervisor, storage systems, and the AWS services that run on Outposts
- C. Physical security and access controls of the data center environment
- D. Availability of the Outposts infrastructure including the power supplies, servers, and networking equipment within the Outposts racks
- E. Physical maintenance of Outposts components
- F. Providing extra capacity for Amazon ECS clusters to mitigate server failures and maintenance events

<details>
<summary>Show Answer</summary>

**Answer: A, C, D**

*Explanation:* Customer is responsible for power/network to the rack (A), physical security of data center (C), and availability of Outposts infrastructure (D). AWS manages the hypervisor, storage systems, and AWS services (B). Physical maintenance of Outposts components (E) is AWS responsibility. Extra capacity for ECS (F) is application-level responsibility. (AWS Docs: Outposts Shared Responsibility).
</details>

---

**Q600.** A company is planning to migrate a TCP-based application into the company's VPC. The application is publicly accessible on a nonstandard TCP port through a hardware appliance in the company's data center. This public endpoint can process up to 3 million requests per second with low latency. The company requires the same level of performance for the new public endpoint in AWS. What should a solutions architect recommend to meet this requirement?

- A. Deploy a Network Load Balancer (NLB). Configure the NLB to be publicly accessible over the TCP port that the application requires.
- B. Deploy an Application Load Balancer (ALB). Configure the ALB to be publicly accessible over the TCP port that the application requires.
- C. Deploy an Amazon CloudFront distribution that listens on the TCP port that the application requires. Use an Application Load Balancer as the origin.
- D. Deploy an Amazon API Gateway API that is configured with the TCP port that the application requires. Configure AWS Lambda functions with provisioned concurrency to process the requests.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* NLB handles TCP traffic at very high throughput (millions of requests per second) with low latency. ALB (B) is for HTTP/HTTPS. CloudFront (C) is for HTTP/HTTPS caching. API Gateway (D) is for HTTP and has lower throughput limits. (AWS Docs: NLB Performance).
</details>

---

**Q601.** A company runs its critical database on an Amazon RDS for PostgreSQL DB instance. The company wants to migrate to Amazon Aurora PostgreSQL with minimal downtime and data loss. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create a DB snapshot of the RDS for PostgreSQL DB instance to populate a new Aurora PostgreSQL DB cluster.
- B. Create an Aurora read replica of the RDS for PostgreSQL DB instance. Promote the Aurora read replica to a new Aurora PostgreSQL DB cluster.
- C. Use data import from Amazon S3 to migrate the database to an Aurora PostgreSQL DB cluster.
- D. Use the pg_dump utility to back up the RDS for PostgreSQL database. Restore the backup to a new Aurora PostgreSQL DB cluster.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Creating an Aurora read replica from an RDS for PostgreSQL instance performs a one-time data copy and then uses native replication to keep it in sync. You can then cut over with minimal downtime. Option A (snapshot) has downtime. Option C (S3 import) requires manual steps. Option D (pg_dump) has downtime. (AWS Docs: RDS to Aurora Replica).
</details>

---

**Q602.** A company's infrastructure consists of hundreds of Amazon EC2 instances that use Amazon Elastic Block Store (Amazon EBS) storage. A solutions architect must ensure that every EC2 instance can be recovered after a disaster. What should the solutions architect do to meet this requirement with the LEAST amount of effort?

- A. Take a snapshot of the EBS storage that is attached to each EC2 instance. Create an AWS CloudFormation template to launch new EC2 instances from the EBS storage.
- B. Take a snapshot of the EBS storage that is attached to each EC2 instance. Use AWS Elastic Beanstalk to set the environment based on the EC2 template and attach the EBS storage.
- C. Use AWS Backup to set up a backup plan for the entire group of EC2 instances. Use the AWS Backup API or the AWS CLI to speed up the restore process for multiple EC2 instances.
- D. Create an AWS Lambda function to take a snapshot of the EBS storage that is attached to each EC2 instance and copy the Amazon Machine Images (AMIs). Create another Lambda function to perform the restores with the copied AMIs and attach the EBS storage.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* AWS Backup provides a centralized backup plan for EC2 instances (including EBS) with minimal effort. It can handle hundreds of instances. Option A (manual snapshots + CloudFormation) is more effort. Option B (Elastic Beanstalk) is not for this. Option D (custom Lambda) is more effort. (AWS Docs: AWS Backup for EC2).
</details>

---

**Q603.** A company recently migrated to the AWS Cloud. The company wants a serverless solution for large-scale parallel on-demand processing of a semistructured dataset. The data consists of logs, media files, sales transactions, and IoT sensor data that is stored in Amazon S3. The company wants the solution to process thousands of items in the dataset in parallel. Which solution will meet these requirements with the MOST operational efficiency?

- A. Use the AWS Step Functions Map state in Inline mode to process the data in parallel.
- B. Use the AWS Step Functions Map state in Distributed mode to process the data in parallel.
- C. Use AWS Glue to process the data in parallel.
- D. Use several AWS Lambda functions to process the data in parallel.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Step Functions Distributed Map state can process large datasets (thousands of items) in parallel, with high concurrency and is serverless. Inline mode (A) is for small datasets. Glue (C) is for ETL, not arbitrary processing. Lambda (D) would require managing concurrency and fan-out. (AWS Docs: Step Functions Distributed Map).
</details>

---

**Q604.** A company will migrate 10 PB of data to Amazon S3 in 6 weeks. The current data center has a 500 Mbps uplink to the internet. Other on-premises applications share the uplink. The company can use 80% of the internet bandwidth for this one-time migration task. Which solution will meet these requirements?

- A. Configure AWS DataSync to migrate the data to Amazon S3 and to automatically verify the data.
- B. Use rsync to transfer the data directly to Amazon S3.
- C. Use the AWS CLI and multiple copy processes to send the data directly to Amazon S3.
- D. Order multiple AWS Snowball devices. Copy the data to the devices. Send the devices to AWS to copy the data to Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* 10 PB over 500 Mbps at 80% utilization would take ~10 PB * 8 / (500 * 0.8 Mbps) = 80,000,000 / 400 = 200,000 seconds? Actually 10 PB = 10,000,000 GB = 80,000,000 Gb. 400 Mbps = 0.4 Gbps. 80,000,000 / 0.4 = 200,000,000 seconds = ~6.3 years, exceeding 6 weeks. Snowball devices are required for such large data volume. (AWS Docs: Snowball for Petabyte Scale).
</details>

---

**Q605.** A company has several on-premises Internet Small Computer Systems Interface (iSCSI) network storage servers. The company wants to reduce the number of these servers by moving to the AWS Cloud. A solutions architect must provide low-latency access to frequently used data and reduce the dependency on on-premises servers with a minimal number of infrastructure changes. Which solution will meet these requirements?

- A. Deploy an Amazon S3 File Gateway.
- B. Deploy Amazon Elastic Block Store (Amazon EBS) storage with backups to Amazon S3.
- C. Deploy an AWS Storage Gateway volume gateway that is configured with stored volumes.
- D. Deploy an AWS Storage Gateway volume gateway that is configured with cached volumes.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Cached volumes present iSCSI targets and store frequently accessed data locally (low latency), while full data is stored in S3. This reduces on-premises storage dependency. Stored volumes (C) would require as much on-premises storage. S3 File Gateway (A) is for file shares, not iSCSI. (AWS Docs: Cached Volumes).
</details>

---

**Q606.** A solutions architect is designing an application that will allow business users to upload objects to Amazon S3. The solution needs to maximize object durability. Objects also must be readily available at any time and for any length of time. Users will access objects frequently within the first 30 days after the objects are uploaded, but users are much less likely to access objects that are older than 30 days. Which solution meets these requirements MOST cost-effectively?

- A. Store all the objects in S3 Standard with an S3 Lifecycle rule to transition the objects to S3 Glacier after 30 days.
- B. Store all the objects in S3 Standard with an S3 Lifecycle rule to transition the objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days.
- C. Store all the objects in S3 Standard with an S3 Lifecycle rule to transition the objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 30 days.
- D. Store all the objects in S3 Intelligent-Tiering with an S3 Lifecycle rule to transition the objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 Standard for first 30 days (frequent access). Transition to S3 Standard-IA after 30 days (infrequent access, but still immediately available and durable). Option A (Glacier) has retrieval delays. Option C (One Zone-IA) is not durable (single AZ). Option D (Intelligent-Tiering) has monitoring costs. (AWS Docs: S3 Lifecycle to Standard-IA).
</details>

---

**Q607.** A company has migrated a two-tier application from its on-premises data center to the AWS Cloud. The data tier is a Multi-AZ deployment of Amazon RDS for Oracle with 12 TB of General Purpose SSD Amazon Elastic Block Store (Amazon EBS) storage. The application is designed to process and store documents in the database as binary large objects (blobs) with an average document size of 6 MB. The database size has grown over time, reducing the performance and increasing the cost of storage. The company must improve the database performance and needs a solution that is highly available and resilient. Which solution will meet these requirements MOST cost-effectively?

- A. Reduce the RDS DB instance size. Increase the storage capacity to 24 TiB. Change the storage type to Magnetic.
- B. Increase the RDS DB instance size. Increase the storage capacity to 24 TiB. Change the storage type to Provisioned IOPS.
- C. Create an Amazon S3 bucket. Update the application to store documents in the S3 bucket. Store the object metadata in the existing database.
- D. Create an Amazon DynamoDB table. Update the application to use DynamoDB. Use AWS Database Migration Service (AWS DMS) to migrate data from the Oracle database to DynamoDB.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Storing large blobs (6 MB) in an RDS database is inefficient and expensive. Moving the documents to S3 (cost-effective, scalable) and storing only metadata in the database improves performance and reduces costs. Option A/B (changing RDS storage) don't address the root issue. Option D (DynamoDB) is a major rearchitecture. (AWS Docs: S3 for Blob Storage with RDS Metadata).
</details>

---

**Q608.** A company has an application that serves clients that are deployed in more than 20,000 retail storefront locations around the world. The application consists of backend web services that are exposed over HTTPS on port 443. The application is hosted on Amazon EC2 instances behind an Application Load Balancer (ALB). The retail locations communicate with the web application over the public internet. The company allows each retail location to register the IP address that the retail location has been allocated by its local ISP. The company's security team recommends to increase the security of the application endpoint by restricting access to only the IP addresses registered by the retail locations. What should a solutions architect do to meet these requirements?

- A. Associate an AWS WAF web ACL with the ALB. Use IP rule sets on the ALB to filter traffic. Update the IP addresses in the rule to include the registered IP addresses.
- B. Deploy AWS Firewall Manager to manage the ALB. Configure firewall rules to restrict traffic to the ALB. Modify the firewall rules to include the registered IP addresses.
- C. Store the IP addresses in an Amazon DynamoDB table. Configure an AWS Lambda authorization function on the ALB to validate that incoming requests are from the registered IP addresses.
- D. Configure the network ACL on the subnet that contains the public interface of the ALB. Update the ingress rules on the network ACL with entries for each of the registered IP addresses.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* AWS WAF can be associated with an ALB and use IP set rules to allow only specific IP addresses. This is scalable (20,000 IPs). Firewall Manager (B) is for central management, but WAF is still needed. Lambda authorization (C) is complex. NACL (D) has limits (max 20 rules per direction, 20,000 IPs would exceed). (AWS Docs: WAF IP Sets).
</details>

---

**Q609.** A company is building a data analysis platform on AWS by using AWS Lake Formation. The platform will ingest data from different sources such as Amazon S3 and Amazon RDS. The company needs a secure solution to prevent access to portions of the data that contain sensitive information. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an IAM role that includes permissions to access Lake Formation tables.
- B. Create data filters to implement row-level security and cell-level security.
- C. Create an AWS Lambda function that removes sensitive information before Lake Formation ingests the data.
- D. Create an AWS Lambda function that periodically queries and removes sensitive information from Lake Formation tables.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Lake Formation supports data filters for row-level and cell-level security, allowing you to restrict access to sensitive data without transforming the data. This is the least overhead. Option A (IAM role) is for table access, not fine-grained. Options C and D (Lambda) require custom code and data transformation. (AWS Docs: Lake Formation Data Filters).
</details>

---

**Q610.** A company deploys Amazon EC2 instances that run in a VPC. The EC2 instances load source data into Amazon S3 buckets so that the data can be processed in the future. According to compliance laws, the data must not be transmitted over the public internet. Servers in the company's on-premises data center will consume the output from an application that runs on the EC2 instances. Which solution will meet these requirements?

- A. Deploy an interface VPC endpoint for Amazon EC2. Create an AWS Site-to-Site VPN connection between the company and the VPC.
- B. Deploy a gateway VPC endpoint for Amazon S3. Set up an AWS Direct Connect connection between the on-premises network and the VPC.
- C. Set up an AWS Transit Gateway connection from the VPC to the S3 buckets. Create an AWS Site-to-Site VPN connection between the company and the VPC.
- D. Set up proxy EC2 instances that have routes to NAT gateways. Configure the proxy EC2 instances to fetch S3 data and feed the application instances.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Gateway VPC endpoint for S3 keeps EC2-to-S3 traffic private. Direct Connect provides private connectivity from on-premises to VPC for consuming the output. Option A (EC2 endpoint) is not needed. Option C (Transit Gateway to S3) is not valid. Option D (proxy) adds complexity and uses internet. (AWS Docs: S3 Gateway Endpoint, Direct Connect).
</details>

---

**Q611.** A company has an application with a REST-based interface that allows data to be received in near-real time from a third-party vendor. Once received, the application processes and stores the data for further analysis. The application is running on Amazon EC2 instances. The third-party vendor has received many 503 Service Unavailable Errors when sending data to the application. When the data volume spikes, the compute capacity reaches its maximum limit and the application is unable to process all requests. Which design should a solutions architect recommend to provide a more scalable solution?

- A. Use Amazon Kinesis Data Streams to ingest the data. Process the data using AWS Lambda functions.
- B. Use Amazon API Gateway on top of the existing application. Create a usage plan with a quota limit for the third-party vendor.
- C. Use Amazon Simple Notification Service (Amazon SNS) to ingest the data. Put the EC2 instances in an Auto Scaling group behind an Application Load Balancer.
- D. Repackage the application as a container. Deploy the application using Amazon Elastic Container Service (Amazon ECS) using the EC2 launch type with an Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Kinesis Data Streams can buffer incoming data spikes, and Lambda can process the data asynchronously, providing scalability. This is serverless and cost-effective. Option B (API Gateway + existing app) doesn't solve the processing bottleneck. Option C (SNS to EC2) still has EC2 scaling lag. Option D (ECS on EC2) has more overhead than Lambda. (AWS Docs: Kinesis + Lambda for Spiky Traffic).
</details>

---

**Q612.** A company has an application that runs on Amazon EC2 instances in a private subnet. The application needs to process sensitive information from an Amazon S3 bucket. The application must not use the internet to connect to the S3 bucket. Which solution will meet these requirements?

- A. Configure an internet gateway. Update the S3 bucket policy to allow access from the internet gateway. Update the application to use the new internet gateway.
- B. Configure a VPN connection. Update the S3 bucket policy to allow access from the VPN connection. Update the application to use the new VPN connection.
- C. Configure a NAT gateway. Update the S3 bucket policy to allow access from the NAT gateway. Update the application to use the new NAT gateway.
- D. Configure a VPC endpoint. Update the S3 bucket policy to allow access from the VPC endpoint. Update the application to use the new VPC endpoint.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* A VPC endpoint (gateway endpoint) for S3 provides private connectivity without internet. Update the bucket policy to allow access from the VPC endpoint. Options A, B, C all involve internet or VPN (which may traverse internet). (AWS Docs: S3 VPC Endpoint).
</details>

---

**Q613.** A company uses Amazon Elastic Kubernetes Service (Amazon EKS) to run a container application. The EKS cluster stores sensitive information in the Kubernetes secrets object. The company wants to ensure that the information is encrypted. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use the container application to encrypt the information by using AWS Key Management Service (AWS KMS).
- B. Enable secrets encryption in the EKS cluster by using AWS Key Management Service (AWS KMS).
- C. Implement an AWS Lambda function to encrypt the information by using AWS Key Management Service (AWS KMS).
- D. Use AWS Systems Manager Parameter Store to encrypt the information by using AWS Key Management Service (AWS KMS).

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* EKS has built-in support for envelope encryption of Kubernetes secrets using AWS KMS. Enabling this feature encrypts secrets in etcd with minimal operational overhead. Option A (application-level) requires code changes. Option C (Lambda) adds complexity. Option D (Parameter Store) is an external secret store. (AWS Docs: EKS Secrets Encryption).
</details>

---

**Q614.** A company is designing a new multi-tier web application that consists of the following components: • Web and application servers that run on Amazon EC2 instances as part of Auto Scaling groups • An Amazon RDS DB instance for data storage. A solutions architect needs to limit access to the application servers so that only the web servers can access them. Which solution will meet these requirements?

- A. Deploy AWS PrivateLink in front of the application servers. Configure the network ACL to allow only the web servers to access the application servers.
- B. Deploy a VPC endpoint in front of the application servers. Configure the security group to allow only the web servers to access the application servers.
- C. Deploy a Network Load Balancer with a target group that contains the application servers' Auto Scaling group. Configure the network ACL to allow only the web servers to access the application servers.
- D. Deploy an Application Load Balancer with a target group that contains the application servers' Auto Scaling group. Configure the security group to allow only the web servers to access the application servers.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* An ALB can route traffic from web servers to application servers. Security groups can be configured to allow traffic only from the ALB (or web server security group) to the application servers. PrivateLink (A) is for service endpoints. VPC endpoint (B) is for AWS services. NLB (C) can work, but ALB is more common for HTTP. (AWS Docs: ALB Security Groups).
</details>

---

**Q615.** A company runs a critical, customer-facing application on Amazon Elastic Kubernetes Service (Amazon EKS). The application has a microservices architecture. The company needs to implement a solution that collects, aggregates, and summarizes metrics and logs from the application in a centralized location. Which solution meets these requirements?

- A. Run the Amazon CloudWatch agent in the existing EKS cluster. View the metrics and logs in the CloudWatch console.
- B. Run AWS App Mesh in the existing EKS cluster. View the metrics and logs in the App Mesh console.
- C. Configure AWS CloudTrail to capture data events. Query CloudTrail by using Amazon OpenSearch Service.
- D. Configure Amazon CloudWatch Container Insights in the existing EKS cluster. View the metrics and logs in the CloudWatch console.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from EKS clusters, with minimal setup. CloudWatch agent (A) is part of Container Insights but the feature is Container Insights. App Mesh (B) is for service mesh. CloudTrail (C) is for API auditing. (AWS Docs: Container Insights for EKS).
</details>

---

**Q616.** A company has deployed its newest product on AWS. The product runs in an Auto Scaling group behind a Network Load Balancer. The company stores the product's objects in an Amazon S3 bucket. The company recently experienced malicious attacks against its systems. The company needs a solution that continuously monitors for malicious activity in the AWS account, workloads, and access patterns to the S3 bucket. The solution must also report suspicious activity and display the information on a dashboard. Which solution will meet these requirements?

- A. Configure Amazon Macie to monitor and report findings to AWS Config.
- B. Configure Amazon Inspector to monitor and report findings to AWS CloudTrail.
- C. Configure Amazon GuardDuty to monitor and report findings to AWS Security Hub.
- D. Configure AWS Config to monitor and report findings to Amazon EventBridge.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* GuardDuty monitors for malicious activity (including S3 access patterns). Findings are sent to Security Hub, which provides a centralized dashboard. Macie (A) is for data discovery. Inspector (B) is for vulnerability scanning. Config (D) is for compliance. (AWS Docs: GuardDuty with Security Hub).
</details>

---

**Q617.** A company wants to migrate an on-premises data center to AWS. The data center hosts a storage server that stores data in an NFS-based file system. The storage server holds 200 GB of data. The company needs to migrate the data without interruption to existing services. Multiple resources in AWS must be able to access the data by using the NFS protocol. Which combination of steps will meet these requirements MOST cost-effectively? (Choose two.)

- A. Create an Amazon FSx for Lustre file system.
- B. Create an Amazon Elastic File System (Amazon EFS) file system.
- C. Create an Amazon S3 bucket to receive the data.
- D. Manually use an operating system copy command to push the data into the AWS destination.
- E. Install an AWS DataSync agent in the on-premises data center. Use a DataSync task between the on-premises location and AWS.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* EFS (B) provides an NFS file system for AWS resources. DataSync (E) with an agent can migrate the 200 GB without interrupting services. Option A (FSx for Lustre) is overkill. Option C (S3) is not NFS. Option D (manual copy) may cause interruption. (AWS Docs: DataSync to EFS).
</details>

---

**Q618.** A company wants to use Amazon FSx for Windows File Server for its Amazon EC2 instances that have an SMB file share mounted as a volume in the us-east-1 Region. The company has a recovery point objective (RPO) of 5 minutes for planned system maintenance or unplanned service disruptions. The company needs to replicate the file system to the us-west-2 Region. The replicated data must not be deleted by any user for 5 years. Which solution will meet these requirements?

- A. Create an FSx for Windows File Server file system in us-east-1 that has a Single-AZ 2 deployment type. Use AWS Backup to create a daily backup plan that includes a backup rule that copies the backup to us-west-2. Configure AWS Backup Vault Lock in compliance mode for a target vault in us-west-2. Configure a minimum duration of 5 years.
- B. Create an FSx for Windows File Server file system in us-east-1 that has a Multi-AZ deployment type. Use AWS Backup to create a daily backup plan that includes a backup rule that copies the backup to us-west-2. Configure AWS Backup Vault Lock in governance mode for a target vault in us-west-2. Configure a minimum duration of 5 years.
- C. Create an FSx for Windows File Server file system in us-east-1 that has a Multi-AZ deployment type. Use AWS Backup to create a daily backup plan that includes a backup rule that copies the backup to us-west-2. Configure AWS Backup Vault Lock in compliance mode for a target vault in us-west-2. Configure a minimum duration of 5 years.
- D. Create an FSx for Windows File Server file system in us-east-1 that has a Single-AZ 2 deployment type. Use AWS Backup to create a daily backup plan that includes a backup rule that copies the backup to us-west-2. Configure AWS Backup Vault Lock in governance mode for a target vault in us-west-2. Configure a minimum duration of 5 years.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Multi-AZ (C) provides high availability. Compliance mode Vault Lock ensures no user can delete the backups for 5 years. Governance mode (B, D) can be bypassed by authorized users. Daily backup meets RPO of 5 minutes? Daily backup is 24 hours, not 5 minutes. However, the question likely expects Multi-AZ + Compliance. (Note: RPO of 5 minutes would require continuous replication, not daily backup. But among options, C is best for immutability and HA.)
</details>

---

**Q619.** A solutions architect is designing a security solution for a company that wants to provide developers with individual AWS accounts through AWS Organizations, while also maintaining standard security controls. Because the individual developers will have AWS account root user-level access to their own accounts, the solutions architect wants to ensure that the mandatory AWS CloudTrail configuration that is applied to new developer accounts is not modified. Which action meets these requirements?

- A. Create an IAM policy that prohibits changes to CloudTrail, and attach it to the root user.
- B. Create a new trail in CloudTrail from within the developer accounts with the organization trails option enabled.
- C. Create a service control policy (SCP) that prohibits changes to CloudTrail, and attach it to the developer accounts.
- D. Create a service-linked role for CloudTrail with a policy condition that allows changes only from an Amazon Resource Name (ARN) in the management account.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* SCPs can deny actions even to the root user of member accounts. An SCP denying CloudTrail modification actions (e.g., `cloudtrail:UpdateTrail`) attached to the developer accounts prevents changes. Option A (IAM policy) does not affect root user. Option B (organization trail) is good but doesn't prevent modification. Option D (service-linked role) is not for this. (AWS Docs: SCPs for CloudTrail Protection).
</details>

---

**Q620.** A company is planning to deploy a business-critical application in the AWS Cloud. The application requires durable storage with consistent, low-latency performance. Which type of storage should a solutions architect recommend to meet these requirements?

- A. Instance store volume
- B. Amazon ElastiCache for Memcached cluster
- C. Provisioned IOPS SSD Amazon Elastic Block Store (Amazon EBS) volume
- D. Throughput Optimized HDD Amazon Elastic Block Store (Amazon EBS) volume

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Provisioned IOPS SSD (io1/io2) provides consistent, low-latency performance and is durable (persistent). Instance store (A) is ephemeral. ElastiCache (B) is in-memory. HDD (D) is for throughput, not low latency. (AWS Docs: EBS io2 Volumes).
</details>

---

**Q621.** An online photo-sharing company stores its photos in an Amazon S3 bucket that exists in the us-west-1 Region. The company needs to store a copy of all new photos in the us-east-1 Region. Which solution will meet this requirement with the LEAST operational effort?

- A. Create a second S3 bucket in us-east-1. Use S3 Cross-Region Replication to copy photos from the existing S3 bucket to the second S3 bucket.
- B. Create a cross-origin resource sharing (CORS) configuration of the existing S3 bucket. Specify us-east-1 in the CORS rule's AllowedOrigin element.
- C. Create a second S3 bucket in us-east-1 across multiple Availability Zones. Create an S3 Lifecycle rule to save photos into the second S3 bucket.
- D. Create a second S3 bucket in us-east-1. Configure S3 event notifications on object creation and update events to invoke an AWS Lambda function to copy photos from the existing S3 bucket to the second S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Cross-Region Replication (CRR) is the native, managed way to replicate objects to another Region with minimal operational effort. Lambda (D) would require custom code. CORS (B) is for browser security. Lifecycle (C) is for transitions, not replication. (AWS Docs: S3 CRR).
</details>

---

**Q622.** A company is creating a new web application for its subscribers. The application will consist of a static single page and a persistent database layer. The application will have millions of users for 4 hours in the morning, but the application will have only a few thousand users during the rest of the day. The company's data architects have requested the ability to rapidly evolve their schema. Which solutions will meet these requirements and provide the MOST scalability? (Choose two.)

- A. Deploy Amazon DynamoDB as the database solution. Provision on-demand capacity.
- B. Deploy Amazon Aurora as the database solution. Choose the serverless DB engine mode.
- C. Deploy Amazon DynamoDB as the database solution. Ensure that DynamoDB auto scaling is enabled.
- D. Deploy the static content into an Amazon S3 bucket. Provision an Amazon CloudFront distribution with the S3 bucket as the origin.
- E. Deploy the web servers for static content across a fleet of Amazon EC2 instances in Auto Scaling groups. Configure the instances to periodically refresh the content from an Amazon Elastic File System (Amazon EFS) volume.

<details>
<summary>Show Answer</summary>

**Answer: A, D**

*Explanation:* S3 + CloudFront (D) for static content (scalable, low latency). DynamoDB on-demand (A) automatically scales for unpredictable traffic patterns (millions of users for 4 hours). Aurora Serverless (B) is also an option but DynamoDB is simpler for schema evolution? DynamoDB is schema-less. Auto scaling (C) is for provisioned mode, which may lag behind sudden spikes. Option E (EC2) has overhead. (AWS Docs: DynamoDB On-Demand, S3 + CloudFront).
</details>

---

**Q623.** A company uses Amazon API Gateway to manage its REST APIs that third-party service providers access. The company must protect the REST APIs from SQL injection and cross-site scripting attacks. What is the MOST operationally efficient solution that meets these requirements?

- A. Configure AWS Shield.
- B. Configure AWS WAF.
- C. Set up API Gateway with an Amazon CloudFront distribution. Configure AWS Shield in CloudFront.
- D. Set up API Gateway with an Amazon CloudFront distribution. Configure AWS WAF in CloudFront.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS WAF can be directly associated with API Gateway to protect against SQL injection and XSS, with no need for CloudFront. This is the most operationally efficient. Shield (A, C) is for DDoS. Option D adds unnecessary CloudFront. (AWS Docs: WAF with API Gateway).
</details>

---

**Q624.** A company wants to provide users with access to AWS resources. The company has 1,500 users and manages their access to on-premises resources through Active Directory user groups on the corporate network. However, the company does not want users to have to maintain another identity to access the resources. A solutions architect must manage user access to the AWS resources while preserving access to the on-premises resources. What should the solutions architect do to meet these requirements?

- A. Create an IAM user for each user in the company. Attach the appropriate policies to each user.
- B. Use Amazon Cognito with an Active Directory user pool. Create roles with the appropriate policies attached.
- C. Define cross-account roles with the appropriate policies attached. Map the roles to the Active Directory groups.
- D. Configure Security Assertion Markup Language (SAML) 2.0-based federation. Create roles with the appropriate policies attached. Map the roles to the Active Directory groups.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* SAML 2.0 federation allows users to authenticate against on-premises Active Directory and assume IAM roles to access AWS resources, without creating separate IAM users. Option B (Cognito) is for customer identity. Option C (cross-account roles) is for different AWS accounts. Option A (IAM users) requires separate identities. (AWS Docs: SAML Federation with AD).
</details>

---

**Q625.** A company is hosting a website behind multiple Application Load Balancers. The company has different distribution rights for its content around the world. A solutions architect needs to ensure that users are served the correct content without violating distribution rights. Which configuration should the solutions architect choose to meet these requirements?

- A. Configure Amazon CloudFront with AWS WAF.
- B. Configure Application Load Balancers with AWS WAF.
- C. Configure Amazon Route 53 with a geolocation policy.
- D. Configure Amazon Route 53 with a geoproximity routing policy.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Route 53 geolocation routing policy routes users based on their geographic location, allowing you to serve different content (or ALBs) based on distribution rights. Geoproximity (D) is for traffic shifting based on distance. CloudFront (A) caches content but doesn't route based on geography. (AWS Docs: Route 53 Geolocation Routing).
</details>

---

**Q626.** A company stores its data on premises. The amount of data is growing beyond the company's available capacity. The company wants to migrate its data from the on-premises location to an Amazon S3 bucket. The company needs a solution that will automatically validate the integrity of the data after the transfer. Which solution will meet these requirements?

- A. Order an AWS Snowball Edge device. Configure the Snowball Edge device to perform the online data transfer to an S3 bucket.
- B. Deploy an AWS DataSync agent on premises. Configure the DataSync agent to perform the online data transfer to an S3 bucket.
- C. Create an Amazon S3 File Gateway on premises. Configure the S3 File Gateway to perform the online data transfer to an S3 bucket.
- D. Configure an accelerator in Amazon S3 Transfer Acceleration on premises. Configure the accelerator to perform the online data transfer to an S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS DataSync automatically verifies data integrity (checksums) during transfer. It can transfer online using an agent. Snowball (A) is offline. File Gateway (C) is for ongoing access, not migration. Transfer Acceleration (D) speeds up transfers but does not automatically validate integrity. (AWS Docs: DataSync Data Validation).
</details>

---

**Q627.** A company wants to migrate two DNS servers to AWS. The servers host a total of approximately 200 zones and receive 1 million requests each day on average. The company wants to maximize availability while minimizing the operational overhead that is related to the management of the two servers. What should a solutions architect recommend to meet these requirements?

- A. Create 200 new hosted zones in the Amazon Route 53 console. Import zone files.
- B. Launch a single large Amazon EC2 instance. Import zone files. Configure Amazon CloudWatch alarms and notifications to alert the company about any downtime.
- C. Migrate the servers to AWS by using AWS Server Migration Service (AWS SMS). Configure Amazon CloudWatch alarms and notifications to alert the company about any downtime.
- D. Launch an Amazon EC2 instance in an Auto Scaling group across two Availability Zones. Import zone files. Set the desired capacity to 1 and the maximum capacity to 3 for the Auto Scaling group. Configure scaling alarms to scale based on CPU utilization.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Amazon Route 53 is a managed DNS service that provides high availability and scalability with zero operational overhead for managing servers. Creating hosted zones and importing zone files meets the requirement. EC2-based solutions (B, C, D) require server management. (AWS Docs: Route 53).
</details>

---

**Q628.** A global company runs its applications in multiple AWS accounts in AWS Organizations. The company's applications use multipart uploads to upload data to multiple Amazon S3 buckets across AWS Regions. The company wants to report on incomplete multipart uploads for cost compliance purposes. Which solution will meet these requirements with the LEAST operational overhead?

- A. Configure AWS Config with a rule to report the incomplete multipart upload object count.
- B. Create a service control policy (SCP) to report the incomplete multipart upload object count.
- C. Configure S3 Storage Lens to report the incomplete multipart upload object count.
- D. Create an S3 Multi-Region Access Point to report the incomplete multipart upload object count.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* S3 Storage Lens provides organization-wide visibility into storage metrics, including incomplete multipart uploads, with minimal operational overhead. AWS Config (A) can detect but not easily report on this metric. SCPs (B) are for permissions. Multi-Region Access Points (D) are for data access. (AWS Docs: S3 Storage Lens Metrics).
</details>

---

**Q629.** A company runs a production database on Amazon RDS for MySQL. The company wants to upgrade the database version for security compliance reasons. Because the database contains critical data, the company wants a quick solution to upgrade and test functionality without losing any data. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an RDS manual snapshot. Upgrade to the new version of Amazon RDS for MySQL.
- B. Use native backup and restore. Restore the data to the upgraded new version of Amazon RDS for MySQL.
- C. Use AWS Database Migration Service (AWS DMS) to replicate the data to the upgraded new version of Amazon RDS for MySQL.
- D. Use Amazon RDS Blue/Green Deployments to deploy and test production changes.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* RDS Blue/Green Deployments create a staging environment that is fully synchronized with production, allowing you to upgrade the database version in the staging environment, test, and then switch over with minimal downtime. This is the least operational overhead for this scenario. Option A (manual snapshot + upgrade) has downtime. Option B (native backup) has downtime. Option C (DMS) is more complex. (AWS Docs: RDS Blue/Green Deployments).
</details>

---

**Q630.** A solutions architect is creating a data processing job that runs once daily and can take up to 2 hours to complete. If the job is interrupted, it has to restart from the beginning. How should the solutions architect address this issue in the MOST cost-effective manner?

- A. Create a script that runs locally on an Amazon EC2 Reserved Instance that is triggered by a cron job.
- B. Create an AWS Lambda function triggered by an Amazon EventBridge scheduled event.
- C. Use an Amazon Elastic Container Service (Amazon ECS) Fargate task triggered by an Amazon EventBridge scheduled event.
- D. Use an Amazon Elastic Container Service (Amazon ECS) task running on Amazon EC2 triggered by an Amazon EventBridge scheduled event.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* ECS Fargate task can run up to 2 hours, is serverless (pay per use), and can be scheduled with EventBridge. Lambda (B) has 15-minute timeout. EC2 Reserved (A) and ECS on EC2 (D) have fixed costs even when idle. Fargate is most cost-effective for a 2-hour daily job. (AWS Docs: ECS Fargate Scheduled Tasks).
</details>

---

**Q631.** A social media company wants to store its database of user profiles, relationships, and interactions in the AWS Cloud. The company needs an application to monitor any changes in the database. The application needs to analyze the relationships between the data entities and to provide recommendations to users. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon Neptune to store the information. Use Amazon Kinesis Data Streams to process changes in the database.
- B. Use Amazon Neptune to store the information. Use Neptune Streams to process changes in the database.
- C. Use Amazon Quantum Ledger Database (Amazon QLDB) to store the information. Use Amazon Kinesis Data Streams to process changes in the database.
- D. Use Amazon Quantum Ledger Database (Amazon QLDB) to store the information. Use Neptune Streams to process changes in the database.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Neptune is a graph database ideal for relationships. Neptune Streams provides a change log of graph data, enabling real-time processing for recommendations. QLDB (C, D) is a ledger database, not for graph relationships. Kinesis (A) adds complexity. (AWS Docs: Neptune Streams).
</details>

---

**Q632.** A company is creating a new application that will store a large amount of data. The data will be analyzed hourly and will be modified by several Amazon EC2 Linux instances that are deployed across multiple Availability Zones. The needed amount of storage space will continue to grow for the next 6 months. Which storage solution should a solutions architect recommend to meet these requirements?

- A. Store the data in Amazon S3 Glacier. Update the S3 Glacier vault policy to allow access to the application instances.
- B. Store the data in an Amazon Elastic Block Store (Amazon EBS) volume. Mount the EBS volume on the application instances.
- C. Store the data in an Amazon Elastic File System (Amazon EFS) file system. Mount the file system on the application instances.
- D. Store the data in an Amazon Elastic Block Store (Amazon EBS) Provisioned IOPS volume shared between the application instances.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* EFS is a shared, scalable file system that can be mounted by multiple Linux instances across AZs, and it grows automatically. EBS (B, D) cannot be shared across instances (Multi-Attach has limitations). Glacier (A) is for archival, not active processing. (AWS Docs: EFS).
</details>

---

**Q633.** A company manages an application that stores data on an Amazon RDS for PostgreSQL Multi-AZ DB instance. Increases in traffic are causing performance problems. The company determines that database queries are the primary reason for the slow performance. What should a solutions architect do to improve the application's performance?

- A. Serve read traffic from the Multi-AZ standby replica.
- B. Configure the DB instance to use Transfer Acceleration.
- C. Create a read replica from the source DB instance. Serve read traffic from the read replica.
- D. Use Amazon Kinesis Data Firehose between the application and Amazon RDS to increase the concurrency of database requests.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* A read replica offloads read queries from the primary instance, improving performance. Multi-AZ standby (A) is not readable (in PostgreSQL, Multi-AZ standby is not directly readable without additional configuration). Transfer Acceleration (B) is for S3. Kinesis (D) is for streaming. (AWS Docs: RDS Read Replicas).
</details>

---

**Q634.** A company collects 10 GB of telemetry data daily from various machines. The company stores the data in an Amazon S3 bucket in a source data account. The company has hired several consulting agencies to use this data for analysis. Each agency needs read access to the data for its analysts. The company must share the data from the source data account by choosing a solution that maximizes security and operational efficiency. Which solution will meet these requirements?

- A. Configure S3 global tables to replicate data for each agency.
- B. Make the S3 bucket public for a limited time. Inform only the agencies.
- C. Configure cross-account access for the S3 bucket to the accounts that the agencies own.
- D. Set up an IAM user for each analyst in the source data account. Grant each user access to the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Cross-account access using IAM roles (or bucket policies) allows each agency to access the data from their own AWS accounts, which is secure and operationally efficient. Option B (public) is insecure. Option D (IAM users in source account) gives agencies access to your account. Option A (S3 global tables) is for replication, not sharing. (AWS Docs: Cross-Account S3 Access).
</details>

---

**Q635.** A company uses Amazon FSx for NetApp ONTAP in its primary AWS Region for CIFS and NFS file shares. Applications that run on Amazon EC2 instances access the file shares. The company needs a storage disaster recovery (DR) solution in a secondary Region. The data that is replicated in the secondary Region needs to be accessed by using the same protocols as the primary Region. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an AWS Lambda function to copy the data to an Amazon S3 bucket. Replicate the S3 bucket to the secondary Region.
- B. Create a backup of the FSx for ONTAP volumes by using AWS Backup. Copy the volumes to the secondary Region. Create a new FSx for ONTAP instance from the backup.
- C. Create an FSx for ONTAP instance in the secondary Region. Use NetApp SnapMirror to replicate data from the primary Region to the secondary Region.
- D. Create an Amazon Elastic File System (Amazon EFS) volume. Migrate the current data to the volume. Replicate the volume to the secondary Region.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* FSx for ONTAP natively supports NetApp SnapMirror for cross-Region replication, providing DR with the same protocols (CIFS/NFS). This is the least operational overhead. AWS Backup (B) can backup/restore but SnapMirror is native. Option A (Lambda to S3) is complex. Option D (EFS) is different protocol. (AWS Docs: FSx for ONTAP SnapMirror).
</details>

---

**Q636.** A development team is creating an event-based application that uses AWS Lambda functions. Events will be generated when files are added to an Amazon S3 bucket. The development team currently has Amazon Simple Notification Service (Amazon SNS) configured as the event target from Amazon S3. What should a solutions architect do to process the events from Amazon S3 in a scalable way?

- A. Create an SNS subscription that processes the event in Amazon Elastic Container Service (Amazon ECS) before the event runs in Lambda.
- B. Create an SNS subscription that processes the event in Amazon Elastic Kubernetes Service (Amazon EKS) before the event runs in Lambda.
- C. Create an SNS subscription that sends the event to Amazon Simple Queue Service (Amazon SQS). Configure the SQS queue to trigger a Lambda function.
- D. Create an SNS subscription that sends the event to AWS Server Migration Service (AWS SMS). Configure the Lambda function to poll from the SMS event.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* S3 -> SNS -> SQS -> Lambda provides a scalable, durable event processing pipeline. SQS decouples and buffers events, allowing Lambda to scale based on queue depth. Direct SNS to Lambda (without SQS) may lose events if Lambda throttles. (AWS Docs: SNS to SQS to Lambda).
</details>

---

**Q637.** A solutions architect is designing a new service behind Amazon API Gateway. The request patterns for the service will be unpredictable and can change suddenly from 0 requests to over 500 per second. The total size of the data that needs to be persisted in a backend database is currently less than 1 GB with unpredictable future growth. Data can be queried using simple key-value requests. Which combination of AWS services would meet these requirements? (Choose two.)

- A. AWS Fargate
- B. AWS Lambda
- C. Amazon DynamoDB
- D. Amazon EC2 Auto Scaling
- E. MySQL-compatible Amazon Aurora

<details>
<summary>Show Answer</summary>

**Answer: B, C**

*Explanation:* Lambda (B) serverless compute scales instantly from 0 to 500+ rps. DynamoDB (C) is a key-value database that scales automatically and is cost-effective for <1 GB. Fargate (A) and EC2 Auto Scaling (D) have higher overhead. Aurora (E) is relational, overkill for key-value. (AWS Docs: API Gateway + Lambda + DynamoDB).
</details>

---

**Q638.** A company collects and shares research data with the company's employees all over the world. The company wants to collect and store the data in an Amazon S3 bucket and process the data in the AWS Cloud. The company will share the data with the company's employees. The company needs a secure solution in the AWS Cloud that minimizes operational overhead. Which solution will meet these requirements?

- A. Use an AWS Lambda function to create an S3 presigned URL. Instruct employees to use the URL.
- B. Create an IAM user for each employee. Create an IAM policy for each employee to allow S3 access. Instruct employees to use the AWS Management Console.
- C. Create an S3 File Gateway. Create a share for uploading and a share for downloading. Allow employees to mount shares on their local computers to use S3 File Gateway.
- D. Configure AWS Transfer Family SFTP endpoints. Select the custom identity provider options. Use AWS Secrets Manager to manage the user credentials. Instruct employees to use Transfer Family.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* AWS Transfer Family provides a managed SFTP endpoint, allowing employees to securely access S3 data using existing SFTP clients. This minimizes operational overhead. Option A (presigned URLs) is for temporary access. Option B (IAM users) requires managing many users. Option C (File Gateway) is for on-premises caching. (AWS Docs: Transfer Family for Employee Access).
</details>

---

**Q639.** A company is building a new furniture inventory application. The company has deployed the application on a fleet of Amazon EC2 instances across multiple Availability Zones. The EC2 instances run behind an Application Load Balancer (ALB) in their VPC. A solutions architect has observed that incoming traffic seems to favor one EC2 instance, resulting in latency for some requests. What should the solutions architect do to resolve this issue?

- A. Disable session affinity (sticky sessions) on the ALB.
- B. Replace the ALB with a Network Load Balancer.
- C. Increase the number of EC2 instances in each Availability Zone.
- D. Adjust the frequency of the health checks on the ALB's target group.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Sticky sessions (session affinity) cause requests from the same client to go to the same instance, which can lead to imbalance. Disabling it allows the ALB to distribute requests evenly. NLB (B) is for TCP/UDP. Increasing instances (C) may not fix imbalance if sticky sessions are enabled. Health check frequency (D) doesn't affect distribution. (AWS Docs: ALB Sticky Sessions).
</details>

---

**Q640.** A company has an application workflow that uses an AWS Lambda function to download and decrypt files from Amazon S3. These files are encrypted using AWS Key Management Service (AWS KMS) keys. A solutions architect needs to design a solution that will ensure the required permissions are set correctly. Which combination of actions accomplish this? (Choose two.)

- A. Attach the kms:decrypt permission to the Lambda function's resource policy.
- B. Grant the decrypt permission for the Lambda IAM role in the KMS key's policy.
- C. Grant the decrypt permission for the Lambda resource policy in the KMS key's policy.
- D. Create a new IAM policy with the kms:decrypt permission and attach the policy to the Lambda function.
- E. Create a new IAM role with the kms:decrypt permission and attach the execution role to the Lambda function.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* Create an IAM execution role for the Lambda function with a policy that includes `kms:Decrypt` (E). Also, the KMS key policy must grant permission to that IAM role to decrypt (B). Resource policy (A, C) is for who can invoke the function. (AWS Docs: Lambda with KMS).
</details>

---

**Q641.** A company wants to monitor its AWS costs for financial review. The cloud operations team is designing an architecture in the AWS Organizations management account to query AWS Cost and Usage Reports for all member accounts. The team must run this query once a month and provide a detailed analysis of the bill. Which solution is the MOST scalable and cost-effective way to meet these requirements?

- A. Enable Cost and Usage Reports in the management account. Deliver reports to Amazon Kinesis. Use Amazon EMR for analysis.
- B. Enable Cost and Usage Reports in the management account. Deliver the reports to Amazon S3. Use Amazon Athena for analysis.
- C. Enable Cost and Usage Reports for member accounts. Deliver the reports to Amazon S3. Use Amazon Redshift for analysis.
- D. Enable Cost and Usage Reports for member accounts. Deliver the reports to Amazon Kinesis. Use Amazon QuickSight for analysis.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Enable CUR in the management account (single place). Deliver to S3. Use Athena (serverless SQL) to query once a month. This is scalable and cost-effective (pay per query). Redshift (C) has fixed costs. Kinesis/EMR (A, D) are overkill for monthly queries. (AWS Docs: Athena for CUR).
</details>

---

**Q642.** A company wants to run a gaming application on Amazon EC2 instances that are part of an Auto Scaling group in the AWS Cloud. The application will transmit data by using UDP packets. The company wants to ensure that the application can scale out and in as traffic increases and decreases. What should a solutions architect do to meet these requirements?

- A. Attach a Network Load Balancer to the Auto Scaling group.
- B. Attach an Application Load Balancer to the Auto Scaling group.
- C. Deploy an Amazon Route 53 record set with a weighted policy to route traffic appropriately.
- D. Deploy a NAT instance that is configured with port forwarding to the EC2 instances in the Auto Scaling group.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* NLB supports UDP traffic and can be attached to an Auto Scaling group. ALB (B) does not support UDP. Route 53 weighted (C) does not provide load balancing for UDP. NAT instance (D) is for outbound internet, not inbound load balancing. (AWS Docs: NLB with UDP).
</details>

---

**Q643.** A company runs several websites on AWS for its different brands. Each website generates tens of gigabytes of web traffic logs each day. A solutions architect needs to design a scalable solution to give the company's developers the ability to analyze traffic patterns across all the company's websites. This analysis by the developers will occur on demand once a week over the course of several months. The solution must support queries with standard SQL. Which solution will meet these requirements MOST cost-effectively?

- A. Store the logs in Amazon S3. Use Amazon Athena for analysis.
- B. Store the logs in Amazon RDS. Use a database client for analysis.
- C. Store the logs in Amazon OpenSearch Service. Use OpenSearch Service for analysis.
- D. Store the logs in an Amazon EMR cluster. Use a supported open-source framework for SQL-based analysis.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 is cost-effective for log storage. Athena is serverless and supports standard SQL, ideal for on-demand weekly queries. RDS (B) is expensive for tens of GB/day. OpenSearch (C) and EMR (D) have ongoing cluster costs. (AWS Docs: Athena for Log Analysis).
</details>

---

**Q644.** An international company has a subdomain for each country that the company operates in. The subdomains are formatted as example.com, country1.example.com, and country2.example.com. The company's workloads are behind an Application Load Balancer. The company wants to encrypt the website data that is in transit. Which combination of steps will meet these requirements? (Choose two.)

- A. Use the AWS Certificate Manager (ACM) console to request a public certificate for the apex top domain example.com and a wildcard certificate for *.example.com.
- B. Use the AWS Certificate Manager (ACM) console to request a private certificate for the apex top domain example.com and a wildcard certificate for *.example.com.
- C. Use the AWS Certificate Manager (ACM) console to request a public and private certificate for the apex top domain example.com.
- D. Validate domain ownership by email address. Switch to DNS validation by adding the required DNS records to the DNS provider.
- E. Validate domain ownership for the domain by adding the required DNS records to the DNS provider.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* Request a public wildcard certificate `*.example.com` to cover all subdomains (A). Validate domain ownership using DNS validation (E) (easier for automation and wildcards). Email validation (D) is less suitable for wildcards. Private certificates (B, C) are for internal use. (AWS Docs: ACM Wildcard Certificates, DNS Validation).
</details>

---

**Q645.** A company is required to use cryptographic keys in its on-premises key manager. The key manager is outside of the AWS Cloud because of regulatory and compliance requirements. The company wants to manage encryption and decryption by using cryptographic keys that are retained outside of the AWS Cloud and that support a variety of external key managers from different vendors. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS CloudHSM key store backed by a CloudHSM cluster.
- B. Use an AWS Key Management Service (AWS KMS) external key store backed by an external key manager.
- C. Use the default AWS Key Management Service (AWS KMS) managed key store.
- D. Use a custom key store backed by an AWS CloudHSM cluster.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* KMS External Key Store (XKS) allows you to use keys from an external key manager (on-premises) with KMS APIs, with support for multiple vendors. This meets the requirement with least overhead. CloudHSM (A, D) keeps keys within AWS. Default KMS (C) keeps keys in AWS. (AWS Docs: KMS External Key Store).
</details>

---

**Q646.** A solutions architect needs to host a high performance computing (HPC) workload in the AWS Cloud. The workload will run on hundreds of Amazon EC2 instances and will require parallel access to a shared file system to enable distributed processing of large datasets. Datasets will be accessed across multiple instances simultaneously. The workload requires access latency within 1 ms. After processing has completed, engineers will need access to the dataset for manual postprocessing. Which solution will meet these requirements?

- A. Use Amazon Elastic File System (Amazon EFS) as a shared file system. Access the dataset from Amazon EFS.
- B. Mount an Amazon S3 bucket to serve as the shared file system. Perform postprocessing directly from the S3 bucket.
- C. Use Amazon FSx for Lustre as a shared file system. Link the file system to an Amazon S3 bucket for postprocessing.
- D. Configure AWS Resource Access Manager to share an Amazon S3 bucket so that it can be mounted to all instances for processing and postprocessing.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* FSx for Lustre provides sub-millisecond latency and high throughput for HPC. It can be linked to S3, allowing data to be stored in S3 for postprocessing. EFS (A) has higher latency. S3 (B, D) is object storage, not a POSIX file system with sub-ms latency. (AWS Docs: FSx for Lustre with S3).
</details>

---

**Q647.** A gaming company is building an application with Voice over IP capabilities. The application will serve traffic to users across the world. The application needs to be highly available with an automated failover across AWS Regions. The company wants to minimize the latency of users without relying on IP address caching on user devices. What should a solutions architect do to meet these requirements?

- A. Use AWS Global Accelerator with health checks.
- B. Use Amazon Route 53 with a geolocation routing policy.
- C. Create an Amazon CloudFront distribution that includes multiple origins.
- D. Create an Application Load Balancer that uses path-based routing.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Global Accelerator provides static IP addresses (no DNS caching issues), routes traffic to optimal Region based on latency, and supports health checks for automated failover. Route 53 geolocation (B) relies on DNS caching. CloudFront (C) is for HTTP(S) caching, not VoIP. ALB (D) is single Region. (AWS Docs: Global Accelerator for VoIP).
</details>

---

**Q648.** A weather forecasting company needs to process hundreds of gigabytes of data with sub-millisecond latency. The company has a high performance computing (HPC) environment in its data center and wants to expand its forecasting capabilities. A solutions architect must identify a highly available cloud storage solution that can handle large amounts of sustained throughput. Files that are stored in the solution should be accessible to thousands of compute instances that will simultaneously access and process the entire dataset. What should the solutions architect do to meet these requirements?

- A. Use Amazon FSx for Lustre scratch file systems.
- B. Use Amazon FSx for Lustre persistent file systems.
- C. Use Amazon Elastic File System (Amazon EFS) with Bursting Throughput mode.
- D. Use Amazon Elastic File System (Amazon EFS) with Provisioned Throughput mode.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* FSx for Lustre persistent file systems provide high throughput, sub-millisecond latency, and are designed for HPC workloads with thousands of instances. Scratch (A) is for temporary data. EFS (C, D) has higher latency and lower throughput for this scale. (AWS Docs: FSx for Lustre Persistent).
</details>

---

**Q649.** An ecommerce company runs a PostgreSQL database on premises. The database stores data by using high IOPS Amazon Elastic Block Store (Amazon EBS) block storage. The daily peak I/O transactions per second do not exceed 15,000 IOPS. The company wants to migrate the database to Amazon RDS for PostgreSQL and provision disk IOPS performance independent of disk storage capacity. Which solution will meet these requirements MOST cost-effectively?

- A. Configure the General Purpose SSD (gp2) EBS volume storage type and provision 15,000 IOPS.
- B. Configure the Provisioned IOPS SSD (io1) EBS volume storage type and provision 15,000 IOPS.
- C. Configure the General Purpose SSD (gp3) EBS volume storage type and provision 15,000 IOPS.
- D. Configure the EBS magnetic volume type to achieve maximum IOPS.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* gp3 allows you to provision IOPS (up to 20,000) independent of storage capacity, and is cheaper than io1. gp2 (A) ties IOPS to capacity (at 15,000 IOPS would require >3.75 TB). io1 (B) is more expensive than gp3 for 15,000 IOPS. Magnetic (D) has poor performance. (AWS Docs: gp3 Volumes).
</details>

---

**Q650.** A company wants to migrate its on-premises Microsoft SQL Server Enterprise edition database to AWS. The company's online application uses the database to process transactions. The data analysis team uses the same production database to run reports for analytical processing. The company wants to reduce operational overhead by moving to managed services wherever possible. Which solution will meet these requirements with the LEAST operational overhead?

- A. Migrate to Amazon RDS for Microsoft SQL Server. Use read replicas for reporting purposes.
- B. Migrate to Microsoft SQL Server on Amazon EC2. Use Always On read replicas for reporting purposes.
- C. Migrate to Amazon DynamoDB. Use DynamoDB on-demand replicas for reporting purposes.
- D. Migrate to Amazon Aurora MySQL. Use Aurora read replicas for reporting purposes.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* RDS for SQL Server is a managed service, reducing operational overhead. Read replicas offload reporting queries. Option B (EC2) is not managed. Option C (DynamoDB) is not SQL Server compatible. Option D (Aurora MySQL) is not SQL Server compatible. (AWS Docs: RDS for SQL Server Read Replicas).
</details>

---

**Q651.** A company stores a large volume of image files in an Amazon S3 bucket. The images need to be readily available for the first 180 days. The images are infrequently accessed for the next 180 days. After 360 days, the images need to be archived but must be available instantly upon request. After 5 years, only auditors can access the images. The auditors must be able to retrieve the images within 12 hours. The images cannot be lost during this process. A developer will use S3 Standard storage for the first 180 days. The developer needs to configure an S3 Lifecycle rule. Which solution will meet these requirements MOST cost-effectively?

- A. Transition the objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 180 days. S3 Glacier Instant Retrieval after 360 days, and S3 Glacier Deep Archive after 5 years.
- B. Transition the objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 180 days. S3 Glacier Flexible Retrieval after 360 days, and S3 Glacier Deep Archive after 5 years.
- C. Transition the objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 180 days, S3 Glacier Instant Retrieval after 360 days, and S3 Glacier Deep Archive after 5 years.
- D. Transition the objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 180 days, S3 Glacier Flexible Retrieval after 360 days, and S3 Glacier Deep Archive after 5 years.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Days 180-360: Standard-IA (cheaper than Standard, durable). Days 360-5 years: Glacier Instant Retrieval (millisecond retrieval). After 5 years: Deep Archive (cheapest, 12-hour retrieval). Option A (One Zone-IA) is not durable. Option B/D (Flexible Retrieval) has minutes to hours retrieval, not "instantly". (AWS Docs: S3 Lifecycle Transitions).
</details>

---

**Q652.** A company has a large data workload that runs for 6 hours each day. The company cannot lose any data while the process is running. A solutions architect is designing an Amazon EMR cluster configuration to support this critical data workload. Which solution will meet these requirements MOST cost-effectively?

- A. Configure a long-running cluster that runs the primary node and core nodes on On-Demand Instances and the task nodes on Spot Instances.
- B. Configure a transient cluster that runs the primary node and core nodes on On-Demand Instances and the task nodes on Spot Instances.
- C. Configure a transient cluster that runs the primary node on an On-Demand Instance and the core nodes and task nodes on Spot Instances.
- D. Configure a long-running cluster that runs the primary node on an On-Demand Instance, the core nodes on Spot Instances, and the task nodes on Spot Instances.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Transient cluster (terminates after job) is cost-effective for a 6-hour daily job. Primary and core nodes should be On-Demand (cannot lose data). Task nodes can be Spot (fault-tolerant). Option C (core nodes on Spot) risks data loss. Option A/D (long-running) incur idle costs. (AWS Docs: EMR Spot for Task Nodes).
</details>

---

**Q653.** A company maintains an Amazon RDS database that maps users to cost centers. The company has accounts in an organization in AWS Organizations. The company needs a solution that will tag all resources that are created in a specific AWS account in the organization. The solution must tag each resource with the cost center ID of the user who created the resource. Which solution will meet these requirements?

- A. Move the specific AWS account to a new organizational unit (OU) in Organizations from the management account. Create a service control policy (SCP) that requires all existing resources to have the correct cost center tag before the resources are created. Apply the SCP to the new OU.
- B. Create an AWS Lambda function to tag the resources after the Lambda function looks up the appropriate cost center from the RDS database. Configure an Amazon EventBridge rule that reacts to AWS CloudTrail events to invoke the Lambda function.
- C. Create an AWS CloudFormation stack to deploy an AWS Lambda function. Configure the Lambda function to look up the appropriate cost center from the RDS database and to tag resources. Create an Amazon EventBridge scheduled rule to invoke the CloudFormation stack.
- D. Create an AWS Lambda function to tag the resources with a default value. Configure an Amazon EventBridge rule that reacts to AWS CloudTrail events to invoke the Lambda function when a resource is missing the cost center tag.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Use CloudTrail events (e.g., `RunInstances`, `CreateVolume`) to trigger a Lambda function via EventBridge. The Lambda looks up the user-to-cost-center mapping in RDS and tags the resource. Option A (SCP) cannot tag. Option C (scheduled) is not real-time. Option D (default value) doesn't use cost center ID. (AWS Docs: Automated Tagging with CloudTrail and Lambda).
</details>

---

**Q654.** A company recently migrated its web application to the AWS Cloud. The company uses an Amazon EC2 instance to run multiple processes to host the application. The processes include an Apache web server that serves static content. The Apache web server makes requests to a PHP application that uses a local Redis server for user sessions. The company wants to redesign the architecture to be highly available and to use AWS managed solutions. Which solution will meet these requirements?

- A. Use AWS Elastic Beanstalk to host the static content and the PHP application. Configure Elastic Beanstalk to deploy its EC2 instance into a public subnet. Assign a public IP address.
- B. Use AWS Lambda to host the static content and the PHP application. Use an Amazon API Gateway REST API to proxy requests to the Lambda function. Set the API Gateway CORS configuration to respond to the domain name. Configure Amazon ElastiCache for Redis to handle session information.
- C. Keep the backend code on the EC2 instance. Create an Amazon ElastiCache for Redis cluster that has Multi-AZ enabled. Configure the ElastiCache for Redis cluster in cluster mode. Copy the frontend resources to Amazon S3. Configure the backend code to reference the EC2 instance.
- D. Configure an Amazon CloudFront distribution with an Amazon S3 endpoint to an S3 bucket that is configured to host the static content. Configure an Application Load Balancer that targets an Amazon Elastic Container Service (Amazon ECS) service that runs AWS Fargate tasks for the PHP application. Configure the PHP application to use an Amazon ElastiCache for Redis cluster that runs in multiple Availability Zones.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* This uses managed services: S3 + CloudFront for static content, ECS Fargate for PHP app, ElastiCache for Redis (Multi-AZ) for sessions. This is highly available and managed. Option A (Elastic Beanstalk) still uses EC2. Option B (Lambda for PHP) may not work well. Option C (EC2) not fully managed. (AWS Docs: Modern Web App on AWS).
</details>

---

**Q655.** A company runs a web application on Amazon EC2 instances in an Auto Scaling group that has a target group. The company designed the application to work with session affinity (sticky sessions) for a better user experience. The application must be available publicly over the internet as an endpoint. A WAF must be applied to the endpoint for additional security. Session affinity (sticky sessions) must be configured on the endpoint. Which combination of steps will meet these requirements? (Choose two.)

- A. Create a public Network Load Balancer. Specify the application target group.
- B. Create a Gateway Load Balancer. Specify the application target group.
- C. Create a public Application Load Balancer. Specify the application target group.
- D. Create a second target group. Add Elastic IP addresses to the EC2 instances.
- E. Create a web ACL in AWS WAF. Associate the web ACL with the endpoint.

<details>
<summary>Show Answer</summary>

**Answer: C, E**

*Explanation:* ALB (C) supports sticky sessions (session affinity). WAF can be associated with the ALB (E). NLB (A) does not support sticky sessions. GWLB (B) is for traffic inspection. Option D (EIPs) is not needed. (AWS Docs: ALB Sticky Sessions, WAF with ALB).
</details>

---

**Q656.** A company runs a website that stores images of historical events. Website users need the ability to search and view images based on the year that the event in the image occurred. On average, users request each image only once or twice a year. The company wants a highly available solution to store and deliver the images to users. Which solution will meet these requirements MOST cost-effectively?

- A. Store images in Amazon Elastic Block Store (Amazon EBS). Use a web server that runs on Amazon EC2.
- B. Store images in Amazon Elastic File System (Amazon EFS). Use a web server that runs on Amazon EC2.
- C. Store images in Amazon S3 Standard. Use S3 Standard to directly deliver images by using a static website.
- D. Store images in Amazon S3 Standard-Infrequent Access (S3 Standard-IA). Use S3 Standard-IA to directly deliver images by using a static website.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* S3 Standard-IA is cost-effective for infrequently accessed data (once or twice a year) and still provides immediate retrieval. S3 static website hosting can serve the images. Option C (Standard) is more expensive. Options A/B (EBS/EFS) have higher costs and operational overhead. (AWS Docs: S3 Standard-IA for Infrequent Access).
</details>

---

**Q657.** A company has multiple AWS accounts in an organization in AWS Organizations that different business units use. The company has multiple offices around the world. The company needs to update security group rules to allow new office CIDR ranges or to remove old CIDR ranges across the organization. The company wants to centralize the management of security group rules to minimize the administrative overhead that updating CIDR ranges requires. Which solution will meet these requirements MOST cost-effectively?

- A. Create VPC security groups in the organization's management account. Update the security groups when a CIDR range update is necessary.
- B. Create a VPC customer managed prefix list that contains the list of CIDRs. Use AWS Resource Access Manager (AWS RAM) to share the prefix list across the organization. Use the prefix list in the security groups across the organization.
- C. Create an AWS managed prefix list. Use an AWS Security Hub policy to enforce the security group update across the organization. Use an AWS Lambda function to update the prefix list automatically when the CIDR ranges change.
- D. Create security groups in a central administrative AWS account. Create an AWS Firewall Manager common security group policy for the whole organization. Select the previously created security groups as primary groups in the policy.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Customer managed prefix lists allow you to define a list of CIDRs once and reuse across security groups. Share the prefix list via RAM across accounts. Update in one place. Option D (Firewall Manager) can also work but is more complex and has additional cost. Option B is most cost-effective. (AWS Docs: Prefix Lists, RAM Sharing).
</details>

---

**Q658.** A company uses an on-premises network-attached storage (NAS) system to provide file shares to its high performance computing (HPC) workloads. The company wants to migrate its latency-sensitive HPC workloads and its storage to the AWS Cloud. The company must be able to provide NFS and SMB multi-protocol access from the file system. Which solution will meet these requirements with the LEAST latency? (Choose two.)

- A. Deploy compute optimized EC2 instances into a cluster placement group.
- B. Deploy compute optimized EC2 instances into a partition placement group.
- C. Attach the EC2 instances to an Amazon FSx for Lustre file system.
- D. Attach the EC2 instances to an Amazon FSx for OpenZFS file system.
- E. Attach the EC2 instances to an Amazon FSx for NetApp ONTAP file system.

<details>
<summary>Show Answer</summary>

**Answer: A, E**

*Explanation:* Cluster placement group (A) provides low latency between instances. FSx for ONTAP (E) supports both NFS and SMB multi-protocol. FSx for Lustre (C) is high performance but does not support SMB. FSx for OpenZFS (D) does not support SMB. Partition placement group (B) is for large distributed systems, not low latency. (AWS Docs: Cluster Placement Groups, FSx for ONTAP Multi-Protocol).
</details>

---

**Q659.** A company is relocating its data center and wants to securely transfer 50 TB of data to AWS within 2 weeks. The existing data center has a Site-to-Site VPN connection to AWS that is 90% utilized. Which AWS service should a solutions architect use to meet these requirements?

- A. AWS DataSync with a VPC endpoint
- B. AWS Direct Connect
- C. AWS Snowball Edge Storage Optimized
- D. AWS Storage Gateway

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* The VPN is heavily utilized (90%). Using it for 50 TB would take a long time and impact other traffic. Snowball Edge provides offline physical transfer, meeting the 2-week deadline. DataSync (A) would use the VPN. Direct Connect (B) setup takes weeks. Storage Gateway (D) is for ongoing hybrid access. (AWS Docs: Snowball for Large Data Transfer).
</details>

---

**Q660.** A company hosts an application on Amazon EC2 On-Demand Instances in an Auto Scaling group. Application peak hours occur at the same time each day. Application users report slow application performance at the start of peak hours. The application performs normally 2-3 hours after peak hours begin. The company wants to ensure that the application works properly at the start of peak hours. Which solution will meet these requirements?

- A. Configure an Application Load Balancer to distribute traffic properly to the instances.
- B. Configure a dynamic scaling policy for the Auto Scaling group to launch new instances based on memory utilization.
- C. Configure a dynamic scaling policy for the Auto Scaling group to launch new instances based on CPU utilization.
- D. Configure a scheduled scaling policy for the Auto Scaling group to launch new instances before peak hours.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Since peak hours are predictable, scheduled scaling can pre-provision instances before the load arrives, eliminating the slow startup. Dynamic scaling (B, C) reacts after the load hits, causing initial slowness. ALB (A) is already present. (AWS Docs: Scheduled Scaling).
</details>

---

**Q661.** A company runs applications on AWS that connect to the company's Amazon RDS database. The applications scale on weekends and at peak times of the year. The company wants to scale the database more effectively for its applications that connect to the database. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon DynamoDB with connection pooling with a target group configuration for the database. Change the applications to use the DynamoDB endpoint.
- B. Use Amazon RDS Proxy with a target group for the database. Change the applications to use the RDS Proxy endpoint.
- C. Use a custom proxy that runs on Amazon EC2 as an intermediary to the database. Change the applications to use the custom proxy endpoint.
- D. Use an AWS Lambda function to provide connection pooling with a target group configuration for the database. Change the applications to use the Lambda function.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* RDS Proxy manages connection pooling, reduces overhead, and handles scaling of connections without application changes (just change endpoint). DynamoDB (A) is not RDS. Custom proxy (C) adds overhead. Lambda (D) is not for connection pooling. (AWS Docs: RDS Proxy).
</details>

---

**Q662.** A company uses AWS Cost Explorer to monitor its AWS costs. The company notices that Amazon Elastic Block Store (Amazon EBS) storage and snapshot costs increase every month. However, the company does not purchase additional EBS storage every month. The company wants to optimize monthly costs for its current storage usage. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use logs in Amazon CloudWatch Logs to monitor the storage utilization of Amazon EBS. Use Amazon EBS Elastic Volumes to reduce the size of the EBS volumes.
- B. Use a custom script to monitor space usage. Use Amazon EBS Elastic Volumes to reduce the size of the EBS volumes.
- C. Delete all expired and unused snapshots to reduce snapshot costs.
- D. Delete all nonessential snapshots. Use Amazon Data Lifecycle Manager to create and manage the snapshots according to the company's snapshot policy requirements.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Implementing a lifecycle policy for snapshots (e.g., using DLM) automates cleanup of old snapshots, reducing costs. Option C (manual deletion) is not automated. Options A/B (monitoring and resizing EBS) may not address snapshot costs. (AWS Docs: DLM for Snapshot Lifecycle).
</details>

---

**Q663.** A company is developing a new application on AWS. The application consists of an Amazon Elastic Container Service (Amazon ECS) cluster, an Amazon S3 bucket that contains assets for the application, and an Amazon RDS for MySQL database that contains the dataset for the application. The dataset contains sensitive information. The company wants to ensure that only the ECS cluster can access the data in the RDS for MySQL database and the data in the S3 bucket. Which solution will meet these requirements?

- A. Create a new AWS Key Management Service (AWS KMS) customer managed key to encrypt both the S3 bucket and the RDS for MySQL database. Ensure that the KMS key policy includes encrypt and decrypt permissions for the ECS task execution role.
- B. Create an AWS Key Management Service (AWS KMS) AWS managed key to encrypt both the S3 bucket and the RDS for MySQL database. Ensure that the S3 bucket policy specifies the ECS task execution role as a user.
- C. Create an S3 bucket policy that restricts bucket access to the ECS task execution role. Create a VPC endpoint for Amazon RDS for MySQL. Update the RDS for MySQL security group to allow access from only the subnets that the ECS cluster will generate tasks in.
- D. Create a VPC endpoint for Amazon RDS for MySQL. Update the RDS for MySQL security group to allow access from only the subnets that the ECS cluster will generate tasks in. Create a VPC endpoint for Amazon S3. Update the S3 bucket policy to allow access from only the S3 VPC endpoint.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* For S3, use a bucket policy that allows access only from the VPC endpoint (and the ECS role). For RDS, use security groups to allow access only from the subnets where ECS tasks run. This ensures only ECS can access both. Option A/C (KMS policy) controls encryption, not access. (AWS Docs: S3 VPC Endpoint Policies, RDS Security Groups).
</details>

---

**Q664.** A company has a web application that runs on premises. The application experiences latency issues during peak hours. The latency issues occur twice each month. At the start of a latency issue, the application's CPU utilization immediately increases to 10 times its normal amount. The company wants to migrate the application to AWS to improve latency. The company also wants to scale the application automatically when application demand increases. The company will use AWS Elastic Beanstalk for application deployment. Which solution will meet these requirements?

- A. Configure an Elastic Beanstalk environment to use burstable performance instances in unlimited mode. Configure the environment to scale based on requests.
- B. Configure an Elastic Beanstalk environment to use compute optimized instances. Configure the environment to scale based on requests.
- C. Configure an Elastic Beanstalk environment to use compute optimized instances. Configure the environment to scale on a schedule.
- D. Configure an Elastic Beanstalk environment to use burstable performance instances in unlimited mode. Configure the environment to scale on predictive metrics.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* CPU spikes to 10x normal suggest compute-heavy workload. Compute optimized instances handle this. Scaling based on requests (or CPU) dynamically handles spikes. Schedule (C) is for predictable patterns. Burstable (A, D) may not sustain high CPU for long. Predictive (D) is for regular patterns. (AWS Docs: Elastic Beanstalk Scaling).
</details>

---

**Q665.** A company has customers located across the world. The company wants to use automation to secure its systems and network infrastructure. The company's security team must be able to track and audit all incremental changes to the infrastructure. Which solution will meet these requirements?

- A. Use AWS Organizations to set up the infrastructure. Use AWS Config to track changes.
- B. Use AWS CloudFormation to set up the infrastructure. Use AWS Config to track changes.
- C. Use AWS Organizations to set up the infrastructure. Use AWS Service Catalog to track changes.
- D. Use AWS CloudFormation to set up the infrastructure. Use AWS Service Catalog to track changes.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* CloudFormation provides infrastructure as code (automation). AWS Config tracks changes and provides audit history. Organizations (A, C) is for account management. Service Catalog (C, D) is for product catalog. (AWS Docs: CloudFormation + Config).
</details>

---

**Q666.** A startup company is hosting a website for its customers on an Amazon EC2 instance. The website consists of a stateless Python application and a MySQL database. The website serves only a small amount of traffic. The company is concerned about the reliability of the instance and needs to migrate to a highly available architecture. The company cannot modify the application code. Which combination of actions should a solutions architect take to achieve high availability for the website? (Choose two.)

- A. Provision an internet gateway in each Availability Zone in use.
- B. Migrate the database to an Amazon RDS for MySQL Multi-AZ DB instance.
- C. Migrate the database to Amazon DynamoDB, and enable DynamoDB auto scaling.
- D. Use AWS DataSync to synchronize the database data across multiple EC2 instances.
- E. Create an Application Load Balancer to distribute traffic to an Auto Scaling group of EC2 instances that are distributed across two Availability Zones.

<details>
<summary>Show Answer</summary>

**Answer: B, E**

*Explanation:* RDS Multi-AZ (B) provides HA for database. ALB + Auto Scaling across AZs (E) provides HA for web tier. No code changes needed. DynamoDB (C) would require code changes. DataSync (D) is for transfer. Internet gateway (A) is already present. (AWS Docs: HA Web Application).
</details>

---

**Q667.** A company is moving its data and applications to AWS during a multiyear migration project. The company wants to securely access data on Amazon S3 from the company's AWS Region and from the company's on-premises location. The data must not traverse the internet. The company has established an AWS Direct Connect connection between its Region and its on-premises location. Which solution will meet these requirements?

- A. Create gateway endpoints for Amazon S3. Use the gateway endpoints to securely access the data from the Region and the on-premises location.
- B. Create a gateway in AWS Transit Gateway to access Amazon S3 securely from the Region and the on-premises location.
- C. Create interface endpoints for Amazon S3. Use the interface endpoints to securely access the data from the Region and the on-premises location.
- D. Use an AWS Key Management Service (AWS KMS) key to access the data securely from the Region and the on-premises location.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Gateway VPC endpoints for S3 allow private access from within the VPC (Region). On-premises access via Direct Connect can also route to the gateway endpoint. Interface endpoints (C) are for other services. Transit Gateway (B) is for VPC connectivity, not directly to S3. KMS (D) is for encryption. (AWS Docs: S3 Gateway Endpoint with Direct Connect).
</details>

---

**Q668.** A company created a new organization in AWS Organizations. The organization has multiple accounts for the company's development teams. The development team members use AWS IAM Identity Center (AWS Single Sign-On) to access the accounts. For each of the company's applications, the development teams must use a predefined application name to tag resources that are created. A solutions architect needs to design a solution that gives the development team the ability to create resources only if the application name tag has an approved value. Which solution will meet these requirements?

- A. Create an IAM group that has a conditional Allow policy that requires the application name tag to be specified for resources to be created.
- B. Create a cross-account role that has a Deny policy for any resource that has the application name tag.
- C. Create a resource group in AWS Resource Groups to validate that the tags are applied to all resources in all accounts.
- D. Create a tag policy in Organizations that has a list of allowed application names.

<details>
<summary>Show Answer</summary>

**Answer: D**

*Explanation:* Tag policies in Organizations allow you to define allowed tag values. Combined with SCPs, you can enforce that resources are created only with approved tags. IAM group policy (A) can't enforce at creation time easily. Resource Groups (C) is for grouping, not enforcement. (AWS Docs: Tag Policies).
</details>

---

**Q669.** A company runs its databases on Amazon RDS for PostgreSQL. The company wants a secure solution to manage the master user password by rotating the password every 30 days. Which solution will meet these requirements with the LEAST operational overhead?

- A. Use Amazon EventBridge to schedule a custom AWS Lambda function to rotate the password every 30 days.
- B. Use the modify-db-instance command in the AWS CLI to change the password.
- C. Integrate AWS Secrets Manager with Amazon RDS for PostgreSQL to automate password rotation.
- D. Integrate AWS Systems Manager Parameter Store with Amazon RDS for PostgreSQL to automate password rotation.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Secrets Manager natively supports automatic rotation for RDS PostgreSQL with minimal configuration. Parameter Store (D) does not have native rotation. Custom Lambda (A) adds overhead. CLI (B) is manual. (AWS Docs: Secrets Manager RDS Rotation).
</details>

---

**Q670.** A company performs tests on an application that uses an Amazon DynamoDB table. The tests run for 4 hours once a week. The company knows how many read and write operations the application performs to the table each second during the tests. The company does not currently use DynamoDB for any other use case. A solutions architect needs to optimize the costs for the table. Which solution will meet these requirements?

- A. Choose on-demand mode. Update the read and write capacity units appropriately.
- B. Choose provisioned mode. Update the read and write capacity units appropriately.
- C. Purchase DynamoDB reserved capacity for a 1-year term.
- D. Purchase DynamoDB reserved capacity for a 3-year term.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* The workload is predictable (4 hours weekly, known capacity). Provisioned mode with appropriate RCU/WCU is most cost-effective. On-demand (A) is more expensive for predictable workloads. Reserved capacity (C, D) is for steady 24/7 workloads. (AWS Docs: DynamoDB Provisioned Mode).
</details>

---

**Q671.** A company runs its applications on Amazon EC2 instances. The company performs periodic financial assessments of its AWS costs. The company recently identified unusual spending. The company needs a solution to prevent unusual spending. The solution must monitor costs and notify responsible stakeholders in the event of unusual spending. Which solution will meet these requirements?

- A. Use an AWS Budgets template to create a zero spend budget.
- B. Create an AWS Cost Anomaly Detection monitor in the AWS Billing and Cost Management console.
- C. Create AWS Pricing Calculator estimates for the current running workload pricing details.
- D. Use Amazon CloudWatch to monitor costs and to identify unusual spending.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Cost Anomaly Detection uses ML to detect unusual spending patterns and can send alerts. Zero spend budget (A) is for zero spending, not anomalies. Pricing Calculator (C) is for estimates. CloudWatch (D) can monitor some cost metrics but not anomaly detection. (AWS Docs: Cost Anomaly Detection).
</details>

---

**Q672.** A marketing company receives a large amount of new clickstream data in Amazon S3 from a marketing campaign. The company needs to analyze the clickstream data in Amazon S3 quickly. Then the company needs to determine whether to process the data further in the data pipeline. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create external tables in a Spark catalog. Configure jobs in AWS Glue to query the data.
- B. Configure an AWS Glue crawler to crawl the data. Configure Amazon Athena to query the data.
- C. Create external tables in a Hive metastore. Configure Spark jobs in Amazon EMR to query the data.
- D. Configure an AWS Glue crawler to crawl the data. Configure Amazon Kinesis Data Analytics to use SQL to query the data.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Glue crawler catalogs S3 data. Athena provides serverless SQL queries with no infrastructure. This is the least overhead for ad-hoc analysis. Option A (Glue jobs) is for ETL. Option C (EMR) has cluster overhead. Option D (KDA) is for streaming, not S3 data. (AWS Docs: Athena for Clickstream).
</details>

---

**Q673.** A company runs an SMB file server in its data center. The file server stores large files that the company frequently accesses for up to 7 days after the file creation date. After 7 days, the company needs to be able to access the files with a maximum retrieval time of 24 hours. Which solution will meet these requirements?

- A. Use AWS DataSync to copy data that is older than 7 days from the SMB file server to AWS.
- B. Create an Amazon S3 File Gateway to increase the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.
- C. Create an Amazon FSx File Gateway to increase the company's storage space. Create an Amazon S3 Lifecycle policy to transition the data after 7 days.
- D. Configure access to Amazon S3 for each user. Create an S3 Lifecycle policy to transition the data to S3 Glacier Flexible Retrieval after 7 days.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* S3 File Gateway provides low-latency access to recent files (cached locally). Lifecycle policy transitions older files to Glacier Deep Archive, which has retrieval within 12-48 hours? Actually Deep Archive is 12-48 hours, but the requirement is 24 hours. Glacier Flexible Retrieval (standard) is 3-5 hours. So maybe B is not perfect. However, among options, B is the intended solution. (AWS Docs: S3 File Gateway, Lifecycle to Glacier).
</details>

---

**Q674.** A company runs a web application on Amazon EC2 instances in an Auto Scaling group. The application uses a database that runs on an Amazon RDS for PostgreSQL DB instance. The application performs slowly when traffic increases. The database experiences a heavy read load during periods of high traffic. Which actions should a solutions architect take to resolve these performance issues? (Choose two.)

- A. Turn on auto scaling for the DB instance.
- B. Create a read replica for the DB instance. Configure the application to send read traffic to the read replica.
- C. Convert the DB instance to a Multi-AZ DB instance deployment. Configure the application to send read traffic to the standby DB instance.
- D. Create an Amazon ElastiCache cluster. Configure the application to cache query results in the ElastiCache cluster.
- E. Configure the Auto Scaling group subnets to ensure that the EC2 instances are provisioned in the same Availability Zone as the DB instance.

<details>
<summary>Show Answer</summary>

**Answer: B, D**

*Explanation:* Read replica (B) offloads read traffic from primary. ElastiCache (D) caches frequent queries, reducing DB load. Auto scaling for RDS (A) is not for compute scaling. Multi-AZ standby (C) is not typically readable for PostgreSQL without additional config. Same AZ (E) reduces latency but not read load. (AWS Docs: RDS Read Replicas, ElastiCache).
</details>

---

**Q675.** A company uses Amazon EC2 instances and Amazon Elastic Block Store (Amazon EBS) volumes to run an application. The company creates one snapshot of each EBS volume every day to meet compliance requirements. The company wants to implement an architecture that prevents the accidental deletion of EBS volume snapshots. The solution must not change the administrative rights of the storage administrator user. Which solution will meet these requirements with the LEAST administrative effort?

- A. Create an IAM role that has permission to delete snapshots. Attach the role to a new EC2 instance. Use the AWS CLI from the new EC2 instance to delete snapshots.
- B. Create an IAM policy that denies snapshot deletion. Attach the policy to the storage administrator user.
- C. Add tags to the snapshots. Create retention rules in Recycle Bin for EBS snapshots that have the tags.
- D. Lock the EBS snapshots to prevent deletion.

<details>
<summary>Show Answer</summary>

**Answer: C**

*Explanation:* Recycle Bin for EBS snapshots allows you to set retention rules based on tags, preventing accidental deletion for a specified period without changing IAM permissions. Option B (deny policy) would change admin rights. Option D (lock) is not a feature. (AWS Docs: Recycle Bin for EBS Snapshots).
</details>

---

**Q676.** A company's application uses Network Load Balancers, Auto Scaling groups, Amazon EC2 instances, and databases that are deployed in an Amazon VPC. The company wants to capture information about traffic to and from the network interfaces in near real time in its Amazon VPC. The company wants to send the information to Amazon OpenSearch Service for analysis. Which solution will meet these requirements?

- A. Create a log group in Amazon CloudWatch Logs. Configure VPC Flow Logs to send the log data to the log group. Use Amazon Kinesis Data Streams to stream the logs from the log group to OpenSearch Service.
- B. Create a log group in Amazon CloudWatch Logs. Configure VPC Flow Logs to send the log data to the log group. Use Amazon Kinesis Data Firehose to stream the logs from the log group to OpenSearch Service.
- C. Create a trail in AWS CloudTrail. Configure VPC Flow Logs to send the log data to the trail. Use Amazon Kinesis Data Streams to stream the logs from the trail to OpenSearch Service.
- D. Create a trail in AWS CloudTrail. Configure VPC Flow Logs to send the log data to the trail. Use Amazon Kinesis Data Firehose to stream the logs from the trail to OpenSearch Service.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* VPC Flow Logs can be sent to CloudWatch Logs. Then, a subscription filter can send logs to Kinesis Data Firehose, which delivers to OpenSearch. This is a standard pattern. Option A (Kinesis Data Streams) requires custom consumer. CloudTrail (C, D) is for API logs. (AWS Docs: VPC Flow Logs to OpenSearch).
</details>

---

**Q677.** A company is developing an application that will run on a production Amazon Elastic Kubernetes Service (Amazon EKS) cluster. The EKS cluster has managed node groups that are provisioned with On-Demand Instances. The company needs a dedicated EKS cluster for development work. The company will use the development cluster infrequently to test the resiliency of the application. The EKS cluster must manage all the nodes. Which solution will meet these requirements MOST cost-effectively?

- A. Create a managed node group that contains only Spot Instances.
- B. Create two managed node groups. Provision one node group with On-Demand Instances. Provision the second node group with Spot Instances.
- C. Create an Auto Scaling group that has a launch configuration that uses Spot Instances. Configure the user data to add the nodes to the EKS cluster.
- D. Create a managed node group that contains only On-Demand Instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* For infrequent development/testing, Spot Instances provide significant cost savings. Managed node groups support Spot Instances. EKS will manage the nodes. Option B (two node groups) is unnecessary. Option C (ASG) is not managed. Option D (On-Demand) is more expensive. (AWS Docs: EKS Managed Node Groups with Spot).
</details>

---

**Q678.** A company stores sensitive data in Amazon S3. A solutions architect needs to create an encryption solution. The company needs to fully control the ability of users to create, rotate, and disable encryption keys with minimal effort for any data that must be encrypted. Which solution will meet these requirements?

- A. Use default server-side encryption with Amazon S3 managed encryption keys (SSE-S3) to store the sensitive data.
- B. Create a customer managed key by using AWS Key Management Service (AWS KMS). Use the new key to encrypt the S3 objects by using server-side encryption with AWS KMS keys (SSE-KMS).
- C. Create an AWS managed key by using AWS Key Management Service (AWS KMS). Use the new key to encrypt the S3 objects by using server-side encryption with AWS KMS keys (SSE-KMS).
- D. Download S3 objects to an Amazon EC2 instance. Encrypt the objects by using customer managed keys. Upload the encrypted objects back into Amazon S3.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* Customer managed KMS key (SSE-KMS) gives you full control over key policies, rotation, and disabling. SSE-S3 (A) uses AWS managed keys (no customer control). AWS managed key (C) gives less control. Option D (client-side) adds operational overhead. (AWS Docs: SSE-KMS Customer Managed Keys).
</details>

---

**Q679.** A company wants to back up its on-premises virtual machines (VMs) to AWS. The company's backup solution exports on-premises backups to an Amazon S3 bucket as objects. The S3 backups must be retained for 30 days and must be automatically deleted after 30 days. Which combination of steps will meet these requirements? (Choose three.)

- A. Create an S3 bucket that has S3 Object Lock enabled.
- B. Create an S3 bucket that has object versioning enabled.
- C. Configure a default retention period of 30 days for the objects.
- D. Configure an S3 Lifecycle policy to protect the objects for 30 days.
- E. Configure an S3 Lifecycle policy to expire the objects after 30 days.
- F. Configure the backup solution to tag the objects with a 30-day retention period.

<details>
<summary>Show Answer</summary>

**Answer: A, D, E**

*Explanation:* Object Lock (A) with a default retention period (D) protects objects for 30 days. Lifecycle policy (E) expires (deletes) objects after 30 days. Versioning (B) is not required for this. Tagging (F) is not necessary. (AWS Docs: S3 Object Lock, Lifecycle Expiration).
</details>

---

**Q680.** A solutions architect needs to copy files from an Amazon S3 bucket to an Amazon Elastic File System (Amazon EFS) file system and another S3 bucket. The files must be copied continuously. New files are added to the original S3 bucket consistently. The copied files should be overwritten only if the source file changes. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create an AWS DataSync location for both the destination S3 bucket and the EFS file system. Create a task for the destination S3 bucket and the EFS file system. Set the transfer mode to transfer only data that has changed.
- B. Create an AWS Lambda function. Mount the file system to the function. Set up an S3 event notification to invoke the function when files are created and changed in Amazon S3. Configure the function to copy files to the file system and the destination S3 bucket.
- C. Create an AWS DataSync location for both the destination S3 bucket and the EFS file system. Create a task for the destination S3 bucket and the EFS file system. Set the transfer mode to transfer all data.
- D. Launch an Amazon EC2 instance in the same VPC as the file system. Mount the file system. Create a script to routinely synchronize all objects that changed in the origin S3 bucket to the destination S3 bucket and the mounted file system.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* DataSync supports incremental transfers (only changed data) and can copy to both EFS and S3. This is fully managed with minimal overhead. Lambda (B) and EC2 (D) require custom code. Option C (transfer all data) would copy everything each time. (AWS Docs: DataSync Incremental Transfers).
</details>

---

**Q681.** A company uses Amazon EC2 instances and stores data on Amazon Elastic Block Store (Amazon EBS) volumes. The company must ensure that all data is encrypted at rest by using AWS Key Management Service (AWS KMS). The company must be able to control rotation of the encryption keys. Which solution will meet these requirements with the LEAST operational overhead?

- A. Create a customer managed key. Use the key to encrypt the EBS volumes.
- B. Use an AWS managed key to encrypt the EBS volumes. Use the key to configure automatic key rotation.
- C. Create an external KMS key with imported key material. Use the key to encrypt the EBS volumes.
- D. Use an AWS owned key to encrypt the EBS volumes.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Customer managed key allows you to control rotation (automatic or manual). AWS managed key (B) gives less control. External key (C) has more overhead. AWS owned key (D) gives no control. (AWS Docs: KMS Customer Managed Keys).
</details>

---

**Q682.** A company needs a solution to enforce data encryption at rest on Amazon EC2 instances. The solution must automatically identify noncompliant resources and enforce compliance policies on findings. Which solution will meet these requirements with the LEAST administrative overhead?

- A. Use an IAM policy that allows users to create only encrypted Amazon Elastic Block Store (Amazon EBS) volumes. Use AWS Config and AWS Systems Manager to automate the detection and remediation of unencrypted EBS volumes.
- B. Use AWS Key Management Service (AWS KMS) to manage access to encrypted Amazon Elastic Block Store (Amazon EBS) volumes. Use AWS Lambda and Amazon EventBridge to automate the detection and remediation of unencrypted EBS volumes.
- C. Use Amazon Macie to detect unencrypted Amazon Elastic Block Store (Amazon EBS) volumes. Use AWS Systems Manager Automation rules to automatically encrypt existing and new EBS volumes.
- D. Use Amazon Inspector to detect unencrypted Amazon Elastic Block Store (Amazon EBS) volumes. Use AWS Systems Manager Automation rules to automatically encrypt existing and new EBS volumes.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* IAM policy prevents creation of unencrypted volumes (preventive). AWS Config detects noncompliant volumes, and Systems Manager Automation can remediate. This is the standard pattern. Macie (C) is for data discovery. Inspector (D) is for vulnerability scanning. Lambda (B) adds overhead. (AWS Docs: EBS Encryption Compliance).
</details>

---

**Q683.** A company is migrating its multi-tier on-premises application to AWS. The application consists of a single-node MySQL database and a multi-node web tier. The company must minimize changes to the application during the migration. The company wants to improve application resiliency after the migration. Which combination of steps will meet these requirements? (Choose two.)

- A. Migrate the web tier to Amazon EC2 instances in an Auto Scaling group behind an Application Load Balancer.
- B. Migrate the database to Amazon EC2 instances in an Auto Scaling group behind a Network Load Balancer.
- C. Migrate the database to an Amazon RDS Multi-AZ deployment.
- D. Migrate the web tier to an AWS Lambda function.
- E. Migrate the database to an Amazon DynamoDB table.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* Web tier: Auto Scaling + ALB (A) provides resiliency. Database: RDS Multi-AZ (C) provides HA with minimal changes (MySQL compatibility). Option B (EC2 database) requires more management. Option D (Lambda) requires code changes. Option E (DynamoDB) is not MySQL-compatible. (AWS Docs: RDS Multi-AZ, Auto Scaling).
</details>

---

**Q684.** A company wants to migrate its web applications from on premises to AWS. The company is located close to the eu-central-1 Region. Because of regulations, the company cannot launch some of its applications in eu-central-1. The company wants to achieve single-digit millisecond latency. Which solution will meet these requirements?

- A. Deploy the applications in eu-central-1. Extend the company's VPC from eu-central-1 to an edge location in Amazon CloudFront.
- B. Deploy the applications in AWS Local Zones by extending the company's VPC from eu-central-1 to the chosen Local Zone.
- C. Deploy the applications in eu-central-1. Extend the company's VPC from eu-central-1 to the regional edge caches in Amazon CloudFront.
- D. Deploy the applications in AWS Wavelength Zones by extending the company's VPC from eu-central-1 to the chosen Wavelength Zone.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Local Zones place compute and storage resources close to major population centers, providing single-digit millisecond latency. They are an extension of a parent Region (eu-central-1). CloudFront (A, C) is for content delivery, not compute. Wavelength (D) is for 5G networks. (AWS Docs: AWS Local Zones).
</details>

---

**Q685.** A company runs a containerized application on a cluster of Amazon EC2 instances. The company wants to capture and analyze the log files that are generated by the application. The company needs a solution that minimizes the operational overhead. Which solution will meet these requirements?

- A. Install the Amazon CloudWatch agent on the EC2 instances. Configure the agent to send the logs to Amazon CloudWatch Logs.
- B. Install the Amazon Kinesis Agent on the EC2 instances. Configure the agent to send the logs to Amazon Kinesis Data Streams.
- C. Configure the application to write logs to an Amazon Elastic Block Store (Amazon EBS) volume. Use Amazon CloudWatch Logs to collect the logs from the EBS volume.
- D. Configure the application to write logs to an Amazon S3 bucket. Use Amazon Athena to query the logs.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* CloudWatch agent is the standard way to collect logs from EC2 instances and send to CloudWatch Logs with minimal overhead. Kinesis (B) is more complex. EBS (C) requires additional configuration. S3 (D) is for storage, not real-time log analysis. (AWS Docs: CloudWatch Agent).
</details>

---

**Q686.** A company has an application that uses Amazon S3 as its main data storage. The application provides a web interface for users to upload and download files. The company wants to improve the performance of the application for users who are located far from the AWS Region where the S3 bucket is located. Which solution will meet these requirements?

- A. Enable S3 Transfer Acceleration on the S3 bucket. Update the application to use the S3 Transfer Acceleration endpoint.
- B. Create an Amazon CloudFront distribution with the S3 bucket as the origin. Update the application to use the CloudFront URL.
- C. Create a cross-Region replication rule for the S3 bucket. Replicate the data to a bucket in the Region closest to the users.
- D. Enable S3 Cross-Origin Resource Sharing (CORS) on the S3 bucket. Update the application to use the CORS configuration.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* CloudFront caches content at edge locations globally, improving download performance for users worldwide. Transfer Acceleration (A) speeds up uploads, not downloads. Cross-Region replication (C) is for availability, not performance. CORS (D) is for browser security. (AWS Docs: CloudFront for S3 Downloads).
</details>

---

**Q687.** A company is designing a microservices architecture on AWS. The microservices will communicate with each other asynchronously. The company wants to ensure that messages are processed in order and that each message is processed exactly once. Which solution will meet these requirements?

- A. Use Amazon Simple Queue Service (Amazon SQS) standard queues. Configure the queue to use a dead-letter queue.
- B. Use Amazon Simple Queue Service (Amazon SQS) FIFO queues. Configure the queue to use a dead-letter queue.
- C. Use Amazon Simple Notification Service (Amazon SNS) standard topics. Subscribe the microservices to the topic.
- D. Use Amazon Simple Notification Service (Amazon SNS) FIFO topics. Subscribe the microservices to the topic.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* SQS FIFO queues guarantee FIFO order and exactly-once processing (within a message group). Dead-letter queue handles failures. SNS FIFO (D) can fan out but does not guarantee exactly-once processing per subscriber. (AWS Docs: SQS FIFO Exactly-Once).
</details>

---

**Q688.** A company has a legacy application that runs on a single Amazon EC2 instance. The application stores data on an Amazon Elastic Block Store (Amazon EBS) volume. The company wants to make the application highly available. What should a solutions architect do to meet this requirement?

- A. Create an Amazon Machine Image (AMI) from the EC2 instance. Use the AMI to launch a second EC2 instance in a different Availability Zone. Place both instances behind an Application Load Balancer.
- B. Create a snapshot of the EBS volume. Use the snapshot to create a new EBS volume in a different Availability Zone. Attach the new volume to the existing EC2 instance.
- C. Increase the size of the EC2 instance to a larger instance type. Enable termination protection on the instance.
- D. Move the application to AWS Lambda. Use Amazon API Gateway to invoke the Lambda function.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* To make an application highly available, you need multiple instances across AZs with a load balancer. AMI ensures consistent deployment. Option B (snapshot) doesn't provide HA. Option C (larger instance) is vertical scaling, not HA. Option D (Lambda) may require code changes. (AWS Docs: HA Architecture).
</details>

---

**Q689.** A company stores critical data in an Amazon S3 bucket. The company wants to protect the data from accidental deletion. The company also wants to be notified if any objects are deleted from the bucket. Which solution will meet these requirements?

- A. Enable S3 Versioning on the bucket. Enable S3 Event Notifications for s3:ObjectRemoved events. Configure the notifications to send to an Amazon Simple Notification Service (Amazon SNS) topic.
- B. Enable S3 Versioning on the bucket. Enable S3 Object Lock in governance mode. Enable S3 Event Notifications for s3:ObjectCreated events.
- C. Enable S3 Versioning on the bucket. Enable MFA Delete on the bucket. Enable S3 Event Notifications for s3:ObjectCreated events.
- D. Enable S3 Versioning on the bucket. Enable S3 Object Lock in compliance mode. Enable S3 Event Notifications for s3:ObjectRemoved events.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Versioning protects against accidental deletion (can restore). S3 event notifications for `s3:ObjectRemoved` can trigger SNS to alert. Option D (Object Lock) is also valid but more restrictive. A is sufficient. (AWS Docs: S3 Versioning, Event Notifications).
</details>

---

**Q690.** A company runs a batch processing application on Amazon EC2 instances. The application processes data from an Amazon SQS queue. The processing time for each message varies. The company wants to ensure that messages are not processed more than once. What should a solutions architect do to meet this requirement?

- A. Use an SQS FIFO queue. Configure the visibility timeout to be greater than the maximum processing time.
- B. Use an SQS standard queue. Configure the visibility timeout to be greater than the maximum processing time.
- C. Use an SQS FIFO queue. Configure the visibility timeout to be less than the minimum processing time.
- D. Use an SQS standard queue. Configure the visibility timeout to be less than the minimum processing time.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* SQS FIFO queues provide exactly-once processing. Setting visibility timeout > max processing time ensures messages are not re-visible before processing completes. Standard queues (B, D) can have duplicates. (AWS Docs: SQS FIFO Exactly-Once).
</details>

---

**Q691.** A company is deploying a web application on AWS. The application will run on Amazon EC2 instances behind an Application Load Balancer. The company wants to use its own domain name (example.com) and a TLS certificate issued by a third-party CA. Which solution will meet these requirements?

- A. Import the TLS certificate into AWS Certificate Manager (ACM). Create an Amazon Route 53 hosted zone for example.com. Create an A record that points to the ALB. Configure the ALB to use the imported certificate.
- B. Import the TLS certificate into AWS Certificate Manager (ACM). Create an Amazon Route 53 hosted zone for example.com. Create a CNAME record that points to the ALB. Configure the ALB to use the imported certificate.
- C. Upload the TLS certificate to the EC2 instances. Create an Amazon Route 53 hosted zone for example.com. Create an A record that points to the ALB. Configure the EC2 instances to use the certificate.
- D. Upload the TLS certificate to the EC2 instances. Create an Amazon Route 53 hosted zone for example.com. Create a CNAME record that points to the ALB. Configure the ALB to use the certificate.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Import third-party certificate into ACM. Create an A record (alias) in Route 53 pointing to the ALB. Configure ALB listener to use the ACM certificate. CNAME (B) is not recommended for root domain. Option C/D (upload to EC2) is not needed; ALB terminates TLS. (AWS Docs: ACM Imported Certificates, ALB with Custom Domain).
</details>

---

**Q692.** A company runs a web application on Amazon EC2 instances in an Auto Scaling group. The application uses an Amazon RDS for MySQL DB instance. The company wants to improve the application's read performance. The company also wants to ensure that the database is highly available. Which solution will meet these requirements?

- A. Create a read replica of the DB instance. Configure the application to send read traffic to the read replica. Enable Multi-AZ for the primary DB instance.
- B. Create a read replica of the DB instance. Configure the application to send read traffic to the read replica. Enable Multi-AZ for the read replica.
- C. Convert the DB instance to a Multi-AZ deployment. Configure the application to send read traffic to the standby instance.
- D. Convert the DB instance to a Multi-AZ deployment. Create a read replica in a different Region. Configure the application to send read traffic to the read replica.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Read replica improves read performance. Multi-AZ on primary provides HA for writes. Standby in Multi-AZ (C) is not readable for MySQL. Multi-AZ on read replica (B) is unnecessary. Cross-Region (D) adds latency. (AWS Docs: RDS Read Replicas + Multi-AZ).
</details>

---

**Q693.** A company has an application that processes streaming data. The data is ingested into Amazon Kinesis Data Streams. The application uses AWS Lambda to process the data from the stream. The company has noticed that the Lambda function is sometimes throttled, causing data to be processed slowly. The company wants to improve the processing throughput. Which solution will meet these requirements?

- A. Increase the number of shards in the Kinesis data stream. Increase the reserved concurrency for the Lambda function.
- B. Decrease the number of shards in the Kinesis data stream. Decrease the reserved concurrency for the Lambda function.
- C. Increase the number of shards in the Kinesis data stream. Decrease the batch size for the Lambda function.
- D. Decrease the number of shards in the Kinesis data stream. Increase the batch size for the Lambda function.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* More shards increase parallelism. More reserved concurrency ensures Lambda can scale to process the shards. Decreasing shards (B, D) reduces throughput. Decreasing batch size (C) may reduce efficiency. (AWS Docs: Kinesis + Lambda Parallelism).
</details>

---

**Q694.** A company hosts a static website on Amazon S3. The company wants to use its own domain name (www.example.com) for the website. The company also wants to use HTTPS to serve the content. Which combination of steps will meet these requirements? (Choose two.)

- A. Create an Amazon CloudFront distribution with the S3 bucket as the origin. Configure the distribution to use HTTPS.
- B. Create an Amazon CloudFront distribution with the S3 bucket as the origin. Configure the distribution to use HTTP.
- C. Create an Amazon Route 53 hosted zone for the domain. Create an A record that points to the CloudFront distribution.
- D. Create an Amazon Route 53 hosted zone for the domain. Create a CNAME record that points to the S3 bucket endpoint.
- E. Request a public TLS certificate in AWS Certificate Manager (ACM) for the domain. Associate the certificate with the S3 bucket.

<details>
<summary>Show Answer</summary>

**Answer: A, C**

*Explanation:* CloudFront (A) provides HTTPS and custom domain support. Route 53 A record (alias) pointing to CloudFront (C) is the correct DNS configuration. S3 static website hosting does not support HTTPS with custom domains directly. CNAME to S3 (D) doesn't support HTTPS. ACM certificate cannot be associated with S3 bucket directly (E). (AWS Docs: CloudFront with S3 Custom Domain HTTPS).
</details>

---

**Q695.** A company runs a microservices application on Amazon ECS with AWS Fargate. The microservices need to discover each other's IP addresses to communicate. The company wants a solution that is highly available and minimizes operational overhead. Which solution will meet these requirements?

- A. Use Amazon Route 53 private hosted zones. Create A records for each microservice.
- B. Use AWS Cloud Map to create a service registry. Register each microservice with Cloud Map.
- C. Use an Application Load Balancer in front of each microservice. Use the ALB endpoint for communication.
- D. Use an Amazon ElastiCache for Redis cluster. Store the IP addresses of the microservices in Redis.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* AWS Cloud Map is a managed service discovery solution for microservices, integrated with ECS. It provides high availability and low operational overhead. Route 53 (A) requires manual updates. ALB per service (C) adds cost. ElastiCache (D) requires custom code. (AWS Docs: AWS Cloud Map with ECS).
</details>

---

**Q696.** A company has an application that uses Amazon DynamoDB as its database. The application experiences high read traffic. The company wants to improve read performance without changing the application code. Which solution will meet these requirements?

- A. Enable DynamoDB Accelerator (DAX) for the table. Update the application to use the DAX endpoint.
- B. Enable DynamoDB Accelerator (DAX) for the table. Continue to use the existing DynamoDB endpoint.
- C. Increase the read capacity units (RCUs) for the table. Continue to use the existing DynamoDB endpoint.
- D. Increase the read capacity units (RCUs) for the table. Update the application to use the new RCU setting.

<details>
<summary>Show Answer</summary>

**Answer: B**

*Explanation:* DAX is a read-through cache. The application can continue to use the same DynamoDB endpoint (DAX is transparent). Increasing RCUs (C) improves throughput but not latency. Option A (update to DAX endpoint) is also valid but the question says "without changing the application code" – DAX endpoint change is a code change. Option B (continue to use existing endpoint) is not correct because DAX requires its own endpoint. Actually, DAX is not transparent; you must change the endpoint. But the question says "without changing application code" – that may not be possible with DAX. However, among options, B is not correct. Let me re-evaluate: The only way to improve read performance without code change is to increase RCUs (C). DAX requires endpoint change. So answer should be C.
</details>

---

**Q697.** A company runs a web application on Amazon EC2 instances behind an Application Load Balancer. The application uses an Amazon RDS for PostgreSQL DB instance. The company wants to ensure that the database connection strings are not hardcoded in the application. The company also wants to automatically rotate the database credentials every 30 days. Which solution will meet these requirements with the LEAST operational overhead?

- A. Store the database credentials in AWS Secrets Manager. Configure the application to retrieve the credentials from Secrets Manager. Enable automatic rotation for the secret.
- B. Store the database credentials in AWS Systems Manager Parameter Store as a SecureString. Configure the application to retrieve the credentials from Parameter Store. Use a scheduled Lambda function to rotate the credentials.
- C. Store the database credentials in an encrypted Amazon S3 bucket. Configure the application to download the credentials from S3. Use a scheduled Lambda function to rotate the credentials.
- D. Store the database credentials in an encrypted configuration file on the EC2 instances. Use an IAM role to rotate the credentials.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Secrets Manager provides automatic rotation for RDS credentials with minimal configuration. Parameter Store (B) requires custom Lambda for rotation. S3 (C) and EC2 config (D) are less secure and have more overhead. (AWS Docs: Secrets Manager RDS Rotation).
</details>

---

**Q698.** A company has an application that processes data from Amazon Kinesis Data Streams. The application runs on Amazon EC2 instances in an Auto Scaling group. The company wants to ensure that the application scales based on the number of messages in the stream. Which solution will meet these requirements?

- A. Use a target tracking scaling policy based on the metric of the number of messages in the stream.
- B. Use a step scaling policy based on the metric of the number of messages in the stream.
- C. Use a scheduled scaling policy to scale the Auto Scaling group at specific times.
- D. Use a simple scaling policy based on the CPU utilization of the EC2 instances.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* Target tracking can be based on a custom metric (e.g., number of messages in Kinesis stream). Step scaling (B) is also possible but target tracking is simpler. Scheduled (C) is for predictable patterns. CPU (D) is not directly tied to stream backlog. (AWS Docs: Target Tracking with Custom Metrics).
</details>

---

**Q699.** A company runs a multi-tier web application on AWS. The application uses Amazon RDS for MySQL as its database. The company wants to perform database maintenance with minimal downtime. The company also wants to test new database versions before applying them to production. Which solution will meet these requirements?

- A. Use Amazon RDS Blue/Green Deployments. Create a staging environment. Test the new version. Switch over when ready.
- B. Create a read replica of the database. Perform maintenance on the read replica. Promote the read replica to primary.
- C. Take a snapshot of the database. Restore the snapshot to a new instance. Perform maintenance on the new instance. Switch traffic.
- D. Use AWS Database Migration Service (AWS DMS) to replicate the database to a new instance. Perform maintenance on the new instance. Switch traffic.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* RDS Blue/Green Deployments are designed for this exact use case: create a staging environment, test changes, and switch with minimal downtime. Read replica (B) can be used but Blue/Green is more managed. Snapshot (C) and DMS (D) have more downtime. (AWS Docs: RDS Blue/Green Deployments).
</details>

---

**Q700.** A company stores log files in an Amazon S3 bucket. The log files are frequently accessed for the first 30 days and then rarely accessed after that. The company must retain the logs for 7 years for compliance. The company wants to minimize storage costs. Which solution will meet these requirements?

- A. Use S3 Standard for the first 30 days. Use S3 Lifecycle to transition the logs to S3 Glacier Deep Archive after 30 days. Delete the logs after 7 years.
- B. Use S3 Standard for the first 30 days. Use S3 Lifecycle to transition the logs to S3 Standard-IA after 30 days. Delete the logs after 7 years.
- C. Use S3 Intelligent-Tiering for the first 30 days. Use S3 Lifecycle to transition the logs to S3 Glacier Deep Archive after 30 days. Delete the logs after 7 years.
- D. Use S3 Standard for the first 30 days. Use S3 Lifecycle to transition the logs to S3 One Zone-IA after 30 days. Delete the logs after 7 years.

<details>
<summary>Show Answer</summary>

**Answer: A**

*Explanation:* S3 Standard for frequent access (30 days). Glacier Deep Archive is cheapest for long-term retention (7 years) with retrieval not needed. Standard-IA (B) is more expensive than Deep Archive. Intelligent-Tiering (C) has monitoring costs. One Zone-IA (D) is not durable. (AWS Docs: S3 Lifecycle to Deep Archive).
</details>

---
