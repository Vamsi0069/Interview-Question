#### 1Q. What are your day-to-day activities as a DevOps/Kubernetes engineer?
Answer:

In my day to day activities , I will handle multiple tasks:
My day starts with a team meeting called a "scrum call." 
In this meeting, we discuss our progress, and the team assigns tasks for the day.
After the meeting, I check Jenkins, a tool we use for automation. 
I look for any new builds or updates. If there are errors in the builds, I work on fixing them.
Kubernetes Monitoring: If Jenkins is running smoothly, 
I move on to checking our Kubernetes (K8s) clusters and pods. 
These are parts of our system where applications run. 
If any pod has issues or isn’t working correctly, 
I troubleshoot the problem, fix it, and inform my team about the updates.
Sometimes, I create Kubernetes configuration files called manifest files. 
These define how our applications and services should run in the Kubernetes environment.
We have distributed team,where cloud team will ping me,for some cloud tasks like 
creating ebs,s3,iam roles and vpc related issues.

#### 2Q. Any issues in your project?
Answer:

High CPU/memory usage in some pods.
Disk space issues in nodes.
Stuck pods due to PVC binding problems.
API server latency under high load.
Ingress routing misconfigurations.


#### 3Q: What is blue-green deployment?
A: It’s a release strategy with two environments (Blue & Green). Blue is live, Green is the new version. After testing, switch traffic to Green. Rollback is easy by switching back to Blue.

#### 4Q. GitLab vs Jenkins (Table Format)
-----------------------------------------------------------------------------------------------------------
| Feature               | GitLab CI/CD                         | Jenkins                                  |
|-----------------------|--------------------------------------|------------------------------------------|
| Setup                 | Pre-integrated, simple               | Manual setup with plugins                |
| Pipeline Definition   | .gitlab-ci.yml (YAML)                | Jenkinsfile (Groovy DSL)                 |
| Scalability           | Runner-based, auto-scaled            | Master-agent model                       |
| Maintenance           | Less, managed by GitLab              | High, manual plugins/updates             |
| Integration           | Native Git integration               | Any SCM via plugins                      |
| UI                    | Modern & built-in                    | Plugin-dependent                         |
| Cost                  | Free & paid tiers                    | Open-source, infra cost                  |
-----------------------------------------------------------------------------------------------------------
#### 5Q: Key difference when configuring a new CI/CD pipeline?
A:
- Jenkins: Customizable but manual setup (Jenkinsfile, agents, plugins).
- GitLab: Easy, tightly integrated with Git, uses .gitlab-ci.yml.

#### 6Q. NAT Instance vs NAT Gateway (Table Format)
-----------------------------------------------------------------------------------------------------
| Feature          | NAT Instance                          | NAT Gateway                            |
|------------------|---------------------------------------|----------------------------------------|
| Type             | EC2-based, user-managed               | AWS-managed service                    |
| High Availability| Manual setup                          | Built-in multi-AZ                      |
| Performance      | Depends on EC2 size                   | Scalable & high throughput             |
| Cost             | Cheaper for low traffic               | More expensive, better for production  |
| Maintenance      | Manual updates                        | No maintenance                         |
-----------------------------------------------------------------------------------------------------


#### 7Q: How should developers write code to access AWS services (S3, SQS, SNS, RDS)?
A:
- Use AWS SDKs
- Access via IAM roles with least privilege
- Store secrets in Secrets Manager
- Follow retry/backoff logic
- Use env variables for config
- Log via CloudWatch

#### 8Q: How to handle developer authentication to AWS?
A:
- Use IAM users/groups with MFA
- Prefer IAM roles with temporary STS credentials
- Use AWS SSO or federation
- Manage secrets via AWS Vault or CLI profiles
- Rotate creds, no hardcoding

#### 9Q: How to enable communication between different VPCs?
A:
- VPC Peering: Direct, simple, but no transitive routing
- Transit Gateway: Centralized hub, scalable
- PrivateLink: For exposing services, not full VPC access
- VPN: For secure cross-region or hybrid setups


#### 10Q. What happens if the kubelet goes down?
Answer:
If the kubelet on a node goes down:
The node stops reporting to the Kubernetes control plane.
After a default period (usually 5 minutes), the node is marked NotReady.
The scheduler may reschedule the pods on other healthy nodes (if they are not static pods or daemonsets).

#### 11Q. What is a static pod in Kubernetes?
Answer:
A static pod is managed directly by the kubelet on a node, not through the Kubernetes API server.
Defined in a local manifest file (e.g., /etc/kubernetes/manifests/).
Used for critical components like control plane pods.
Cannot be managed with kubectl.

#### 12Q. What is the role of the Kubernetes scheduler?
Answer:
The Kubernetes scheduler assigns newly created pods to nodes based on:
Resource availability
Node affinity/anti-affinity
Taints and tolerations
Custom scheduling rules

#### 13Q. Can the scheduler schedule the API server as well?
Answer:
No, the API server and other control plane components are usually run as static pods, which are not scheduled by the Kubernetes scheduler.

#### 14Q. Compare Deployment vs. StatefulSet
----------------------------------------------------------------------------------------------------------------------------
| **Feature**           | **Deployment**                                  | **StatefulSet**                                |
| --------------------- | ----------------------------------------------- | ---------------------------------------------- |
| **Use Case**          | Stateless applications (e.g., web servers)      | Stateful applications (e.g., databases)        |
| **Pod Identity**      | Pods are interchangeable, no fixed identity     | Each Pod has a stable, unique identity         |
| **Pod Name**          | Random suffix (e.g., `nginx-xyz12`)             | Predictable names (e.g., `db-0`, `db-1`)       |
| **Storage (Volumes)** | Shared or ephemeral                             | Each Pod gets its own persistent volume        |
| **Scaling**           | Easy and fast; Pods treated equally             | Slower, one Pod updated at a time              |
| **Network Identity**  | No stable DNS for individual Pods               | Each Pod gets a stable DNS hostname            |
| **Pod Ordering**      | No guaranteed order for startup/termination     | Follows   ordered deployment and terminationb  |
| **Use With**          | Frontend apps, APIs, microservices              | Databases (e.g., MySQL, Cassandra, Kafka)      |
----------------------------------------------------------------------------------------------------------------------------

#### 15Q. What are Kubernetes Services?
Answer:
A Kubernetes Service provides a stable network endpoint to access a set of pods.
Types: ClusterIP, NodePort, LoadBalancer, ExternalName
Helps decouple frontends from backends
Uses labels/selectors to route traffic to the correct pods

#### 16Q. What is Horizontal Pod Autoscaler (HPA) in Kubernetes?
Answer:
HPA automatically scales the number of pods in a Deployment or ReplicaSet based on metrics like CPU usage or custom metrics.

#### 17Q. HPA vs VPA in Kubernetes

| **Aspect**            | **HPA (Horizontal Pod Autoscaler)**                   | **VPA (Vertical Pod Autoscaler)**                        |
| --------------------- | ----------------------------------------------------- | -------------------------------------------------------- |
| **Scaling Direction** | **Horizontal** – scales the number of Pods            | **Vertical** – adjusts CPU/Memory of a single Pod        |
| **Purpose**           | Handle increased load by adding/removing Pods         | Optimize resource usage within Pods                      |
| **Metrics Used**      | CPU, memory, or custom metrics (via Metrics Server)   | Historical usage and live metrics                        |
| **Pod Restart**       | No restart (new Pods added/removed)                   | **Pod restart required** to apply new resource values    |
| **Best Use Case**     | Stateless apps with variable load (e.g., web servers) | Long-running apps with consistent load patterns          |
| **Kubernetes Object** | `HorizontalPodAutoscaler` resource                    | `VerticalPodAutoscaler` resource                         |
| **Limitations**       | Can’t optimize within a Pod                           | Not suitable for rapid load spikes or multi-replica apps |

