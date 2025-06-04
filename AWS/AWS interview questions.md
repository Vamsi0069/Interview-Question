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
ssh -i key.pem ec2-user@<bastion-ip> → ssh into private IP

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

{
  "Effect": "Allow",
  "Principal": "*",
  "Action": ["s3:GetObject"],
  "Resource": "arn:aws:s3:::mybucket/*"
}
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
