# Terraform In One Shot
This repository is your one stop solution for Terraform for DevOps Engineers 

# Terraform Commands - Complete Guide

## **1. Setup & Initialization**
### **Install Terraform**
```sh
# Linux & macOS
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform

# Verify Installation
terraform -v
```

### **Initialize Terraform**
```sh
terraform init
```
- Downloads provider plugins
- Sets up the working directory

## **2. Terraform Core Commands**
### **Format & Validate Code**
```sh
terraform fmt       # Formats Terraform code
terraform validate  # Validates Terraform syntax
```

### **Plan & Apply Infrastructure**
```sh
terraform plan      # Shows execution plan without applying
terraform apply     # Creates/updates infrastructure
terraform apply -auto-approve  # Applies without manual confirmation
```

### **Destroy Infrastructure**
```sh
terraform destroy  # Destroys all managed resources
terraform destroy -auto-approve  # Without confirmation
```

## **3. Managing Terraform State**
### **Check Current State**
```sh
terraform state list  # Lists all managed resources
terraform show        # Shows detailed resource info
```

### **Manually Modify State**
```sh
terraform state mv <source> <destination>  # Move resource in state file
terraform state rm <resource>  # Removes resource from state (not from infra)
```

### **Remote Backend (S3 & DynamoDB)**
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "global/s3/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-lock"
    encrypt        = true
  }
}
```
```sh
terraform init  # Reinitialize with remote backend
```

## **4. Variables & Outputs**
### **Define & Use Variables**
```hcl
variable "instance_type" {
  default = "t2.micro"
}
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

### **Pass Variables in CLI**
```sh
terraform apply -var="instance_type=t3.small"
```

### **Output Values**
```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```
```sh
terraform output instance_ip
```

## **5. Loops & Conditionals**
### **for_each Example**
```hcl
resource "aws_s3_bucket" "example" {
  for_each = toset(["bucket1", "bucket2", "bucket3"])
  bucket   = each.key
}
```

### **Conditional Expressions**
```hcl
variable "env" {}
resource "aws_instance" "example" {
  instance_type = var.env == "prod" ? "t3.large" : "t2.micro"
}
```

## **6. Terraform Modules**
### **Create & Use a Module**
```sh
mkdir -p modules/vpc
```
```hcl
# modules/vpc/main.tf
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
```
```hcl
# Root module
module "vpc" {
  source = "./modules/vpc"
}
```
```sh
terraform init
terraform apply
```

## **7. Workspaces (Environment Management)**
### **Create & Switch Workspaces**
```sh
terraform workspace new dev
terraform workspace new prod
terraform workspace select prod
terraform workspace list
```

## **8. Terraform Debugging & Logs**
```sh
export TF_LOG=DEBUG  # Enable debug logs
terraform apply 2>&1 | tee debug.log  # Save logs
```

---

## Projects

### Terraform with Ansible
[Get it here](https://github.com/LondheShubham153/terraform-ansible-multi-env)

### Terraform with GitHub
[Get it here](https://github.com/Amitabh-DevOps/online_shop/tree/github-action/.github/workflows)

### Terraform to EKS
[Get it here](https://github.com/DevMadhup/Springboot-BankApp/tree/DevOps/Terraform/EKS-Deployment)

## **Final Thoughts**
This README covers all the Terraform commands needed for your **"Terraform in One Shot"** video. Let me know if you need modifications or extra details! 🚀