#### 18Q. What is a DaemonSet in Kubernetes?
Answer:
A DaemonSet ensures that a specific pod runs on all (or selected) nodes in the cluster.
Examples: log collection, monitoring agents, network plugins.

#### 19Q. Compare Liveness Probe vs. Readiness Probe
Probe Type	Purpose	Effect on Pod
Liveness Probe	Checks if container is alive	Pod is restarted if it fails
Readiness Probe	Checks if container is ready to serve	Pod is removed from service endpoints

#### 20Q. How do you perform a Kubernetes cluster upgrade?
Answer:
Drain nodes: kubectl drain <node-name>
Upgrade kubeadm: apt upgrade kubeadm
Run kubeadm upgrade plan and kubeadm upgrade apply
Upgrade kubelet and kubectl
Restart kubelet
Uncordon nodes: kubectl uncordon <node-name>

#### 21Q. If a pod has three containers and one container is unhealthy (liveness probe fails), what happens?
Answer:
Only the unhealthy container is restarted by kubelet. The other two containers continue to run unaffected.

#### 22Q. If the kubectl command is not working, how do you troubleshoot it?
Answer:

Check ~/.kube/config
Validate context: kubectl config get-contexts
Check connectivity to API server
Run kubectl version
Use curl or telnet to test API server reachability
#### 23Q. In a Kubernetes Deployment using a PVC, where a pod is using the PVC, what happens to the pod if someone deletes the Deployment?
Answer:

Pods created by the Deployment will be deleted.
PVC is not deleted (unless manually configured via ReclaimPolicy).
The underlying PersistentVolume may remain, depending on the reclaim policy.

#### 24Q. Compare node affinity vs. node selector
Feature	Node Affinity	Node Selector
Flexibility	More expressive	Simple key-value match
Operators	In, NotIn, Exists, etc.	Only exact match
Scheduling type	Preferred/Required	Required
Use case	Advanced scheduling requirements	Basic filtering

#### 25Q. What is the Terraform directory structure?
Answer:

```
project/
├── main.tf        # Main configuration file
├── variables.tf   # Input variables
├── outputs.tf     # Output values
├── terraform.tfvars # Actual variable values
├── backend.tf     # Backend config for remote state
├── modules/       # Reusable modules
│   └── <module_name>/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── envs/          # Environment-specific configs
    ├── dev/
    ├── prod/
```

#### 26Q. How do you connect one EC2 instance to another across multiple regions?
Answer:
You can connect EC2 instances across regions by:
Setting up a VPN connection between VPCs in different regions.
Using VPC Peering (now supported cross-region).
Using AWS Transit Gateway for more complex architectures.

#### 27Q. What is an Internet Gateway, and where is it placed?
Answer:
An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet.
It is attached at the VPC level, not the subnet level.

#### 28Q. What is VPC Peering, and how do you configure it?
Answer:
VPC Peering connects two VPCs to route traffic using private IPs.
Steps to configure:
Create a VPC peering connection.
Accept the request from the target VPC.
Add route table entries in both VPCs.
Ensure security groups and NACLs allow traffic.

#### 29Q. How do you give private access to an S3 bucket?
Answer:

Remove public access settings.
Attach bucket policy that allows access from specific VPC or IAM roles.
Use VPC endpoint for S3 for private access without using the internet.

#### 30Q. What is CloudFront?
Answer:
CloudFront is AWS’s Content Delivery Network (CDN) that caches content at edge locations to reduce latency and speed up delivery.

#### 31Q. What is a NAT Gateway, and where do you place it?
Answer:
A NAT Gateway enables instances in a private subnet to access the internet (for updates, etc.) while remaining unreachable from the outside.
It is placed in a public subnet and requires a route from private subnets to the NAT Gateway.

#### 32Q. What is the Terraform state file? Where and how do you store it to avoid conflicts?
Answer:
The terraform.tfstate file stores the current state of your infrastructure.
To avoid conflicts:

Use remote state backends (e.g., S3 with DynamoDB locking).

Example:

backend "s3" {
  bucket         = "my-terraform-state"
  key            = "env/dev/terraform.tfstate"
  region         = "us-west-2"
  dynamodb_table = "terraform-lock"
}

#### 33Q. A client asks you to provision infrastructure with EC2, S3 bucket, and VPC. Write the Terraform script
Answer:

provider "aws" {
  region = "us-west-2"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_s3_bucket" "bucket" {
  bucket = "my-unique-bucket-name-123"
  acl    = "private"
}

resource "aws_instance" "web" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.main.id
}

resource "aws_subnet" "main" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "us-west-2a"
}
#### 34Q. What is the difference between Terraform and Ansible?
Feature                  	Terraform	                                       Ansible
Purpose	             Infrastructure provisioning (IaC)	               Configuration management
Language	           Declarative (HCL)	                               Procedural (YAML + Python)
Idempotency	         Built-in	                                         Manual in some cases
Agent-based	         Agentless	                                       Agentless
Execution	           Plans infra before applying changes	             Executes tasks immediately

#### 35Q. How do you back up data in Docker? What are the different ways?
Answer:

Use Docker volumes: back up by copying data from /var/lib/docker/volumes/.
Use docker cp to copy data from containers.
Mount volume and copy data manually.

#### 36Q. Write a Dockerfile for a Python application
dockerfile

FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]

#### 37Q. What is the difference between ADD and ENTRYPOINT?
| **Instruction**  | **Purpose**                                          | **Functionality**                                       |
| ---------------- | ---------------------------------------------------- | ------------------------------------------------------- |
| **`ADD`**        | Copies files/directories into the image              | Also supports remote URLs and auto-extracts tar files   |
| **`ENTRYPOINT`** | Defines the **main command** to run in the container | Makes the container behave like a standalone executable |

#### 38Q. What is the command to delete all containers with a single command?

docker rm -f $(docker ps -aq)

#### 39Q. What is Persistent Storage, PV (Persistent Volume), and PVC (Persistent Volume Claim)?
Answer:

Persistent Storage: storage that outlives pod lifecycles.
PV: cluster-managed storage resource.
PVC: user request for storage; binds to a PV.

#### 40Q. Do you handle production support issues?
Answer:
Yes. This involves:
Monitoring logs with kubectl logs
Restarting failed pods
Analyzing node/pod health
Investigating metrics and alerts from tools like Prometheus/Grafana

#### 41Q. What Nagios plugins have you used?
Answer:

check_http – for web server health
check_disk – for disk space monitoring
check_load – for CPU load
check_ping – for network availability

#### 42Q. What are the default port numbers for Grafana and Prometheus?

|Tool	         |      Default Port     |
|----------------|-----------------------|
|Grafana         | 	3000             |
|Prometheus	 |       9090            |

#### 43Q. What is the main configuration file in Grafana?
Answer:
grafana.ini
Located by default at  /etc/grafana/grafana.ini

#### 44Q. What is the difference between a hard link and a soft link
Feature	                   Hard Link	                       Soft Link (Symbolic Link)
Inode sharing	        Shares the same inode	               Points to the original inode
Broken link	    Works even if original is deleted	       Breaks if the target is deleted
File systems	    Must be on the same file system	       Can link across file systems
Appearance	            File	                                     Shortcut
#### 45Q. Servers got rebooted → How do you tell why it got rebooted?
Answer:

Check system logs:

journalctl --since "1 hour ago" | grep -i shutdown
Inspect /var/log/messages or /var/log/syslog.
Use last reboot to see last reboot times.
Look for kernel panic, OOM errors, or manual reboots in logs.

#### 46Q. Process taking high CPU utilization – how to handle it?
Answer:

Use top, htop, or ps -eo pid,ppid,%cpu,cmd --sort=-%cpu to identify.
Check logs of the process/container.
Restart the pod/container if needed.
Add resource limits in Kubernetes to prevent abuse.

#### 47Q. How do you increase disk size?
Answer:

EC2 (AWS):
Modify volume from AWS Console.
Use lsblk to identify disk.
Resize partition with growpart.

Resize filesystem:
sudo resize2fs /dev/xvdf1
#### 48Q. How do you increase the space size in the same partition?
Answer:

Extend the disk at the cloud/VM level.
Use tools like growpart or parted to expand the partition.
Use resize2fs (for ext4) or xfs_growfs (for XFS) to resize the filesystem.


#### 49Q. Architecture of Kubernetes
Answer:

Master Components: API Server, Scheduler, Controller Manager, etcd.
Node Components: Kubelet, Kube-proxy, container runtime (Docker/CRI-O).
Add-ons: DNS, Dashboard, Ingress controller, etc.

#### 50Q. K8s Autoscaling
Answer:

Horizontal Pod Autoscaler (HPA): scales pods based on CPU/memory or custom metrics.
Vertical Pod Autoscaler (VPA): adjusts resource requests/limits of pods.
Cluster Autoscaler: adds/removes nodes based on pending pods.

#### 51Q. Difference between StatefulSet & Deployment
| **Feature**             | **Deployment**                                         | **StatefulSet**                                          |
| ----------------------- | ------------------------------------------------------ | -------------------------------------------------------- |
| **Use Case**            | Stateless applications (e.g., web servers, APIs)       | Stateful applications (e.g., databases, Kafka)           |
| **Pod Identity**        | Pods are **interchangeable**, no fixed identity        | Pods have **stable, unique identities**                  |
| **Pod Naming**          | Randomized (e.g., `web-abc123`)                        | Predictable (e.g., `db-0`, `db-1`)                       |
| **Storage**             | Shared or ephemeral storage                            | **Persistent Volume** per Pod (retained across restarts) |
| **DNS Hostnames**       | No stable DNS per Pod                                  | Each Pod gets a **stable network identity**              |
| **Startup/Termination** | All Pods start/stop in **parallel**                    | Ordered **start/stop** and **rolling updates**           |
| **Scaling**             | Fast and simple scaling                                | Slower, ordered scaling                                  |
| **Pod Replacement**     | New Pod is identical to old (no identity preservation) | Maintains **state and identity** even after restart      |


#### 52Q. What is the concept of Ingress in Kubernetes?
Answer:
Ingress is an API object that manages external HTTP/HTTPS access to services inside a Kubernetes cluster.
It allows path-based or host-based routing and works with Ingress controllers like NGINX, Traefik.

#### 53Q. What is a Namespace in K8s?
Answer:
A namespace is a logical isolation unit in Kubernetes used to divide cluster resources between multiple users or teams.
Useful in multi-tenant environments.

#### 54Q. Difference between ReplicaSet & DaemonSet
---------------------------------------------------------------------------------------------------------------------------------------
| **Feature**             |         **ReplicaSet**                         |          **DaemonSet**                                   |
| ------------------------| ---------------------------------------------- |--------------------------------------------------------- |
| **Purpose**             | Ensure a specific number of identical Pods are running| Ensure **one Pod per node** (or specific nodes)   |
| **Pod Count**           | Defined by user (e.g., 3 replicas)             | Automatically runs **one Pod on each node**              |
| **Use Case**            | Web servers,API services,stateless applications| Monitoring agents, log collectors, storage plugins       |
| **Scaling**             | Manual or via Horizontal Pod Autoscaler        | Scales automatically with node changes                   |
| **Pod Placement**       | Pods can run on **any nodes**                  | Exactly one per node(unless node selectors are used)     |
| **Update Strategy**     | Supports rolling updates via Deployment        | Uses `RollingUpdate` strategy for controlled rollout     |
| **Controller Used With**| Often used by **Deployment**                   | Managed directly as **DaemonSet**                        |
--------------------------------------------------------------------------------------------------------------------------------------

#### 55Q. Advantages of Kubernetes
Answer:

Automated scaling and self-healing.
Rolling updates and rollbacks.
Resource optimization.
Supports hybrid and multi-cloud environments.
Declarative configuration with YAML.

#### 56Q. What is Blue-Green Deployment in K8s?
Answer:
Blue-Green Deployment involves running two identical environments (blue and green).
Blue = current live
Green = new version
Switch traffic from blue to green once the green version is verified.

#### 57Q. What is DockerHub?
Answer:
DockerHub is a cloud-based registry to store and share container images.
You can pull official or custom images from it.

#### 58Q. 3 containers running – How to get into one container?
Answer:

docker exec -it <container_id_or_name> /bin/bash
Or use /bin/sh if bash is not available.

#### 59Q. Can I use JSON over YAML in Kubernetes?
Answer:
Yes, Kubernetes supports both YAML and JSON for manifests, but YAML is more human-readable and widely used.

#### 60Q. Difference between Docker Container & Kubernetes
| **Aspect**               | **Docker (Container)**                               | **Kubernetes**                                        |
| ------------------------ | ---------------------------------------------------- | ----------------------------------------------------- |
| **Primary Function**     | Containerization platform – builds & runs containers | Container orchestration – manages container clusters  |
| **Scope**                | Works on a single host                               | Manages multiple containers across multiple hosts     |
| **Container Lifecycle**  | Manual (start/stop/restart)                          | Automated (scheduling, self-healing, rolling updates) |
| **Scaling**              | Manual scaling                                       | Auto-scaling and load balancing                       |
| **Networking**           | Basic network configuration                          | Advanced service discovery, DNS, and network policies |
| **Storage**              | Local volumes                                        | Supports persistent volumes, dynamic provisioning     |
| **High Availability**    | Not built-in                                         | Built-in via replica management and node distribution |
| **Monitoring & Logging** | Basic logging                                        | Integrated with tools like Prometheus, Grafana, ELK   |
| **Multi-container apps** | Managed via `docker-compose`                         | Managed via Pods, Deployments, StatefulSets, etc.     |

#### 61Q. How will you monitor your Docker containers?
Answer:

Use docker stats for live metrics.
Use third-party tools like cAdvisor, Prometheus, and Grafana.
Integrate with logging tools like ELK or Fluentd.

#### 62Q. Hands-on with Grafana & Prometheus – what can you do?
Answer:

Install and configure Prometheus with K8s metrics.
Add Prometheus as a data source in Grafana.
Create dashboards to monitor CPU, memory, disk, pod health.
Set up alerts in Grafana.

#### 63Q. What is SonarQube?
Answer:
SonarQube is a tool used to analyze code quality, detect bugs, code smells, and security vulnerabilities in code repositories.
Integrates with CI/CD pipelines to enforce code standards.

#### 64Q. What is sed command used for, and what are its flags?
Answer:
sed is a stream editor used for text transformation, like find & replace.

Common flags:

-e : Add the script to the commands to be executed
-i : Edit files in-place
-n : Suppress default output (used with p for printing lines)
s : Substitute (e.g., sed 's/old/new/g' file.txt)

#### 65Q. What is awk command?
Answer:
awk is a powerful text processing tool used for pattern scanning and data extraction.
Example:

awk '{print $1, $3}' file.txt
Prints the 1st and 3rd columns of a file.

#### 66Q. How to find files in Linux?
Answer:

find /path -name "filename"
Examples:

By name: find . -name "*.log"
By size: find / -size +100M
Recently modified: find /var/log -mtime -1

#### 67Q. Java Spring Boot app deployment in Kubernetes
Answer:

