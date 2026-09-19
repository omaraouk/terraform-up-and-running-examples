# AWS Scalable Web Cluster (Terraform Labs)

This repository contains an automated infrastructure-as-code blueprint to deploy a highly available, auto-scaling web application cluster on AWS using **Terraform**. It follows the architectural patterns laid out in the book *Terraform: Up & Running*.

## Architecture Overview
* **Load Balancer:** An Application Load Balancer (ALB) acting as the single public entry point on port 80.
* **Auto Scaling Group:** A dynamic cluster that scales between 2 and 10 Ubuntu 24.04 LTS instances (`t3.micro`).
* **Web Server:** A simple web server running on port `8080` isolated behind security group wrappers.
* **Access Management:** Secure SSH access configured dynamically via input variables.

---

## Prerequisites

Before running this configuration, ensure you have the following installed and configured:

1. [Terraform CLI](https://hashicorp.com) (v1.3.0 or higher)
2. [AWS CLI](https://amazon.com) installed and authenticated (`aws configure`)
3. An active AWS Account (Eligible for Free Tier)

---

## Inputs

| Name | Description | Type | Default | Required |
| :--- | :--- | :--- | :--- | :---: |
| `server_port` | The port the server will use for HTTP requests | `number` | `8080` | no |
| `ssh_public_key` | The public SSH key string used for instance authentication | `string` | n/a | **yes** |

---

## Outputs

| Name | Description | Example URL |
| :--- | :--- | :--- |
| `alb_dns_name` | The domain name of the entry-point load balancer | `http://amazonaws.com` |

---

## Quick Start Deployment

### 1. Configure your local variables
Because `ssh_public_key` is required and marked as sensitive, create a local variables file named `terraform.tfvars` (this file is ignored by Git to protect your privacy):

```hcl
# terraform.tfvars
ssh_public_key = "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5..."
```

### 2. Run the deployment commands
Execute the following standard workflow sequence in your terminal:

```bash
# Initialize working directory and download AWS providers
terraform init

# Review the execution plan blueprint
terraform plan

# Deploy infrastructure to the cloud (type 'yes' to confirm)
terraform apply
```

### 3. Verification
Once complete, copy the `alb_dns_name` output URL printed on your screen, paste it into your browser, and you should see:
```text
Hello, World
```

---

## Clean Up & Teardown

To avoid incurring ongoing charges against your AWS Free Tier allowance, always destroy the infrastructure when you are finished testing:

```bash
terraform destroy
```
