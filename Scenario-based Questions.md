#### 1Q. You have static & dynamic web apps using high EC2 + NGINX, causing high cost & low availability. What's your solution?
Answer:

Use S3 + CloudFront for static content.
Run dynamic apps on ECS/EKS/Fargate with auto-scaling.
Replace high EC2s with smaller instances in ASG.
Use ALB instead of standalone NGINX.
Containerize the app for better resource usage.

Result: Lower cost, high availability, easier management.

#### 2Q. One out of 10 microservices in Kubernetes is down. How do you fix it?
Answer:

Run kubectl get pods to find the failed pod.
Use kubectl logs and describe to diagnose.
Check for resource issues or crash errors.
Restart with kubectl rollout restart.
Roll back if a new image/code caused the issue.
Ensure HPA and probes are correctly set.

Result: Service is restored quickly, root cause identified.

#### 3Q. A client wants to implement a new system in 3 months, but your analysis shows it will take 6 months. How would you handle this situation?
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

#### 4Q. How would you assess whether an AI implementation would be beneficial for a specific business process?
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

#### 5Q. What is the SLA of each and every incident in your current organisation ?

A. in our organisation we have 4 types of incidents based on priority and urgency i.e., P1, P2, P3, P4
The SLA is 2, 8, 72 and 96 hours respectively.

#### 6Q. Did you ever be involved in change management, what is the process of creating CRQ's in your current organisation ?

A. Regarding change management, we generally raise a change when ever there is some configuration or patching is being done on production environment servers. 
We have to raise a change and get in approved in CAB (Change Advisory Board) call. In CAB meeting we discuss the potential risks and dependencies involved to implement the change and all the respective stakeholders holders or teams linked to that change are informed about the activity. Later it should be approved by manager or team lead, the multiple level of approvals depends on individual teams and organisations.

#### 7Q.Do you have any idea about the prod and pre pod environment ?

A. Prod environment is generally production environment, this is what the end consumers get to see, like all the websites and links that we usually get accesses like bank websites, Flipkart and Amazon apps.
Pre prod environment is where all the testing is done before going live in production environment. If everything goes well in Pre prod then we push the updates and patches to production environment.


#### 8Q .You have created a Dockerfile for Tomcat, and in that Dockerfile, you have exposed port 8080. You ran this Docker container mapping it to host port 8081. Will the container work or not? If it works, how is it working, and on which port?
Answer:
Yes, the container will work.

Inside the container, Tomcat is running on port 8080 (as defined in the Dockerfile using EXPOSE 8080).

On the host, you mapped it to port 8081 using -p 8081:8080.

So, requests made to http://<host-ip>:8081 will be forwarded to the container's port 8080.

This port mapping allows external access without changing the container’s internal port configuration.

#### Q9. You are writing a Dockerfile for Jenkins and Tomcat, but both use the same port (8080). How do you resolve this port conflict?
Answer:
You have multiple options:

Run them in separate containers and use different host ports:

docker run -p 8080:8080 jenkins
docker run -p 8081:8080 tomcat
Change the internal application port for one of them (e.g., reconfigure Tomcat or Jenkins to run on 8081).

Use Docker Compose with separate services and define distinct port mappings.

#### Q10. One of your colleagues ran a Terraform script that created an encrypted state file stored in DynamoDB. You want to view and use that state file. How do you decrypt it?
Answer:

Terraform state files stored in DynamoDB are not encrypted by Terraform itself.

If encryption is enabled, it's likely via server-side encryption (SSE) using AWS KMS.

**To decrypt and use it:**

Ensure you have IAM permissions to access the DynamoDB table and KMS decryption rights.

Terraform will automatically read and decrypt the state file if you have the correct IAM policies.

You don't manually decrypt; Terraform does it during operations like terraform plan/apply.

#### Q11. You are creating infrastructure using Terraform, and the state file is stored in S3/DynamoDB. One of your team members manipulated the state file. What’s the next step to rectify this?
Answer:

Identify the issue: Run terraform plan to detect any drift.

**Restore from backup:**

If versioning is enabled in S3, restore the previous version of the state file.

Or use a manual backup if available.

Lock the state file using DynamoDB state locking to avoid future manipulation.

Enable access control using IAM and audit logs to prevent unauthorized changes.

#### Q12. You need to deploy a .war file to an already created server using Terraform. How will you do it?
Answer:
Terraform is not ideal for application-level deployment. Combine it with provisioners or configuration tools:

**Approach:**

Use remote-exec provisioner:
```
provisioner "remote-exec" {
  inline = [
    "scp /path/to/app.war user@host:/opt/tomcat/webapps/"
  ]
}
```
Or better, use Ansible or Jenkins for application deployment after infrastructure creation.

#### Q13. Your previous colleague manually created infrastructure from AWS Console. The client asked you to manage that using Terraform. How will you proceed?
Answer:
Use Terraform import:

Write Terraform code that matches the existing infrastructure.

Use terraform import to bring resources under Terraform control:

terraform import aws_instance.example i-1234567890abcdef0
Run terraform plan to verify drift.

Be cautious; Terraform won’t fetch all configuration details, so manual refinement of .tf files is required.

#### Q14. Your manager asks you to establish communication between Ansible Linux server and Windows, and create a file on Windows using a YAML playbook. What’s your approach?
Answer:

Enable WinRM on the Windows machine.

Use Ansible with winrm connection plugin.

**Example YAML playbook:**
```
- name: Create file on Windows
  hosts: windows
  tasks:
    - name: Create a file
      win_file:
        path: C:\temp\example.txt
        state: touch
```		
**Configure inventory:**

```
[windows]
win_host ansible_host=192.168.1.10 ansible_user=Administrator ansible_password=yourpass ansible_connection=winrm ansible_winrm_transport=basic ansible_winrm_server_cert_validation=ignore
```
#### Q15. What is the port number of WinRM?
Answer:

HTTP: 5985

HTTPS: 5986

#### Q16. Your organization has over 100 servers with rotating IP addresses every 15 days. How do you automatically update IPs in Ansible inventory?
Answer:
**Use Dynamic Inventory:**

For AWS, use ec2.py or aws_ec2 dynamic inventory plugin.

**Configure with a dynamic_inventory.yml:**

```
plugin: aws_ec2
regions:
  - us-east-1
filters:
  tag:Environment: production
keyed_groups:
  - key: tags.Name
    prefix: tag
```
	
**Run playbooks like:**
```
ansible-playbook -i dynamic_inventory.yml playbook.yml
```

This ensures your Ansible inventory updates automatically based on EC2 instances’ tags or metadata.
