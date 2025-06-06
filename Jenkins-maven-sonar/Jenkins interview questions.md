#### How can you automatically trigger jobs in a CI/CD pipeline?

Via webhooks, polling SCM, Git push triggers, or GitHub Actions → Jenkins integration.

#### How do you configure AWS security credentials in Jenkins?

Use:

Jenkins Credentials Manager (add AWS keys)

Environment variables

AWS CLI with IAM role on EC2 or IRSA in EKS

#### Can you explain the build stages in Jenkins for your project?

Common stages:

Checkout

Build

Test

Docker Build

Push to ECR

Deploy to Kubernetes (kubectl)

#### What are Jenkins shared libraries and how do they work?

Shared Groovy code (like vars/mySteps.groovy) stored in a Git repo and imported across pipelines using @Library.

#### What are the different types of Jenkins jobs?

Freestyle Job

Pipeline Job (Scripted / Declarative)

Multibranch Pipeline

Folder / Matrix Job

#### Do you work with freestyle projects or scripted pipelines in Jenkins?

Scripted pipelines offer more control; declarative pipelines are preferred for structured, readable, and version-controlled CI/CD.

#### Key difference when configuring a new CI/CD pipeline?
A:
- Jenkins: Customizable but manual setup (Jenkinsfile, agents, plugins).
- GitLab: Easy, tightly integrated with Git, uses .gitlab-ci.yml.

#### What is the difference between Declarative and Scripted Pipeline in Jenkins?
Declarative Pipeline: Uses a more structured and predefined syntax. Easier to write and read, especially for beginners.

Scripted Pipeline: Uses Groovy-based syntax. Offers more flexibility and is suitable for complex logic.

#### How can you upgrade Jenkins?
1, Backup Jenkins (Recommended) 
Backup your Jenkins home directory (usually /var/lib/jenkins):(sudo cp -r /var/lib/jenkins /var/lib/jenkins_backup)

2, Upgrade Jenkins (Using Package Manager)

sudo yum check-update
sudo yum upgrade jenkins
sudo systemctl restart jenkins

#### What is Jenkins Master-Slave Architecture?
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


#### What is SonarQube?
SonarQube is a code quality and security analysis tool.

It inspects code for bugs, vulnerabilities, and code smells.

Integrates with Jenkins, GitHub, and other CI/CD tools.

#### How to integrate GitHub with Jenkins?
Install Git and GitHub plugin in Jenkins.

Create a GitHub Personal Access Token.

In Jenkins, go to Manage Jenkins > Configure System > GitHub and add credentials.

Set up webhooks in GitHub to trigger builds on code push.

#### What are Shared Libraries in Jenkins?
Shared libraries allow you to reuse common code across multiple Jenkins pipelines.

They are stored in a separate Git repo or directory structure and loaded using:

@Library('library-name') _( in shell script)

#### Explain Jenkins architecture.
A: Jenkins follows a master-agent architecture.

The master schedules builds, manages agents, and handles web UI.

Agents execute build jobs on different platforms or environments.

#### What is the difference between Jenkins agents and labels?
A: Agents are machines (nodes) that run jobs.

Labels are tags you assign to agents to group them for job scheduling (e.g., linux, docker).

#### If we get an error while pipeline execution, how do you solve it?
Answer:
•	Check pipeline logs to identify the exact stage and error message.

•	Reproduce the issue locally (if possible).

•	Fix config or script issues (e.g., syntax, credentials).

•	Rerun the pipeline and monitor.

#### How do you connect Jenkins to the cloud (AWS)?
Q: How do you connect Jenkins to cloud environments like AWS?

A: I configure Jenkins with AWS CLI/SDK or IAM credentials (via credentials plugin). I install plugins like AWS EC2, use IAM roles (on EC2 agents), and store secrets in AWS Secrets Manager or Jenkins credentials.

