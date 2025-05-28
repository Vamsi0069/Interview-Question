#### 1.Q: Have you ever come across a situation where a process in Linux is automatically killed?

Yes, this typically happens when the system runs out of memory. The OOM Killer (Out Of Memory Killer) in Linux terminates processes to free up RAM. You can check this using:
dmesg | grep -i "killed process"

#### 2.Q:  Write code for CPU and RAM utilization using Python?

```python
import psutil
# CPU usage
cpu_percent = psutil.cpu_percent(interval=1)
print(f"CPU Usage: {cpu_percent}%")
# RAM usage
memory = psutil.virtual_memory()
print(f"RAM Usage: {memory.percent}%")
```

#### 3.Q: How do you read and write key-value pairs from a text.json file in Python?

```python
import json
#### Read JSON file
with open('text.json', 'r') as f:
    data = json.load(f)
#### Access key-value pairs
for key, value in data.items():
    print(f"{key}: {value}")
#### Modify or add a new key-value pair
data['new_key'] = 'new_value'
#### Write back to the file
with open('text.json', 'w') as f:
    json.dump(data, f, indent= 4)
```    
#### 4.Q: How can you create user groups in Linux?

#### Create a group
sudo groupadd devteam

#### Add user to group
sudo usermod -aG devteam username

####Verify
groups username

#### 5.Q: How do you create an RDS instance in a specific VPC using Terraform? 
```
resource "aws_db_instance" "example" {
  identifier         = "mydb"
  engine             = "mysql"
  instance_class     = "db.t3.micro"
  allocated_storage  = 20
  name               = "mydb"
  username           = "admin"
  password           = "admin123"
  vpc_security_group_ids = [aws_security_group.rds_sg.id]
  db_subnet_group_name   = aws_db_subnet_group.example.name
  skip_final_snapshot    = true
}
resource "aws_db_subnet_group" "example" {
  name       = "my-subnet-group"
  subnet_ids = [aws_subnet.subnet1.id, aws_subnet.subnet2.id]
}
```
#### 6Q: How to configure multi-region failover if one region goes down?
A: Use Route 53 with failover routing policy and health checks. Primary region serves traffic; if it fails, Route 53 redirects to the secondary region.


#### 7Q: How to enable monitoring for multiple AWS services?
A: Use CloudWatch:
- Metrics: auto-collected
- Logs: via agents or SDK
- Alarms: for thresholds
- Dashboards: for visualization
- Events/EventBridge: for automation


#### 8Q: How to monitor and track cost by environment?
A:
- Tag all resources with Environment=sbox/uat/prod
- Use CloudWatch filters and dashboards per tag
- In Cost Explorer, filter by tags
- Enable Cost Allocation Tags and use AWS Budgets


#### 9Q: A public Lambda needs to access a service in a private subnet. How?
A:
- Attach Lambda to VPC with private subnets
- Ensure security group rules allow traffic
- Lambda uses ENI to talk privately inside VPC

#### 10Q: What is AssumeRole in AWS?
A:
AssumeRole lets a user or service temporarily get permissions of another IAM role using STS, ideal for cross-account access or secure delegation.

