# **Terraform Mastery: One-Stop Guide for DevOps Engineers**  

This repository serves as a **comprehensive guide** for mastering **Terraform**, covering **setup, core commands, state management, modules, workspaces, and debugging techniques**. It’s designed to help **DevOps Engineers** efficiently manage infrastructure as code (IaC) using Terraform.

---

## **📌 1. Setup & Initialization**  

### **🔹 Install Terraform**  
#### **For Linux & macOS:**  
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
```

### **🔹 Verify Installation**  
```bash
terraform -v
```

### **🔹 Initialize Terraform**  
```bash
terraform init
```
✅ **This command:**  
- Downloads necessary provider plugins  
- Sets up the working directory  

---

## **📌 2. Core Terraform Commands**  

### **🔹 Formatting & Validation**  
```bash
terraform fmt       # Formats Terraform code  
terraform validate  # Validates Terraform syntax  
```

### **🔹 Planning & Applying Infrastructure**  
```bash
terraform plan      # Shows execution plan  
terraform apply     # Deploys resources  
terraform apply -auto-approve  # Deploys without confirmation  
```

### **🔹 Destroying Infrastructure**  
```bash
terraform destroy  # Destroys all managed resources  
terraform destroy -auto-approve  # Without confirmation  
```

---

## **📌 3. Managing Terraform State**  

### **🔹 Check Current State**  
```bash
terraform state list  # Lists all managed resources  
terraform show        # Shows detailed resource info  
```

### **🔹 Manually Modify State**  
```bash
terraform state mv <source> <destination>  # Move resource in state file  
terraform state rm <resource>  # Remove from state (but not from infrastructure)  
```

### **🔹 Configuring Remote Backend (S3 & DynamoDB)**  
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
```bash
terraform init  # Reinitialize with remote backend  
```

---

## **📌 4. Using Variables & Outputs**  

### **🔹 Defining Variables**  
```hcl
variable "instance_type" {
  default = "t2.micro"
}
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

### **🔹 Passing Variables via CLI**  
```bash
terraform apply -var="instance_type=t3.small"
```

### **🔹 Defining Output Values**  
```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```
```bash
terraform output instance_ip
```

---

## **📌 5. Loops & Conditionals in Terraform**  

### **🔹 Using `for_each` for Multiple Resources**  
```hcl
resource "aws_s3_bucket" "example" {
  for_each = toset(["bucket1", "bucket2", "bucket3"])
  bucket   = each.key
}
```

### **🔹 Conditional Expressions**  
```hcl
variable "env" {}
resource "aws_instance" "example" {
  instance_type = var.env == "prod" ? "t3.large" : "t2.micro"
}
```

---

## **📌 6. Terraform Modules (Reusable Components)**  

### **🔹 Creating & Using a Module**  
#### **Step 1: Create a Module**  
```bash
mkdir -p modules/vpc
```
📁 **modules/vpc/main.tf**
```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
```
#### **Step 2: Use the Module in Root Configuration**  
```hcl
module "vpc" {
  source = "./modules/vpc"
}
```
#### **Step 3: Deploy with Modules**  
```bash
terraform init
terraform apply
```

---

## **📌 7. Workspaces for Environment Management**  

### **🔹 Creating & Switching Workspaces**  
```bash
terraform workspace new dev
terraform workspace new prod
terraform workspace select prod
terraform workspace list
```

---

## **📌 8. Debugging & Logs in Terraform**  

### **🔹 Enable Debug Logs**  
```bash
export TF_LOG=DEBUG  
terraform apply 2>&1 | tee debug.log  # Save logs for review  
```

---

## **📌 9. Real-World Terraform Use Cases**  

### **✅ Terraform with Ansible**
📍 **Project:** **Automating infrastructure provisioning and configuration using Terraform & Ansible.**  
🔗 **[Get it here](https://github.com/your-username/terraform-ansible-integration)**  

### **✅ Terraform with GitHub**
📍 **Project:** **Managing GitHub repositories, teams, and settings as code with Terraform.**  
🔗 **[Get it here](https://github.com/your-username/terraform-github-automation)**  

### **✅ Terraform for AWS EKS**
📍 **Project:** **Provisioning a production-ready Kubernetes cluster using Terraform on AWS EKS.**  
🔗 **[Get it here](https://github.com/your-username/terraform-eks-setup)**  

---

## **📌 Final Thoughts**  

Terraform simplifies **infrastructure provisioning**, making it **scalable, repeatable, and manageable**. Mastering Terraform allows DevOps professionals to efficiently automate cloud environments. 🚀  

💡 **Explore, experiment, and optimize your Terraform workflows for production!**  

---

**✅ This updated version:**  
- **Looks professional & structured**  
- **Covers all Terraform essentials**  
- **Includes real-world use cases**  
- **Has interactive formatting for better readability**  

Let me know if you need any refinements! 🎯🚀
