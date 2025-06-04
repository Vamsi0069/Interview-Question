#### What is the difference between for_each and for in Terraform?

for_each is used to create multiple resources (like EC2 instances or subnets) from a map or set.

for is used inside expressions to transform lists or maps (e.g., locals, output, dynamic blocks).

#### How do Terraform modules work and why should we use them?

Modules are reusable sets of Terraform configuration files.

Benefits: Reusability, consistency, maintainability, and organization of code.

#### What is the purpose of the Terraform state file and how is it managed?

.tfstate tracks the current infrastructure.

It should be stored remotely (e.g., in S3) to avoid conflicts and enable team collaboration.

#### Why do we create modules in Terraform and what are their benefits?

They help reuse code, enforce standardization, and reduce duplication.

#### Do you have any experience using functions in Terraform? If yes, explain.

Yes, functions like lookup(), join(), split(), cidrsubnet(), and coalesce() are used to manipulate data during infrastructure creation.

#### How do you avoid conflicts with the Terraform state file? Where do you store it?

Store the state in Amazon S3, and use DynamoDB for state locking to prevent race conditions in team environments.

#### What is DynamoDB and how is it used in Terraform?

DynamoDB is used for state locking to ensure that only one person or process runs terraform apply at a time.

###  How do you import existing cloud infrastructure under Terraform management?

To bring already-created AWS infrastructure (or any cloud resource) under Terraform’s control without recreating it, use the `terraform import` command.

#### Steps:

1. Define the resource block in your Terraform code (without properties for now):

   ```hcl
   resource "aws_instance" "example" {
     # leave blank for now or minimal attributes
   }
   ```

2. Run the import command:

   ```bash
   terraform import aws_instance.example i-0abcd1234efgh5678
   ```

3. After import, run:

   ```bash
   terraform plan
   ```

   → to see differences. Then manually update your code to match the real resource configuration.

---

###  What is the `taint` command in Terraform?

The `terraform taint` command marks a resource as tainted, meaning Terraform will destroy and recreate it during the next `terraform apply`.

####  Use case:

If a resource (like an EC2 instance) is unhealthy or misconfigured but still exists in state, and you want Terraform to replace it, run:

```bash
terraform taint aws_instance.example
```

Then on next `terraform apply`, Terraform will destroy and recreate that instance.

>  Note: In newer Terraform versions (`>= v1.0`), `terraform taint` is deprecated in favor of:

```bash
terraform apply -replace=aws_instance.example
```
####  How to run terraform plan and terraform apply from Jenkins for different environments?

* Use environment-specific `.tfvars` and Jenkins parameters:

```sh
terraform init
terraform plan -var-file=dev.tfvars
terraform apply -var-file=dev.tfvars
```
