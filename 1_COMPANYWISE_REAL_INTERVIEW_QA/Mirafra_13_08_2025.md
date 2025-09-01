## Q) what is terraform init plan and apply ?

   terraform init → Set up Terraform (download providers, prepare modules).
   
   terraform plan → Preview what changes Terraform will make.
   
   terraform apply → Actually make those changes.
   
## Q) write a python code to remove the duplicate i;e a =[1, 2, 2, 1, 5, 8]?
 a = [1, 2, 2, 1, 5, 8]

## Remove duplicates while preserving order
a_no_duplicates = list(dict.fromkeys(a))

print(a_no_duplicates) 
 ## Output: [1, 2, 5, 8]

## Q) what are issued you have faced in ansible playbooks
 Common issues people face when working with Ansible playbooks include:
 YAML syntax errors,Variable issues,Module failures,Inventory/Host connection problems, Idempotency issues

## Q) explain about terraform statefile?
   
   Terraform state file (terraform.tfstate) is Terraform’s record of your infrastructure —
   it maps your config to real resources, tracks their current state, and 
   is required to plan/apply changes correctly. 
   Keep it safe, don’t edit manually, and store it remotely for team use.

## Q) how do you manage if you get terraform statefile conflicts ?

Use a remote backend with state locking (e.g., S3 + DynamoDB, Terraform Cloud) so only
one run modifies the state at a time.
If a conflict occurs, resolve manually by running terraform refresh or editing via terraform state commands,
then re-run with locking enabled.



## Q) what are the terraform commands you have used ?
terraform init – Initialize providers/modules.

terraform plan – Preview changes.

terraform apply – Apply changes.

terraform destroy – Remove infrastructure.

terraform fmt – Format .tf files.

terraform validate – Check syntax and configs.

terraform refresh – Sync state with real resources.

terraform state – View or modify state.

terraform taint / terraform untaint – Force resource recreation.

terraform output – Show output variables.

terraform providers – Show providers used.

terraform workspace – Manage multiple environment


## Q) what are aws services you have used ?
 Common AWS services I’ve used include:
 
Compute – EC2, Lambda, Auto Scaling Groups, Elastic Beanstalk

Storage – S3, EBS, EFS, Glacier

Networking – VPC, Subnets, Route 53, API Gateway, CloudFront, Load Balancers (ALB/NLB)

Databases – RDS, DynamoDB,

Security – IAM, KMS, Security Groups, WAF, Secrets Manager

Monitoring & Logging – CloudWatch, CloudTrail, Config

DevOps Tools – CodePipeline, CodeBuild, CodeDeploy, CloudFormation, Terraform on AWS

Messaging & Integration – SQS, SNS, EventBridge

## Q)In terraform  what is  local provisioners and remote provisioners?
Local provisioner → Runs commands on the machine where Terraform is executed (your local system). Example: local-exec.

Remote provisioner → Runs commands on the resource created by Terraform (e.g., inside an EC2 instance over SSH). Example: remote-exec.


## Q) how do you optimize cost in aws ?
Right-size resources (match instance sizes to workload).

Use Reserved/Spot Instances for predictable or flexible workloads.

Auto Scale to avoid idle capacity.

Turn off unused resources (non-prod at night/weekends).

Use S3 lifecycle policies to move/expire old data.

Monitor with Cost Explorer & Budgets to track and control spending.

Leverage serverless (Lambda, Fargate) for event-driven workloads.


## Q) what kind of deployment strategies you have worked?
 Common deployment strategies I’ve worked with:
 
Rolling Deployment – Gradually replace old versions with new ones.

Blue-Green Deployment – Two identical environments; switch traffic to the new one when ready.

Canary Deployment – Release to a small subset of users first, then expand.

## Q) How do you rollout your deployment in k8s?

In Kubernetes, I roll out deployments using

kubectl apply -f deployment.yaml      # Apply changes  

kubectl rollout status deployment/<name>   # Check rollout progress  

kubectl rollout history deployment/<name>  # View history  

kubectl rollout undo deployment/<name>     # Rollback if needed

Typically with rolling updates to ensure zero downtime.
