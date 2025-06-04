Explain what is NAT gateway? And what is the use of it? 

What is the use of cloud trail and how you manage in your project?

Suppose your team member is accessing the AWS service and you want them to not to access? What can be done? 

Difference between NAT gateway and internet gateway? 

You are managing the EKS service and suddenly you see that all the pods go offline? Where do you check the logs for it? 

Someone accidentally deleted the IPs in EKS and where can you see it in Aws?

What can be checked when the API server is down? But the instances are still running

How do you connect from public subnet to a private subnet? 

Softskills:

Suppose you are a team lead and your team member is having some issue and he had all the resources to check but still reaches you for the solution? How will you respond?

What do you expect from a Team Lead in your project?

How will you handle the production issue?

Why do we use a private subnet in a VPC?

For enhanced security. Resources (like databases) in private subnets are not accessible from the internet.

#### How many AWS accounts are used in your current project?

Usually at least 3: Dev, Staging, Prod. Sometimes separate accounts for Security, Billing, or Sandbox.

#### How can you download files from an S3 bucket within AWS while keeping public access completely blocked?

Use pre-signed URLs, IAM roles, or VPC endpoints to download securely without public access.

#### How do you monitor your applications and EC2 instances using Amazon CloudWatch?

Use CloudWatch metrics, custom dashboards, alarms, and logs agents to monitor CPU, memory, disk, logs, and custom app metrics.

#### Have you worked with AWS Transit Gateway in your project?

Transit Gateway helps in centralized VPC-to-VPC and on-prem connectivity across accounts. It simplifies the network mesh.