Create Dockerfile for your app.
Build and push the image to a container registry.
Create K8s manifests:
Deployment
Service
(Optional) Ingress
Apply using:

kubectl apply -f deployment.yaml

#### 68Q. Can you explain path-based routing?
Answer:
In Kubernetes (via Ingress), path-based routing directs traffic based on the URL path.

Example:

- path: /api
  backend: serviceName: api-service
- path: /web
  backend: serviceName: web-service
  
#### 69Q. What are roles in Kubernetes?
Answer:
Roles define permissions within a namespace.
ClusterRoles apply across all namespaces.
Used in RBAC (Role-Based Access Control) to control what users or services can do.

#### 70Q. What are bucket policies in AWS S3?
Answer:
Bucket policies are JSON-based rules that control access to an entire S3 bucket or objects inside.

Example:

{
  "Effect": "Allow",
  "Principal": "*",
  "Action": ["s3:GetObject"],
  "Resource": "arn:aws:s3:::mybucket/*"
}
#### 71Q. If you get a 403 error (Access Denied) in S3, what do you check?
Answer:

Check bucket policy
Check IAM role/user permissions
Verify if object ACL is private
Ensure correct Region and signed URL, if applicable

#### 72Q. We check the policy: GetObject, PutObject — why?
Answer:
These are the S3 actions needed to download (GetObject) or upload (PutObject) files.
Without them, you’ll get 403 errors.

#### 73Q. What are objects in Kubernetes?
Answer:
Objects are persistent entities representing the desired state.
Examples: Pod, Service, Deployment, ConfigMap, Secret, Ingress.

#### 74Q. What is terraform taint?
Answer:
Marks a resource for destruction and recreation during the next apply.

terraform taint aws_instance.my_instance

#### 75Q. What is git merge and git rebase?
Command	                     Purpose
merge	             Combines two branches, creates a merge commit
rebase	             Reapplies commits from one branch onto another for a linear history

#### 76Q. What are branching strategies?
Answer:

GitFlow (feature, develop, release branches)
Trunk-Based (single main branch with feature flags)
GitHub Flow (short-lived feature branches + pull requests)

#### 77Q. What are issues you faced in production?
Answer:

Pod crash due to memory leaks.
PVC binding issues.
DNS resolution failure in K8s.
S3 permission errors (403).
Auto-scaling delay under heavy load.

#### 78Q. Which Linux distro did you use in your project?
Answer:
Mostly Amazon Linux 2, Ubuntu, or CentOS depending on the cloud provider and application needs.

#### 79Q. Which distro do you prefer and why?
Answer:
Ubuntu – due to:
Wide community support
Easy package management (apt)
Better documentation

#### 80Q. What are roles and templates in Ansible?
Answer:

Roles: Standardized way to organize playbooks into reusable components (tasks, handlers, vars, etc.)
Templates: Jinja2 files (.j2) used to dynamically generate configuration files.

#### 81Q. What is 2/2 check in AWS EC2?
Answer:

EC2 passes 2 checks:
System status check
Instance status check
Both must be "2/2 checks passed" for healthy status.

#### 82Q. How to create EKS Cluster with IAM Role?
Answer:

Create an IAM role with AmazonEKSClusterPolicy.
Use eksctl or Terraform to create the cluster:

eksctl create cluster --name demo --region us-west-2 --with-oidc

#### 83Q. Why did you use S3 in your project?
Answer:

Store logs, backups, artifacts.
Host static websites.
Use as Terraform backend (state file storage).

#### 84Q. What is a bucket policy in AWS?
Answer:
JSON document attached to a bucket to define access rules for users, roles, or the public.

#### 85Q. What is 403 error in S3 object permission?
Answer:
It means access denied. Possible causes:
Missing s3:GetObject permission
Object is private
Bucket policy restricts access

#### 86Q. What is state locking in Terraform?
Answer:
Prevents simultaneous changes to the same infrastructure by multiple users.
Uses DynamoDB table (in AWS) to manage locks.

#### 87Q. Where will you find lockingID in Terraform?
Answer:
In the DynamoDB table used for state locking.
Look for the item with LockID in the table where Terraform stores its locks.

#### 88Q. What are templates in Ansible and how do you define them?
Answer:

Files written using Jinja2 syntax.
Defined in playbooks like:

```yaml

tasks:
  - name: Apply nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
```	  
#### 89Q. What are Ingress and Egress in Kubernetes?
Answer:

Ingress: Rules to allow external traffic into the cluster.

Egress: Outbound traffic from pods to outside the cluster.

#### 90Q. Can we use ArgoCD without Helm?
Answer:
Yes 
ArgoCD supports:

Kustomize
Plain YAML
Helm
Jsonnet
Helm is optional.

#### 91Q: How do you create handlers in Ansible?
```
#### playbook.yml
tasks:
  - name: Install nginx
    apt:
      name: nginx
      state: present
    notify: restart nginx

handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```    
#### 92Q: What is the typical directory structure of an Ansible project?
```
project/
├── inventory
├── playbook.yml
├── roles/
│   └── webserver/
│       ├── tasks/
│       │   └── main.yml
│       ├── handlers/
│       │   └── main.yml
│       ├── templates/
│       ├── files/
│       ├── vars/
│       └── defaults/
```
#### 93.Q: How can you set up password-less authentication in Linux using SSH?
1.Generate SSH key
    ssh-keygen
    
2.Copy public key to remote server:
    ssh-copy-id user@remote-server
    
3. Now you can SSH without a password:
    ssh user@remote-server
   
#### 94.Q: What is SELinux and how does it work?
SELinux (Security-Enhanced Linux) is a security module in Linux that provides mandatory access control (MAC). It defines access policies for users, processes, and files.

Check status:
               Sestatus
Modes: 
Enforcing: Enforces policies.
Permissive: Logs violations, doesn’t enforce.
Disabled: Completely off.

#### 95.Q: What is AWS Serverless and what are its key benefits?

AWS Serverless refers to a cloud-native development model that allows you to build and run applications without managing servers. Key services include:

•	AWS Lambda – run code in response to events.

•	Amazon API Gateway – expose APIs.

•	Amazon DynamoDB – NoSQL database.

•	AWS Step Functions – orchestrate workflows

#### 96Q. What is the difference between Declarative and Scripted Pipeline in Jenkins?
Declarative Pipeline: Uses a more structured and predefined syntax. Easier to write and read, especially for beginners.
Scripted Pipeline: Uses Groovy-based syntax. Offers more flexibility and is suitable for complex logic.

#### 97Q. How can you upgrade Jenkins?
1, Backup Jenkins (Recommended) 
Backup your Jenkins home directory (usually /var/lib/jenkins):(sudo cp -r /var/lib/jenkins /var/lib/jenkins_backup)

2, Upgrade Jenkins (Using Package Manager)

sudo yum check-update

sudo yum upgrade jenkins

sudo systemctl restart jenkins

#### 98Q. What is Jenkins Master-Slave Architecture?
Master: The main Jenkins server that manages the overall environment — it schedules builds, handles the UI, and delegates tasks to agents.
Slave (Agent): Remote machines that connect to the master and run the actual build/test jobs. They can be Linux, Windows, or container-based systems.

#### How it works:
Master receives a trigger (e.g., code push).
Assigns the job to an available slave.
Slave executes the job and reports back the result.
Connection methods: SSH, JNLP, or WebSocket.

#### Why use it?
Load distribution
Run jobs in parallel
Use different environments for different jobs (e.g., Java on one, Python on another)


#### 99Q. What is SonarQube?
SonarQube is a code quality and security analysis tool.
It inspects code for bugs, vulnerabilities, and code smells.
Integrates with Jenkins, GitHub, and other CI/CD tools.