#### 11Q: Bash script to find error logs in log files?
A:
#!/bin/bash
LOG_DIR="/path/to/logs"
PATTERN="error"
grep -iH "$PATTERN" "$LOG_DIR"/*.log


#### 12Q. Why is curl -k https://<api-server>:6443 not working?
Answer:
Possible reasons:
API server is down or not reachable
Port 6443 is blocked by firewall
TLS certificate mismatch (use -k to ignore)
IP or DNS name incorrect

#### 13Q. What is ELK Stack?
Answer:

The ELK Stack is a powerful log aggregation and analytics platform composed of three main open-source components:

| **Component**     | **Function**                                                                           |
| ----------------- | -------------------------------------------------------------------------------------- |
| **Elasticsearch** | A distributed, RESTful search and analytics engine that stores and indexes logs.       |
| **Logstash**      | A data processing pipeline that collects, filters, and forwards logs to Elasticsearch. |
| **Kibana**        | A web-based UI to visualize and explore data stored in Elasticsearch.                  |

ELK is used for centralized logging, monitoring, and visualizing logs from servers, applications, containers, and cloud infrastructure.

#### 14Q. Script for Fetching the Memory Utilization of 2 Servers
Answer:

Here is a simple Bash script using ssh to fetch memory utilization from two remote Linux servers:

#!/bin/bash

**# List of servers (replace with actual IPs or hostnames)**
servers=("server1.example.com" "server2.example.com")

**# Loop through each server**

for server in "${servers[@]}"; do
  echo "----- Memory usage on $server -----"
  ssh user@$server free -h
  echo ""
done

**How it works:**

Uses ssh to connect to each server.

Runs free -h to display memory usage in a human-readable format.

Prints the output with a header for each server.

**Prerequisites:**

Passwordless SSH access (using SSH keys) must be set up.

Replace user with your actual username and hostnames accordingly.

#### 15Q. You have static & dynamic web apps using high EC2 + NGINX, causing high cost & low availability. What's your solution?
Answer:

Use S3 + CloudFront for static content.
Run dynamic apps on ECS/EKS/Fargate with auto-scaling.
Replace high EC2s with smaller instances in ASG.
Use ALB instead of standalone NGINX.
Containerize the app for better resource usage.

Result: Lower cost, high availability, easier management.

#### 16Q. One out of 10 microservices in Kubernetes is down. How do you fix it?
Answer:

Run kubectl get pods to find the failed pod.
Use kubectl logs and describe to diagnose.
Check for resource issues or crash errors.
Restart with kubectl rollout restart.
Roll back if a new image/code caused the issue.
Ensure HPA and probes are correctly set.

Result: Service is restored quickly, root cause identified.

#### 17Q. A client wants to implement a new system in 3 months, but your analysis shows it will take 6 months. How would you handle this situation?
Answer:

Communicate Transparently
Explain the findings and timeline based on a clear scope, technical complexity, and resource availability.

Break Down the Project
Propose a phased approach: deliver core features in 3 months, with additional phases after that.

Explore Alternatives
Identify options to accelerate delivery—like increasing the team size, reducing scope (MVP), or using pre-built solutions.

Provide Evidence
Share data from similar past projects, effort estimates, and risk assessments to support your timeline.

Goal: Align client expectations with reality while still showing flexibility and commitment to delivery.

#### 18Q. How would you assess whether an AI implementation would be beneficial for a specific business process?
Answer:

Understand the Process
Evaluate if the process is data-driven, repetitive, and can benefit from pattern recognition or prediction.

Identify Pain Points
Look for inefficiencies, manual work, or high error rates that AI can solve (e.g., forecasting, automation, classification).

Check Data Availability
Confirm if there's sufficient, clean, and labeled data to train AI models.

Estimate ROI
Compare AI implementation costs vs. potential benefits (time saved, error reduction, better decisions).

Pilot First
Propose a small-scale proof of concept (PoC) to validate feasibility and effectiveness.

Goal: Ensure AI adds real value, is technically feasible, and aligns with business goals.

#### 19Q: What is AWS Fargate and how does it differ from EC2-based ECS?
A:
AWS Fargate is a serverless compute engine for containers that works with ECS and EKS.
Key Differences from EC2-based ECS:
•	Fargate: No need to provision or manage EC2 instances.
•	EC2: You manage the EC2 infrastructure, networking, patching, etc.
Use case: When you want to run containers without managing the underlying infrastructure.

#### 20Q: What is AWS Glue and what is it used for?
A:
AWS Glue is a fully managed ETL (Extract, Transform, Load) service used for:
•	Discovering, cataloging, and transforming data.
•	Preparing data for analytics and machine learning.
Components:
•	Glue Crawlers – automatically detect schema and create metadata tables.
•	Glue Jobs – run PySpark or Python scripts for ETL.
•	Glue Data Catalog – central metadata repository.

#### 21Q: What is AWS EventBridge Scheduler and how is it used?
A:
Amazon EventBridge Scheduler (formerly CloudWatch Events Scheduler) is a fully managed scheduler for running tasks or workflows at defined times or intervals.

**Use cases:**

•	Schedule Lambda functions or Step Functions.

•	Start/stop EC2 or RDS instances.

•	Trigger Glue jobs or ECS tasks periodically.

**Example:**
```
{
  "ScheduleExpression": "rate(5 minutes)",
  "Target": {
    "Arn": "arn:aws:lambda:region:account-id:function:MyFunction"
  }
}
```
#### 22Q. What is Amazon ECS (Elastic Container Service)?
A:
Amazon ECS is a fully managed container orchestration service that lets you run and scale Docker containers.

**Key Concepts:**

•	Cluster – logical grouping of tasks or services.

•	Task Definition – blueprint for your container (image, CPU, memory, etc.).

•	Services – keep tasks running, support scaling and load balancing.

**Modes:**

•	EC2 launch type

•	Fargate launch type (serverless)

#### 23Q: What is Amazon ECR (Elastic Container Registry)?
A:
Amazon ECR is a fully managed Docker container registry that makes it easy to store, manage, and deploy container images.

**Features:**

•	Integrated with ECS, EKS, and Lambda.

•	Secure access via IAM.

•	Supports image versioning and scanning.

**Common Commands:**

**#### Authenticate Docker to ECR**

aws ecr get-login-password | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com

**#### Push Docker image**

docker build -t my-image .

docker tag my-image:latest <account_id>.dkr.ecr.<region>.amazonaws.com/my-image:latest

docker push <account_id>.dkr.ecr.<region>.amazonaws.com/my-image:latest

**#### 24Q. Do you know about UAT?**

Answer:
Yes. User Acceptance Testing is the final testing phase where end-users validate the software before going to production.

#### 25Q. What is Minikube?
Answer:
Minikube runs a local Kubernetes cluster inside a VM or container for learning and development purposes.

#### 26Q. If storage is full on a Linux server, how do you manage it?
Q: What steps do you take when a Linux server runs out of disk space?
A: I check disk usage with df -h and identify large files or directories using du -sh * | sort -h. I clear logs (/var/log), old Docker images (docker image prune), and cached packages (apt clean, yum clean all). If necessary, I increase the volume size or mount additional storage.

#### 27Q. If you are getting an SSH error, what do you do?
Q: How do you troubleshoot SSH connection issues?
A: I check network reachability using ping or telnet, confirm correct IP, key permissions (chmod 400 for PEM), verify SSH service status, and review /var/log/auth.log or /var/log/secure. I also confirm the security group/firewall allows port 22.

#### 28Q. What is a CRD (Custom Resource Definition)?
Answer:
A CRD lets you define a custom resource (e.g., MySQLCluster) and use it like a native Kubernetes object. It extends Kubernetes capabilities without modifying the core.

#### 29Q. What are common Kubernetes Operations (Day2 Ops)?
Answer:
•	Scaling workloads

•	Monitoring & logging

•	Rolling updates

•	Backup & restore (etcd, volumes)

•	Debugging pods

•	Resource limits and quota management

#### 30Q. What is a Service Mesh?
Answer:
A Service Mesh manages communication between services. Features:

•	Traffic management

•	Security (mTLS)

•	Observability (metrics, tracing)

**Examples:** Istio, Linkerd

#### 31Q. What is Sidecar Injection?
Answer:
Sidecar injection is the process of automatically adding a sidecar container (like an Envoy proxy) to pods. This is used in service meshes (e.g., Istio) for traffic interception.

#### 32Q. What is Envoy Proxy?
Answer:
Envoy is a high-performance proxy used in service meshes (e.g., Istio) for:

•	Load balancing

•	Traffic routing

•	TLS termination

•	Observability

#### 33Q. What is a Pod Disruption Budget (PDB)?
Answer:
PDB ensures a minimum number of pods are always available during voluntary disruptions (like node drain). You can define:

•	minAvailable

•	maxUnavailable

#### 34Q. What are Probes in Kubernetes?
Answer:
Used to check pod health:

•	Liveness Probe: Restarts container if it's stuck.

•	Readiness Probe: Controls pod availability to services.

•	Startup Probe: For slow-starting apps.

#### 35Q. What is the difference between Voluntary and Involuntary Disruption?
Answer:
•	Voluntary: Triggered by user (e.g., kubectl drain, rolling update).

•	Involuntary: System-triggered (e.g., node crash, OOM).

#### 36Q. What is Safe Eviction vs Hard Eviction?
Answer:
•	Safe Eviction: Graceful shutdown respecting Pod Disruption Budgets and lifecycle hooks.

•	Hard Eviction: Forced eviction due to resource pressure or system errors.

#### 37Q. What are Pod Security Policies (PSP)?
Answer:
(Deprecated in Kubernetes v1.25)
PSPs controlled security-related settings like:

•	Privileged mode

•	Host namespaces

•	Volume types

•	User IDs

Use Pod Security Admission (PSA) instead in newer versions.

#### 38Q. What is CrashLoopBackOff error?
Answer:
This occurs when a container keeps crashing repeatedly. Causes:

•	Application error

•	Misconfiguration

•	Unavailable dependencies

**Fix:**

•	Check logs, describe pod, check readiness/liveness probes.

#### 39Q.what is step functions in aws?
A:AWS Step Functions is a service designed to help you orchestrate workflows in a serverless manner. It visualizes these workflows as state machines, which are a series 
of steps that your application follows.  These steps can involve calling other AWS services, or your own custom code.

Here are some of the key benefits of using Step Functions:

**Visual Workflows:** Step Functions provides a drag-and-drop interface to define your workflows, making it easier to design and understand complex processes.
**Simplified Automation:** You can automate workflows across a variety of AWS services without having to write and maintain a lot of custom code.
**Error Handling and Resilience:** Step Functions includes built-in error handling, retries, and timeouts to make your workflows more robust.
**Event-Driven Architecture:** Step Functions integrates well with event-driven architectures, allowing you to define workflows that are triggered by events.
Overall, Step Functions is a powerful tool that can help you build and manage complex workflows in the AWS cloud.

