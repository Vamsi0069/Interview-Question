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