#### 100Q. How to integrate GitHub with Jenkins?
Install Git and GitHub plugin in Jenkins.
Create a GitHub Personal Access Token.
In Jenkins, go to Manage Jenkins > Configure System > GitHub and add credentials.
Set up webhooks in GitHub to trigger builds on code push.

#### 101Q. What is the command to check memory in Linux?
free -h

#### 102Q. What is the command to check file size in Linux?
du -sh filename

#### 103Q. How can you search for a word in Linux?
grep 'word' filename
To search recursively in all files: grep -r 'word' /path/to/directory

#### 104Q. What are Shared Libraries in Jenkins?
Shared libraries allow you to reuse common code across multiple Jenkins pipelines.
They are stored in a separate Git repo or directory structure and loaded using:
@Library('library-name') _( in shell script)


#### 105Q. what is the difference between git revert and git reset?
git revert creates a new commit that undoes changes of a specific commit without altering commit history (safe for shared branches).
git reset moves the HEAD and possibly updates the index or working directory (can rewrite history; not safe for shared branches).

#### 106Q. what isthe difference between git fetch and git pull?
git fetch downloads changes from the remote repository but doesn’t apply them.
git pull is equivalent to git fetch followed by git merge; it updates your current branch.

#### 107Q. How do you troubleshoot if an application is running slow on Linux?
Check CPU/memory usage: top, htop, vmstat
Check disk I/O: iostat, iotop, df -h
Check network issues: netstat, ping, traceroute
Check logs: /var/log/syslog, application-specific logs
Check processes: ps aux --sort=-%mem


#### 108Q: What is a Key Pair in AWS?
A:
A Key Pair is used for secure SSH access to EC2 instances. It includes a public key (stored in AWS) and a private key (downloaded by the user).
You create it in EC2 Dashboard → Key Pairs → Create Key Pair, and use it when launching an EC2 instance.


#### 109Q. What is AWS VPC peering?
A: VPC peering is a networking connection between two VPCs that enables routing traffic between them using private IPs. Peering works across regions and accounts but does not support transitive peering.

#### 110Q: What is terraform taint used for?
A: It marks a specific resource for destruction and recreation on the next terraform apply.

#### 111Q: What are Terraform Workspaces?
A: Workspaces allow you to manage multiple state files within a single configuration directory. Useful for managing different environments like dev, staging, and prod.

#### 112Q. Explain Jenkins architecture.
A: Jenkins follows a master-agent architecture.
The master schedules builds, manages agents, and handles web UI.
Agents execute build jobs on different platforms or environments.

#### 113Q. What is the difference between Jenkins agents and labels?
A: Agents are machines (nodes) that run jobs.
Labels are tags you assign to agents to group them for job scheduling (e.g., linux, docker).

#### 114Q. What is a Docker multi-stage build file?
A: Multi-stage builds allow you to use multiple FROM statements in a Dockerfile to separate build-time and runtime environments, reducing final image size.

#### 115Q. What are Kubernetes Stateful Services?
A: Stateful services retain state across restarts. StatefulSets manage pods with stable network identity, persistent storage, and ordered, graceful deployment.

#### 116Q. what is the kubernetes architecture?
o	**Master Node:** Controls the Kubernetes cluster. It contains several components:

o	**API Server:** Exposes the Kubernetes API.

o	**Controller Manager:** Ensures that the cluster is in the desired state (e.g., creating new pods when needed).

•	**Scheduler:** Assigns workloads to nodes.

•	**ETCD:** is a distributed key-value store used to store all cluster data, including configuration data, secrets, and state information.

•	**Worker Node:** Runs the containerized applications. Components include:

•	**Kubelet:** Ensures the containers are running in a Pod.

•	**Kube Proxy:** Maintains network rules for Pod communication.

•	**Container Runtime:** Runs the containers (e.g., Docker).

#### 117Q.What are different types of Kubernetes Services?
A:
**ClusterIP** – Default, internal-only access
**NodePort** – Exposes service on a port on each node
**LoadBalancer** – Uses external load balancer
**ExternalName** – Maps service to external DNS name
**Headless Service** – Created by setting clusterIP: None, used for direct pod access, useful in StatefulSets and DNS discovery.

#### 118Q. Q: What is AWS Serverless and what are its key benefits?
A:
AWS Serverless refers to a cloud-native development model that allows you to build and run applications without managing servers. Key services include:

•	AWS Lambda – run code in response to events.

•	Amazon API Gateway – expose APIs.

•	Amazon DynamoDB – NoSQL database.

•	AWS Step Functions – orchestrate workflows.

**Benefits:**

•	No server provisioning or management.

•	Auto-scaling and high availability.

•	Pay only for what you use (event-driven).

#### 119Q: What AWS resources have you worked with?
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

#### 120Q: What AWS services do you use for scaling instances?
A:
For automatic and manual scaling, I’ve used:

•	**Auto Scaling Groups (ASG):** Automatically add/remove EC2 instances based on CPU, memory, or custom metrics.

•	**Elastic Load Balancer (ELB):** Distributes traffic across instances to balance load.

•	**CloudWatch Alarms:** Used to trigger scaling actions.

•	**ECS with Fargate or EC2:** Task-based scaling based on request load or queue depth.

#### 121Q: Which command is used in Linux to check disk usage?
A:
df -h           **Shows disk usage in human-readable format**

du -sh *        **Shows folder sizes in the current directory**

#### 122Q: How can you delete the contents of a file in Linux without deleting the file itself?
A:
> filename           **Truncates the file**

: > filename         **Same as above**

truncate -s 0 filename  # Explicitly sets file size to 0

#### 123Q. If we get a lock while executing terraform plan, how to unlock it?
 Answer:
Use the following command to manually unlock the Terraform state:
terraform force-unlock <LOCK_ID>
Only use it if you're sure no other process is actively using the state.

#### 124Q. If we get an error while pipeline execution, how do you solve it?
Answer:
•	Check pipeline logs to identify the exact stage and error message.

•	Reproduce the issue locally (if possible).

•	Fix config or script issues (e.g., syntax, credentials).

•	Rerun the pipeline and monitor.

#### 125Q . What are the steps to push your code to a central repo (e.g., GitHub)?
Answer:
git init                                     **# Initialize local repo**

git add .                                    **# Stage changes**

git commit -m "message"                      **# Commit changes**

git remote add origin <repo-url>             **# Link to central repo**

git push -u origin main                      **# Push code**

#### 126Q . How can you create infrastructure at a time in two AWS accounts using Terraform?
Answer:
• Define two different provider blocks with different credentials:

provider "aws" {
  alias  = "account1"
  region = "us-east-1"
  profile = "account1-profile"
}
provider "aws" {
  alias  = "account2"
  region = "us-west-2"
  profile = "account2-profile"
}

• Use provider = aws.account1 and provider = aws.account2 in resources.

#### 127Q. How can we secure Docker containers?
Answer:
•	Use minimal base images (e.g., alpine).

•	Run containers as non-root users.

•	Regularly scan images for vulnerabilities.

•	Use Docker secrets for sensitive data.

•	Enable network and runtime restrictions.

•	Keep Docker and host OS updated.

#### 128Q. count vs for_each in Terraform
Answer:
Feature	                     count	                            for_each
Use Case	            Repetition by number	                  Repetition by map/set
Indexing	             Uses count.index	                  Uses each.key / each.value
Best for	                      Lists                                               	                 Maps or sets (with named keys)

#### 129Q. Inbuilt Terraform functions you use
Answer:
•	length() – get list length

•	lookup() – safe map value lookup

•	join() – join strings

•	split() – split string to list

•	merge() – merge maps

•	format() – formatted strings

•	element() – get item by index

•	file() – read local file

