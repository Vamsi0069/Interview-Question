#### 1. How can you automatically trigger jobs in a CI/CD pipeline?

Via webhooks, polling SCM, Git push triggers, or GitHub Actions → Jenkins integration.

#### 2. How do you configure AWS security credentials in Jenkins?

Use:

Jenkins Credentials Manager (add AWS keys)

Environment variables

AWS CLI with IAM role on EC2 or IRSA in EKS

#### 3. Can you explain the build stages in Jenkins for your project?

Common stages:

Checkout

Build

Test

Docker Build

Push to ECR

Deploy to Kubernetes (kubectl)

#### 4. What are Jenkins shared libraries and how do they work?

Shared Groovy code (like vars/mySteps.groovy) stored in a Git repo and imported across pipelines using @Library.

#### 5. What are the different types of Jenkins jobs?

Freestyle Job

Pipeline Job (Scripted / Declarative)

Multibranch Pipeline

Folder / Matrix Job

#### Do you work with freestyle projects or scripted pipelines in Jenkins?

Scripted pipelines offer more control; declarative pipelines are preferred for structured, readable, and version-controlled CI/CD.
