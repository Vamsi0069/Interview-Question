####  What is StorageClass?

* Abstracts dynamic volume provisioning (e.g., gp2, io1 in AWS EBS).


####  Where is the Terraform state stored?

* Locally (`terraform.tfstate`) or remotely (e.g., S3 + DynamoDB for locking).

####  Security Group vs NACL?

* SG: stateful, instance-level.
* NACL: stateless, subnet-level.

####  What is AWS Session Manager?

* Connect to EC2 without SSH/key, via Systems Manager.

####  What is AMI?

* Amazon Machine Image: snapshot/template for EC2.

####  Create AMI from EC2?

* AWS Console > Actions > Create Image.

####  EBS vs EFS?

* EBS: block storage for one instance.
* EFS: NFS-based, shared across instances.

####  What is AWS Backup?

* Centralized service to automate backup of AWS resources (RDS, EBS, etc).

####  What is RDS MySQL?

* Managed MySQL DB service.

####  What is AWS Lambda?

* Serverless compute that runs code on triggers.

####  VPC Peering (A-B-C)?

* Create peerings: A↔B, B↔C, A↔C (fully meshed).

####  Elastic IP vs Public IP?

* Elastic IP: static.
* Public IP: dynamic unless associated.

####  ECS launch types?

* EC2 (self-managed) and Fargate (serverless).

####  Monitor EC2 with CloudWatch?

* Enable detailed monitoring.
* Use custom metrics or CloudWatch Agent.

####  AWS Step Functions?

* Serverless workflow orchestration (like state machines).

#### What is a NAT Gateway? And what is its use?
A NAT Gateway (Network Address Translation Gateway) allows private subnet instances to initiate outbound internet traffic (like installing packages, pulling updates) without exposing them to incoming connections from the public internet.

Example: EC2s in a private subnet can yum update via NAT Gateway.

#### Difference between NAT Gateway and Internet Gateway
Feature	Internet Gateway	NAT Gateway
Direction	Bidirectional (inbound/outbound)	Outbound only
Attached To	VPC	Subnet
Used For	Public Subnet EC2s to connect to internet	Private Subnet EC2s to access internet
Security	Exposes public IP	No public IP exposed

#### What is AWS CloudTrail and how do you manage it in your project?
CloudTrail logs all API calls and activities across AWS accounts (who did what, when, from where).
We manage CloudTrail by:

Storing logs in versioned S3 buckets

Enabling multi-region trails

Using CloudWatch Logs integration to trigger alerts (e.g., for unauthorized actions)

Applying S3 bucket policies and encryption with KMS for security

#### Suppose your team member is accessing an AWS service, and you want to revoke their access?
Update or remove their IAM policy or role assignment

Use Service Control Policies (SCPs) in AWS Organizations (if in multi-account setup)

For immediate revocation: disable or delete the IAM user, or rotate their credentials

#### How do you connect from a public subnet to a private subnet?
Use bastion host (jump box) in the public subnet

Connect via SSH:
```ssh -i key.pem ec2-user@<bastion-ip> → ssh into private IP```

Alternatively, set up:

AWS Systems Manager Session Manager for secure agent-based access (no need to open ports)

#### Why do we use a private subnet in a VPC?

For enhanced security. Resources (like databases) in private subnets are not accessible from the internet.

#### How many AWS accounts are used in your current project?

Usually at least 3: Dev, Staging, Prod. Sometimes separate accounts for Security, Billing, or Sandbox.

#### How can you download files from an S3 bucket within AWS while keeping public access completely blocked?

Use pre-signed URLs, IAM roles, or VPC endpoints to download securely without public access.

#### How do you monitor your applications and EC2 instances using Amazon CloudWatch?

Use CloudWatch metrics, custom dashboards, alarms, and logs agents to monitor CPU, memory, disk, logs, and custom app metrics.

#### Have you worked with AWS Transit Gateway in your project?

Transit Gateway helps in centralized VPC-to-VPC and on-prem connectivity across accounts. It simplifies the network mesh.

#### How should developers write code to access AWS services (S3, SQS, SNS, RDS)?
A:
- Use AWS SDKs
- Access via IAM roles with least privilege
- Store secrets in Secrets Manager
- Follow retry/backoff logic
- Use env variables for config
- Log via CloudWatch

#### How to handle developer authentication to AWS?
A:
- Use IAM users/groups with MFA
- Prefer IAM roles with temporary STS credentials
- Use AWS SSO or federation
- Manage secrets via AWS Vault or CLI profiles
- Rotate creds, no hardcoding

#### How to enable communication between different VPCs?
A:
- VPC Peering: Direct, simple, but no transitive routing
- Transit Gateway: Centralized hub, scalable
- PrivateLink: For exposing services, not full VPC access
- VPN: For secure cross-region or hybrid setups

#### What is an Internet Gateway, and where is it placed?
Answer:
An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet.
It is attached at the VPC level, not the subnet level.