#### 130Q. Difference between ALB and NLB
Answer:
Feature	ALB (Application Load Balancer)	NLB (Network Load Balancer)
Layer	Layer7 (HTTP/HTTPS)	Layer4 (TCP/UDP)
Features	Path-based, host-based routing	Fast TCP handling, static IP
Use Case	Web apps, HTTP APIs	Low latency apps, real-time systems

#### 131Q. How do you connect to a private subnet?
Answer:
•	Use a bastion host (jump box) in the public subnet.

•	Or use Session Manager (SSM) if agents are installed.

•	Optionally use a VPN or Direct Connect.

#### 132Q. AWS Lambda vs AWS Fargate
Answer:
Feature	AWS Lambda	AWS Fargate
Type	Serverless functions	Serverless containers
Use Case	Short event-driven tasks	Long-running container apps
Timeout	Max15 minutes	No hard timeout
Pricing	Per request + duration	Based on vCPU and memory used

#### 133Q. How can you print output line by line in shell scripting?
Answer:
while read line; do
  echo "$line"
done < filename.txt

#### 134Q. What is OAI (Origin Access Identity) in CloudFront?
Answer:
OAI is used to restrict access to an S3 bucket so only CloudFront can fetch content, preventing direct access via S3 URL.

#### 135Q. How can you use if-else in Terraform?
Answer:
Use ternary operator:
variable "env" {}
output "instance_type" {
  value = var.env == "prod" ? "t3.large" : "t2.micro"
}

#### 136Q. What is rollback mechanism in Jenkins?
Answer:
•	Use Git to revert to a previous commit and re-deploy.

•	In pipeline, define a rollback stage to deploy last stable artifact (e.g., using Nexus/S3).

•	Tools like ArgoCD, Ansible, or Helm can assist with rollbacks in CD.

#### 137Q. What is Maven lifecycle?
Answer:
Maven has 3 built-in lifecycles:

•	clean – cleans previous build (mvn clean)

•	default – main build (compile, test, package, install, deploy)

•	site – generates documentation

#### 138Q. How can you skip test phase in Maven?
Answer:
mvn install -DskipTests
Or skip completely:
mvn install -Dmaven.test.skip=true

#### 139Q. Difference between Maven, Ant, and Jenkins?
Answer:
Tool Purpose Key Feature
Maven Build tool Convention over configuration
Ant	Build tool Procedural (manual steps)
Jenkins	CI/CD automation Executes pipelines, integrates tools

#### 140Q. What are Kubernetes namespaces?
Answer:
Namespaces logically isolate resources in a cluster. Example: dev, test, prod environments in the same cluster.

#### 141Q. What is Argo CD?
Answer:
Argo CD is a GitOps tool for Kubernetes. It continuously syncs your Kubernetes cluster state with Git repositories.

#### 142Q. What is Ingress in Kubernetes?
Answer:
Ingress exposes HTTP and HTTPS routes from outside the cluster to services inside using rules and host/path-based routing.

#### 143Q. Difference between Docker and Kubernetes?
Answer:
| **Aspect**               | **Docker**                        | **Kubernetes**                                         |
| ------------------------ | --------------------------------- | ------------------------------------------------------ |
| **Type**                 | Containerization platform         | Container orchestration platform                       |
| **Primary Function**     | Build, ship, and run containers   | Deploy, manage, scale, and orchestrate containers      |
| **Scope**                | Works on a **single host**        | Manages **clusters of nodes** (multi-host)             |
| **Component**            | Uses Docker CLI and Docker Engine | Uses Master (Control Plane) and Nodes                  |
| **Container Management** | Manual or via `docker-compose`    | Automated (via Deployments, StatefulSets, etc.)        |
| **Scaling**              | Manual                            | Auto-scaling supported                                 |
| **Networking**           | Basic networking                  | Advanced service discovery and load balancing          |
| **High Availability**    | Not built-in                      | Built-in redundancy and failover                       |
| **Monitoring & Logging** | Basic logging                     | Supports monitoring/logging with Prometheus, ELK, etc. |
| **Installation**         | Simple, lightweight               | More complex, requires cluster setup                   |

#### 144Q. How many ways can you create Docker images?
Answer:
#### 1.	Using a Dockerfile (recommended)

#### 2.	Using docker commit from a running container

#### 3.	Programmatically with Docker SDK

#### 145Q. What are other lines than shebang in shell scripts?
Answer:
•	Comments (#)

•	Variable declarations

•	Logic/commands (if, echo, loops)

•	Function definitions

#### 146Q. What does a Shebang line represent?
Answer:
Starts with #!, tells the system which interpreter to use. Example:

#!/bin/bash
#### 147Q. How can you modify permissions in Linux?
Answer:
Use chmod:
chmod 755 file.sh
Use chown to change ownership:
 chown user:group file

#### 148Q. What is crontab, and what are allow/deny files?
Answer:
•	crontab schedules jobs.

•	/etc/cron.allow – only users in this file can use crontab.

•	/etc/cron.deny – users listed here cannot use crontab.

#### 149Q. How can you kill a process in Linux?
Answer:
kill <PID>
kill -9 <PID>       #### force kill

#### 150Q. How can you check CPU utilization?
Answer:
Use:
top
htop
mpstat

#### 151Q. How can you check if a port is open and listening?
Answer:
netstat -tuln | grep 80
ss -tuln | grep 80

#### 152Q. How can you check if the network is working using traceroute?
Answer:
traceroute google.com
It shows the path and delays to the destination.

#### 153Q. How can you capture last 20 lines of a file?
Answer:
tail -n 20 filename.log

#### 154Q. What are the features of Jenkins?
Answer:
•	Open-source CI/CD tool

•	Supports pipeline-as-code

•	Plugins for Docker, Kubernetes, Git, etc.

•	Can build, test, deploy automatically

#### 155Q. How can you build pipelines in Jenkins?
Answer:
•	Use Declarative or Scripted pipeline in a Jenkinsfile

•	Use UI to create pipeline jobs and add stages

•	Example:
```
groovy
pipeline {
  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }
  }
}
```
#### 156Q. What is SNS and how do you create it?
A:
SNS (Simple Notification Service) is an AWS service used to send notifications via email, SMS, HTTP, or Lambda.
To create it:

Go to AWS SNS in the console.
Click “Create topic” → Choose Standard or FIFO.
Name the topic and click Create.
To add a subscriber, choose the topic → Create subscription → select protocol (e.g., Email) and provide the endpoint.

#### 157Q. What is a Playbook?
A:
A Playbook in Ansible is a YAML file that defines a set of automation tasks (called "plays") to be executed on remote systems. It is used to configure systems, deploy applications, and manage infrastructure in a repeatable way.
Example use: Installing software, restarting services, or copying files across servers.

#### 158Q. How do you execute commands in Linux?
Answer:
•	Directly in shell: ls, echo, cd

•	Or in script files with .sh extension

•	Use bash script.sh or ./script.sh

#### 159Q. In which environments do you work?
Answer:
•	Dev – Development

•	QA – Testing

•	UAT – User Acceptance Testing

•	Prod – Live/production

#### 160Q. How can you recover a deleted S3 object?
Q: How do you restore a deleted S3 object?
A: If versioning was enabled, I can retrieve the deleted object using a previous version. Without versioning, the object is permanently deleted unless S3 backup (e.g., replication or lifecycle rule to Glacier) is configured.

#### 161Q. How do you differentiate between a public and a private subnet in AWS?
Q: How do you identify a public vs private subnet?
A: A public subnet has a route to the internet via an internet gateway (IGW). A private subnet lacks this route and usually uses a NAT gateway for internet access. I verify this by checking route tables.

#### 162Q. How to recreate infrastructure in Terraform?
Q: How do you recreate Terraform-managed resources?
A: I use terraform taint <resource> to mark a resource for recreation, or terraform destroy followed by terraform apply to recreate everything. I also use terraform state rm if needed to remove a resource from state before recreating.

#### 163Q. What is a remote module in Terraform?
Q: What is a remote Terraform module and how do you use it?
A: A remote module is a reusable configuration hosted in a repo (like GitHub, Terraform Registry). It’s used via a module block with a source URL. 

Example:

module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "3.0.0"
  ...
}

#### 164Q. What are modules in Terraform?
Q: Why do you use modules in Terraform?
A: Modules group and reuse Terraform configurations, promoting clean and DRY code. They help manage large infrastructure by encapsulating resources like VPCs, EC2s, or databases into logical components.

#### 165Q. How can you pass passwords securely in Terraform?
Q: How do you handle sensitive values like passwords in Terraform?
A: I use terraform.tfvars or environment variables and mark them as sensitive in the variable block. For storage, I use tools like AWS Secrets Manager or HashiCorp Vault and access them using data blocks.

#### 166Q. What is the difference between CMD and ARG in a Dockerfile?
Q: What’s the difference between CMD and ARG in Docker?
A: ARG defines variables at build-time, while CMD provides defaults for runtime. 

Example:

dockerfile

ARG VERSION=1.0
CMD ["node", "app.js"]

#### 167Q. How do you secure Docker images?
Q: What steps do you take to secure Docker images?
A: I use minimal base images (like Alpine), scan images with tools like Trivy or Docker Scout, avoid hardcoding secrets, use .dockerignore, and sign images using Docker Content Trust or Notary.

#### 168Q. How do you secure a 3-tier architecture?
Q: How do you secure a 3-tier app (web, app, DB)?
A: I use security groups and NACLs to isolate layers:
•	Web tier: Public subnet with limited inbound (HTTP/HTTPS).
•	App tier: Private subnet, allows traffic only from web tier.
•	DB tier: Private subnet, accessible only by app tier.
Enable encryption (TLS, KMS), IAM roles, and monitoring (CloudWatch, GuardDuty).

#### 169Q. How do you expose your app to the internet in Kubernetes?
Q: How do you expose an application in Kubernetes to the internet?
A: I use a Service of type LoadBalancer or Ingress. For complex routing and HTTPS, I prefer using an Ingress controller (like NGINX or ALB Ingress).

#### 170Q. How do you connect Jenkins to the cloud (AWS)?
Q: How do you connect Jenkins to cloud environments like AWS?
A: I configure Jenkins with AWS CLI/SDK or IAM credentials (via credentials plugin). I install plugins like AWS EC2, use IAM roles (on EC2 agents), and store secrets in AWS Secrets Manager or Jenkins credentials.

#### 171Q. How many VPCs can you create per region?
Answer:
By default, you can create 5 VPCs per region per AWS account. This limit can be increased by requesting a quota increase from AWS.

#### 172Q. What is the difference between a private and public subnet?
Answer:
• **Public Subnet:** A subnet that is associated with a route table that has a route to an Internet Gateway (IGW). Resources in this subnet can access the internet.

•**Private Subnet:** A subnet that does not have a route to the Internet Gateway. Used for internal resources like databases.

#### 173Q. What is a Transit Gateway?
Answer:
An AWS Transit Gateway enables you to connect multiple VPCs and on-premises networks through a central hub, simplifying your network architecture and reducing the number of peering connections.

#### 174Q. What is VPC Peering?
Answer:
VPC Peering allows direct communication between two VPCs in the same or different AWS accounts/regions. It’s non-transitive and is used for point-to-point connectivity.

#### 175Q. What is a VPC Endpoint?
Answer:
A VPC Endpoint allows private connection between your VPC and AWS services (like S3, DynamoDB) without using the internet, improving security and performance.

#### 176Q. How can you restore an RDS snapshot with a custom database name?
Answer:
You cannot directly rename a database when restoring a snapshot. Instead:
#### 1.	Restore the snapshot.
#### 2.	Create a new DB instance.
#### 3.	Use tools like pg_dump/mysqldump, or AWS DMS to export and import data into a DB with the desired name.

#### 177Q. How can you create an HTTPD application on 100 EC2 instances using an Ansible playbook?
Answer:
Define the inventory of 100 EC2 instances.

Write a playbook to install and start HTTPD:

```yaml
---
- hosts: webservers
  become: yes
  tasks:
    - name: Install httpd
      yum:
        name: httpd
        state: present
    - name: Start httpd service
      service:
        name: httpd
        state: started
        enabled: yes
```
Run the playbook: ansible-playbook -i inventory.ini playbook.yml

#### 178Q. How can you troubleshoot if an application is not working on an EC2 instance?
Answer:
•	Check EC2 instance status (Running/Reachable).

•	Verify Security Groups and NACLs (port access).

•	Check application logs (/var/log/, journalctl, etc.).

•	Confirm service status (systemctl status).

•	Check CPU/memory/disk usage.

•	Test network connectivity (ping, telnet, curl).

#### 179Q. How can you connect S3 to an EC2 instance?
Answer:
•	Attach an IAM Role to EC2 with S3 access permissions (e.g., AmazonS3ReadOnlyAccess).

•	Use AWS CLI or SDK on EC2:

aws s3 ls s3://your-bucket-name

#### 180Q. What is the difference between Security Group and Network ACL (NACL)?
Answer:
Feature	Security Group	NACL
Level	Instance-level	Subnet-level
Stateful	Yes	No
Rules	Allow only	Allow and Deny
Applies to	EC2 Instances	Subnets
Default Behavior	Deny all unless allowed	Allow all unless changed

#### 181Q. What are EC2 instance types?
Answer:
EC2 instances are categorized based on their hardware capabilities and use cases. Below is a corrected and properly aligned table:

| **Instance Series** | **Type**                    | **Use Case**                       | **Examples**                |
| ------------------- | --------------------------- | ---------------------------------- | --------------------------- |
| **t-series**        | Burstable general purpose   | Low-cost, spiky workloads          | `t2.micro`, `t3.small`      |
| **m-series**        | General purpose             | Balanced compute, memory, network  | `m5.large`, `m6g.medium`    |
| **c-series**        | Compute optimized           | High-performance compute workloads | `c5.large`, `c6g.xlarge`    |
| **r-series**        | Memory optimized            | In-memory databases, caching       | `r5.large`, `r6g.xlarge`    |
| **i-series**        | Storage optimized           | High IOPS storage workloads        | `i3.large`, `i4i.xlarge`    |
| **g/p-series**      | GPU / Accelerated computing | ML, AI, video processing           | `g4dn.xlarge`, `p3.2xlarge` |


#### 182Q. What should you do if a Pod crashes?
Answer:
•	Check logs: kubectl logs <pod-name>

•	Describe pod: kubectl describe pod <pod-name>

•	Check events and container status for error messages.

•	**Investigate issues like:**
o	CrashLoopBackOff

o	ImagePull errors

o	OOMKilled (Out of Memory)

o	Misconfigurations in YAML (ports, env vars, etc.)

#### 183Q. What are the different scaling strategies in Kubernetes?
Answer:
•	**Manual Scaling:** Using kubectl scale command or editing the deployment.

•	**Horizontal Pod Autoscaler (HPA):** Scales pods based on CPU/memory utilization.

•	**Vertical Pod Autoscaler (VPA):** Adjusts CPU/memory requests/limits.

•	**Cluster Autoscaler:** Automatically adds/removes nodes based on pod needs.

#### 184Q. What are the Deployment strategies in Kubernetes?
Answer:
•	**Rolling Update (default):** Gradually replaces old pods with new ones.

•	**Recreate:** Deletes old pods before creating new ones.

•	**Blue/Green Deployment:** Deploys new version alongside old one, then switches.