#### What is VPC Peering, and how do you configure it?
Answer:
VPC Peering connects two VPCs to route traffic using private IPs.
Steps to configure:
Create a VPC peering connection.
Accept the request from the target VPC.
Add route table entries in both VPCs.
Ensure security groups and NACLs allow traffic.

#### How do you give private access to an S3 bucket?
Answer:

Remove public access settings.
Attach bucket policy that allows access from specific VPC or IAM roles.
Use VPC endpoint for S3 for private access without using the internet.

#### What is CloudFront?
Answer:
CloudFront is AWS’s Content Delivery Network (CDN) that caches content at edge locations to reduce latency and speed up delivery.

#### What is a NAT Gateway, and where do you place it?
Answer:
A NAT Gateway enables instances in a private subnet to access the internet (for updates, etc.) while remaining unreachable from the outside.
It is placed in a public subnet and requires a route from private subnets to the NAT Gateway.

#### What are bucket policies in AWS S3?
Answer:
Bucket policies are JSON-based rules that control access to an entire S3 bucket or objects inside.

Example:
```
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": ["s3:GetObject"],
  "Resource": "arn:aws:s3:::mybucket/*"
}
```
#### If you get a 403 error (Access Denied) in S3, what do you check?
Answer:

Check bucket policy
Check IAM role/user permissions
Verify if object ACL is private
Ensure correct Region and signed URL, if applicable

#### We check the policy: GetObject, PutObject — why?
Answer:
These are the S3 actions needed to download (GetObject) or upload (PutObject) files.
Without them, you’ll get 403 errors.

#### 81Q. What is 2/2 check in AWS EC2?
Answer:

EC2 passes 2 checks:
System status check
Instance status check
Both must be "2/2 checks passed" for healthy status.

#### Why did you use S3 in your project?
Answer:

Store logs, backups, artifacts.
Host static websites.
Use as Terraform backend (state file storage).

#### What is a bucket policy in AWS?
Answer:
JSON document attached to a bucket to define access rules for users, roles, or the public.

#### What is 403 error in S3 object permission?
Answer:
It means access denied. Possible causes:
Missing s3:GetObject permission
Object is private
Bucket policy restricts access

#### What is AWS Serverless and what are its key benefits?

AWS Serverless refers to a cloud-native development model that allows you to build and run applications without managing servers. Key services include:
•	AWS Lambda – run code in response to events.
•	Amazon API Gateway – expose APIs.
•	Amazon DynamoDB – NoSQL database.
•	AWS Step Functions – orchestrate workflows

#### What is a Key Pair in AWS?
A:
A Key Pair is used for secure SSH access to EC2 instances. It includes a public key (stored in AWS) and a private key (downloaded by the user).
You create it in EC2 Dashboard → Key Pairs → Create Key Pair, and use it when launching an EC2 instance.


#### What is AWS VPC peering?
A: VPC peering is a networking connection between two VPCs that enables routing traffic between them using private IPs. Peering works across regions and accounts but does not support transitive peering.

#### What AWS resources have you worked with?
A:
I’ve worked with a wide range of AWS resources, including:
•	EC2 (virtual machines)
•	S3 (object storage)
•	RDS (managed databases)
•	ECS/Fargate (containers)
•	Lambda (serverless compute)
•	IAM (access control)
•	CloudWatch (monitoring/logs)
•	VPC (networking)
•	Route 53 (DNS)
•	Auto Scaling Groups
•	ALB/NLB (load balancers)
•	Elastic Beanstalk, CloudFormation, and Terraform for provisioning

#### What AWS services do you use for scaling instances?
A:
For automatic and manual scaling, I’ve used:
•	Auto Scaling Groups (ASG): Automatically add/remove EC2 instances based on CPU, memory, or custom metrics.
•	Elastic Load Balancer (ELB): Distributes traffic across instances to balance load.
•	CloudWatch Alarms: Used to trigger scaling actions.
•	ECS with Fargate or EC2: Task-based scaling based on request load or queue depth.

#### Difference between ALB and NLB
Answer:
Feature	ALB (Application Load Balancer)	NLB (Network Load Balancer)
Layer	Layer7 (HTTP/HTTPS)	Layer4 (TCP/UDP)
Features	Path-based, host-based routing	Fast TCP handling, static IP
Use Case	Web apps, HTTP APIs	Low latency apps, real-time systems

#### How do you connect to a private subnet?
Answer:
•	Use a bastion host (jump box) in the public subnet.
•	Or use Session Manager (SSM) if agents are installed.
•	Optionally use a VPN or Direct Connect.

#### AWS Lambda vs AWS Fargate
Answer:
Feature	AWS Lambda	AWS Fargate
Type	Serverless functions	Serverless containers
Use Case	Short event-driven tasks	Long-running container apps
Timeout	Max15 minutes	No hard timeout
Pricing	Per request + duration	Based on vCPU and memory used

#### What is OAI (Origin Access Identity) in CloudFront?
Answer:
OAI is used to restrict access to an S3 bucket so only CloudFront can fetch content, preventing direct access via S3 URL.