•	**Canary Deployment:** Gradually rolls out to a small subset before full rollout.

#### 185Q. How can you pause a container?
Answer:
Kubernetes doesn't directly support pausing containers, but you can:

•	Use kubectl rollout pause deployment/<deployment-name> to pause updates.

•	Use Linux SIGSTOP/SIGCONT signals in advanced container runtime setups.

#### 186Q. What are Init Containers?
Answer:
Init containers are special containers that run before app containers in a Pod. They:

•	Run sequentially.

•	Are used for initial setup tasks (e.g., configs, waiting for DB readiness).

•	Must complete successfully for the main container to start.

#### 187Q. What are Sidecar Containers?
Answer:
Sidecars are helper containers that run alongside the main container in the same pod.

**Examples:**

•	Logging agent

•	Data synchronizer

•	Proxy (like Envoy for service mesh)

#### 188Q. What are the different types of containers in Kubernetes?
Answer:
•	**App Containers:** Primary application logic.

•	**Init Containers:** Run before app containers for setup tasks.

•	**Sidecar Containers:** Provide supporting features (logging, monitoring).

•	**Ambassador Containers:** Help with service communication/proxying.

#### 189Q. How can you troubleshoot if a Namespace is renamed?
Answer:
•	Namespaces can’t be renamed, only deleted and recreated.

•	**If a resource disappears:**

o	Check with kubectl get all --all-namespaces

o	Validate configs still reference the correct namespace.

#### 190Q. What is etcd?
Answer:
etcd is a distributed key-value store used by Kubernetes to store all cluster state data (like config, secrets, nodes, etc.). It must be highly available and backed up.

#### 191Q. What is a StatefulSet?
Answer:
Used to manage stateful applications:

•	Each pod has a persistent identity.

•	Ordered, graceful deployment and scaling.

•	Stable network names and storage (e.g., databases).

#### 192Q. What is a Headless Service?
Answer:
A service with clusterIP: None:

•	Doesn't assign a cluster IP.

•	DNS returns the pod IPs directly.

•	Used with StatefulSets for service discovery.

#### 193Q. What is a ReplicaSet?
Answer:B
Maintains a stable set of pod replicas.

•	Ensures desired number of pods are running.

•	Used by Deployments internally.


#### 194Q. What is a Deployment object in Kubernetes?
Answer:
A Deployment is used to:

•	Manage ReplicaSets

•	Perform rolling updates

•	Rollback to previous versions

•	Scale pods

#### 195Q. What is a DaemonSet?
Answer:
Ensures that a copy of a pod runs on all (or selected) nodes.

**Use cases:**

•	Log collection (e.g., Fluentd)

•	Monitoring agents (e.g., Prometheus Node

#### 196Q. What is GitHub Actions Pipeline?
Answer:
GitHub Actions is a CI/CD automation tool provided by GitHub. It allows you to define workflows in .github/workflows/*.yml files to automate processes like:
Code build
Test
Deployment

**Structure:**

```yaml

name: CI Pipeline
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run build
        run: echo "Build complete"
```
		
#### 197Q. How do you deploy from staging to preprod using CI/CD?
Answer:

Define environments: staging, preprod
Use manual approval (environments protection rules) for preprod.
Sample GitHub Actions deployment step:

```yaml

jobs:
  deploy-staging:
    ...
  deploy-preprod:
    needs: deploy-staging
    environment:
      name: preprod
      url: https://preprod.example.com
    steps:
      - name: Deploy to preprod
        run: ./deploy.sh
```
Use a deployment strategy like:
Use if conditions to check branch or tag.
Use environment with required reviewers in GitHub.

#### 198Q. How can you run a specific task in Ansible?
Answer:
Use tags in the playbook:

```yaml

tasks:
  - name: Install Apache
    apt:
      name: apache2
      state: present
    tags: install
```
Run specific task with:
ansible-playbook site.yml --tags install

#### 199Q. How do you SSH into a remote server and run a command?
Example:

ssh username@ip "ls -l /path/to/dir"

Replace username@ip with the actual user and server IP (e.g., ubuntu@192.168.1.10).

#### 200Q. Bash: How do you print the last word of a sentence?
Question: Write a bash command to print the last word.

Answer:

echo "This is a sentence" | awk '{print $NF}'

$NF = Number of Fields, i.e., last field.

#### 201Q. This is incorrect syntax: Echo | awk -F ‘{pritn($NF)} — Why?
Issue:

Wrong quote characters
pritn is a typo, should be print
-F not needed unless using a specific delimiter
Corrected version:

echo "Hello world" | awk '{print $NF}'

#### 202Q. Bash: Write a script to print the first word of a string
Answer (Script):

#!/bin/bash
read -p "Enter a sentence: " sentence
echo "$sentence" | awk '{print $1}'

Usage:
$ ./firstword.sh

Enter a sentence: Hello from DevOps

Hello

#### 203Q. Ansible Jinja2 template?
- Template files using Jinja2 syntax to dynamically create config files with variables, loops, and conditionals.

#### 204Q. What is dig command?
- dig is a DNS query tool to retrieve DNS records like IP addresses, MX, NS etc. Example: dig google.com.

#### 205Q. What is nslookup?
- nslookup is a command-line tool to query DNS for domain or IP info, older than dig. Example: nslookup google.com.

#### 206Q. What is dnslookup?
- No such standard command. Usually means performing DNS lookup using tools like nslookup or dig.

#### 207Q. What is AWS EKS Node Group?
- A Node Group in EKS is a set of EC2 instances that run Kubernetes workloads. Can be managed (AWS handles) or self-managed.

#### 208Q. What is eksctl?
- A CLI tool to create/manage EKS clusters and node groups easily with simple commands or YAML configs.

#### 209Q. Shell script to list files created 7 days ago in /var/log?
bash
#!/bin/bash
cd /var/log || exit
find . -type f -mtime 7 -print

(Note: Linux tracks modification time, not true creation time.)

#### 210Q. Inode of Lambda function?
- AWS Lambda functions don’t have inodes. Inodes are filesystem metadata; Lambda runs serverless, so no direct inode.

#### 211Q. CloudWatch vs CloudTrail comparison:
-------------------------------------------------------------------------------------
| Feature          | CloudWatch                  | CloudTrail                       |
|------------------|-----------------------------|----------------------------------|
| Purpose          | Monitoring & metrics        | API call logging & auditing      |
| Data Type        | Metrics, logs               | Management API event logs        |
| Use Case         | Performance & alerts        | Security, compliance, auditing   |
| Real-time?       | Yes                         | No                               |
| Retention        | Configurable                | 90 days default + S3 archival    |
-------------------------------------------------------------------------------------
#### 212Q. Terraform null_resource?
- Resource that does nothing but can run provisioners or act on triggers for side effects without managing real infra.

#### 213Q. Terraform data block?
- Reads/extracts info from existing infrastructure or external sources, no resource creation.

#### 214Q. Terraform element function?
- Returns item from list at given index with wrap-around if index > list length.

#### 215Q. How to enable Terraform debug mode?
- Set env var TF_LOG to DEBUG or TRACE:
bash
export TF_LOG=DEBUG
terraform apply

#### 216Q. Shared Libraries in Jenkins?
- Reusable Groovy pipeline code stored in a Git repo, shared across multiple Jenkinsfiles with @Library.

#### 217Q. Different build triggers in Jenkins?
- Poll SCM, Webhooks (GitHub), Periodic builds, Build after other projects, Remote triggers, Manual.

#### 218Q. Jenkins: Run job always on Slave 2?
- Label slave 2 (e.g. slave2) and restrict job to run on that label in job config.

#### 219Q. Ansible Handlers?
- Special tasks triggered by notify, run once at end if notified, often to restart services after config changes.