#### What is SNS and how do you create it?
A:
SNS (Simple Notification Service) is an AWS service used to send notifications via email, SMS, HTTP, or Lambda.
To create it:

Go to AWS SNS in the console.
Click “Create topic” → Choose Standard or FIFO.
Name the topic and click Create.
To add a subscriber, choose the topic → Create subscription → select protocol (e.g., Email) and provide the endpoint.

#### How can you recover a deleted S3 object?
Q: How do you restore a deleted S3 object?
A: If versioning was enabled, I can retrieve the deleted object using a previous version. Without versioning, the object is permanently deleted unless S3 backup (e.g., replication or lifecycle rule to Glacier) is configured.

#### How do you differentiate between a public and a private subnet in AWS?
Q: How do you identify a public vs private subnet?
A: A public subnet has a route to the internet via an internet gateway (IGW). A private subnet lacks this route and usually uses a NAT gateway for internet access. I verify this by checking route tables.

#### How do you secure a 3-tier architecture?
Q: How do you secure a 3-tier app (web, app, DB)?
A: I use security groups and NACLs to isolate layers:
•	Web tier: Public subnet with limited inbound (HTTP/HTTPS).
•	App tier: Private subnet, allows traffic only from web tier.
•	DB tier: Private subnet, accessible only by app tier.
Enable encryption (TLS, KMS), IAM roles, and monitoring (CloudWatch, GuardDuty).

#### How many VPCs can you create per region?
Answer:
By default, you can create 5 VPCs per region per AWS account. This limit can be increased by requesting a quota increase from AWS.

#### What is the difference between a private and public subnet?
Answer:
•	Public Subnet: A subnet that is associated with a route table that has a route to an Internet Gateway (IGW). Resources in this subnet can access the internet.
•	Private Subnet: A subnet that does not have a route to the Internet Gateway. Used for internal resources like databases.

#### What is a Transit Gateway?
Answer:
An AWS Transit Gateway enables you to connect multiple VPCs and on-premises networks through a central hub, simplifying your network architecture and reducing the number of peering connections.

#### What is VPC Peering?
Answer:
VPC Peering allows direct communication between two VPCs in the same or different AWS accounts/regions. It’s non-transitive and is used for point-to-point connectivity.

#### What is a VPC Endpoint?
Answer:
A VPC Endpoint allows private connection between your VPC and AWS services (like S3, DynamoDB) without using the internet, improving security and performance.

#### How can you restore an RDS snapshot with a custom database name?
Answer:
You cannot directly rename a database when restoring a snapshot. Instead:
#### 1.	Restore the snapshot.
#### 2.	Create a new DB instance.
#### 3.	Use tools like pg_dump/mysqldump, or AWS DMS to export and import data into a DB with the desired name.

#### How can you connect S3 to an EC2 instance?
Answer:
•	Attach an IAM Role to EC2 with S3 access permissions (e.g., AmazonS3ReadOnlyAccess).
•	Use AWS CLI or SDK on EC2:
aws s3 ls s3://your-bucket-name

#### What is the difference between Security Group and Network ACL (NACL)?
Answer:
Feature	Security Group	NACL
Level	Instance-level	Subnet-level
Stateful	Yes	No
Rules	Allow only	Allow and Deny
Applies to	EC2 Instances	Subnets
Default Behavior	Deny all unless allowed	Allow all unless changed

#### What are EC2 instance types?
Answer:
EC2 instances are categorized based on their hardware capabilities and use cases. Below is a corrected and properly aligned table:

Instance      Series	Type	                 Use Case	                             Examples
t-series	Burstable general purpose	   Low-cost, spiky workloads	         t2.micro, t3.small
m-series	General purpose	               Balanced compute, memory, network	 m5.large, m6g.medium
c-series	Compute optimized	           High-performance compute workloads	 c5.large, c6g.xlarge
r-series	Memory optimized	           In-memory databases, caching	         r5.large, r6g.xlarge
i-series	Storage optimized	           High IOPS storage workloads	         i3.large, i4i.xlarge
g/p-series	GPU/Accelerated computing	   ML, AI, video processing	             g4dn.xlarge, p3.2xlarge

#### What is nslookup?
- nslookup is a command-line tool to query DNS for domain or IP info, older than dig. Example: nslookup google.com.

#### What is dnslookup?
- No such standard command. Usually means performing DNS lookup using tools like nslookup or dig.

#### Inode of Lambda function?
- AWS Lambda functions don’t have inodes. Inodes are filesystem metadata; Lambda runs serverless, so no direct inode.

#### CloudWatch vs CloudTrail comparison:
| Feature          | CloudWatch                  | CloudTrail                       |
|------------------|-----------------------------|----------------------------------|
| Purpose          | Monitoring & metrics        | API call logging & auditing      |
| Data Type        | Metrics, logs               | Management API event logs        |
| Use Case         | Performance & alerts        | Security, compliance, auditing   |
| Real-time?       | Yes                         | No                               |
| Retention        | Configurable                | 90 days default + S3 archival    |

